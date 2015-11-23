# MongoDB - Artigo
autor: **José Aparecido da Silva Junior**

## Variável Global
Variáveis em Javascript podem pertencer a 2 escopos. **GLOBAL** e **LOCAL**.

O Javascript usa funções para gerenciar escopos.

Funciona assim: uma variável declarada dentro de uma função é local aquela função e não está disponível fora dela. Por outro lado, variáveis globais são aquelas declaradas fora de qualquer função ou que simplesmente são usadas em serem declaradas.

Exemplos:

```javascript
    var global1 = "Olá, sou uma global";
    count = 1;
    myArray = [];

    function minhaFuncao(){
        var local1 = "sou uma variável local da minhaFuncao()";
        console.log(count);
        count++;
    }
```

Uma variável **global** pode ser acessada até mesmo de dentro de uma função, como uma variável local, mas ao alterarmos seu valor (dentro da função) todo o restante da aplicação verá o valor alterado.

O problema das **globais** é que elas são compartilhadas por todo o código em sua aplicação javascript ou sua página web. Elas coexistem no mesmo namespace global e sempre existe a possibilidade de ocorrerem colisões de nomes, por exemplo, quando duas partes separadas de uma aplicação usam variaveis globais de mesmo nome, mas com propósitos diferentes.

Exemplo:

```javascript
    var total = 1;

    incrementa(10);

    console.log(retornaTotal());

    alert(total);

    function incrementa(valor){
        total += valor;
    }

    function retornaTotal(){
        return total;
    }
```

## Hoisting

Ao contrário de outras linguagens, o JavaScript permite que tenhamos múltiplas declarações `var` em qualquer lugar dentro do código, e todas elas agirão como se tivessem sido declaradas no topo de uma função. Esse comportamento é conhecido como `Hoisting`.

`Hoisting` significa hastear, levantar... Ou seja, qualquer declaração de variável feita após o topo do script, é hasteada, levantada, para o topo.

Exemplo:

```javascript

function minhaFuncao(){
    var variavel1 = "variável declarada no inicio da função";
    console.log(variavel1);
    var variavel2 = "variável declarada no meio da função";
    console.log(variavel2);
}

minhaFuncao();

```

O exemplo acima, funciona perfeitamente não é mesmo? Porém isso pode gerar sérios bugs.

Por exemplo:

```javascript
var variavel1 = "variável global";
function minhaFuncao(){
    console.log(variavel1);
    var variavel1 = "variável declarada no meio da função";
    console.log(variavel1);
}
minhaFuncao();
```
Talvez você espere que 'variável global' seja impresso no primeiro `console.log()`, já que a variavel `variavel1` foi declarada globalmente fora do escopo da função, porém sinto decepcioná-lo. Na primeira impressão no console teremos `undefined`, pois, `variavel1` é considerada uma variavel local à função (mesmo tendo sida declarada posteriormente).

Então, para evitar confusões como esta, escolha declarar todas as variáveis que for usar no inicio do escopo.


## Closure
Básicamente um closure é uma função interior que tem acesso a variáveis de uma função exterior. O closure tem acesso a 3 níveis de escopo: seu próprio escopo, o escopo da função que a engloba e ao escopo global.

Exemplo:

```javascript
    console.log(nickName("José","Junior"));

    function nickName(first, second){
        var nickname = "";

        function makeNickName(first, second){
            return nickname = first+"-"+second;
        }

        return makeNickName(first, second);
    }
```


## Variável por parâmetro

As funções no javascript podem ter N parâmetros. Caso na chamada de uma função, alguns parâmetros sejam omitidos, estes serão setatos como `undefined`.
Quando uma variável global é passada como parâmetro de uma função, seu valor não é alterado, pois, mesmo que haja a tentativa, sua passagem é feita por valor e não por referência.


## Instanciação usando uma IIFE

O IIFE significa "Immediately-invoked function expression", mas podemos chamá-lo de função imediata, "autoinvovável" ou "autoexecutável". Como o próprio nome diz, ela executa a função imediatamente depois de criada.

Veja um exemplo:
```javascript

(function(){
    var pokemon = "";
    console.log("executou");
}())
```
Mas qual o benefício de se implementar tal padrão? Encapsulamento! Como já foi explicado acima, no javascript o escopo das váriaveis é gerenciado com funções. Se uma variável é declarada dentro de uma função, ela é LOCAL àquela função. (não vou explicar tudo novamente... hehe)

Então, todas as variáveis que definirmos serão locais para as funções autoinvocáveis, e não teremos que nos preocuparmos  em poluir o espaço global com variáveis temporárias.

Por exemplo, se no exemplo anterior, tentarmos acessar  a variável `pokemon` no escopo global, teremos um erro.

```javascript

(function(){
    var pokemon = "";
    console.log("executou");
}());

console.log(pokemon);
```

É possível também passarmos parâmetros para funções imediatas. Para isto, basta no segundo par de parenteses final, informar o valor a ser passado como parâmetro.

Exemplo:
```javascript

(function(nome){
    var pokemon = nome || "Bulbassauro";
    console.log(pokemon);
}("Pikachu"));

```
Ah! Vale dizer também, que uma IIFE pode ser escrita assim:
```javascript
    (function(){
    var pokemon = "";
    // ...
    }())
```

Ou assim (repare nos parenteses finais):
```javascript
    (function(){
    var pokemon = "";
    // ...
    })()
```

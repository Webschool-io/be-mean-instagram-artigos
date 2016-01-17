# Instanciação de Variáveis em JavaScript
**Autor**: Wellerson Roberto

## Hoisting ##

**Hoisting** (significa "levantar" em português) é um comportamento que acontece ao declararmos uma variável em JavaScript. Independente de onde ela seja declarada, ela sempre é "elevada" pro topo do escopo. Esse comportamento é mais fácil de ser entendido com um exemplo prático:

```
function testarHoisting(){
	console.log(a);
}
```
Chamar a função acima, como é de se esperar, irá disparar o erro "Uncaught ReferenceError: a is not defined", pois de fato nenhuma variável **a** foi declarada.

```
function testarHoisting(){
	console.log(a);
	var a;
}
```
É natural pensar que no exemplo acima o JavaScript iria apontar o mesmo erro, afinal a função é chamada antes da variável **a** ter sido declarada. Mas não, ele irá retornar apenas **undefined** (variável existe mas com valor ainda não definido). Isso acontece por que a declaração da variável **a** foi elevada pro topo de escopo, basicamente, é como se aquele código se tornasse esse:
```
function testarHoisting(){
	var a;
	console.log(a);
}
```

Mas tenha cuidado,o hoisting só acontece com a **DECLARAÇÃO** da variável, nunca com a sua inicialização.
```
function testarHoisting(){
	console.log(a);
	var a = 7;
	console.log(a);
}
```
Embora possa parecer que o exemplo acima retornaria 7 nas duas chamadas ao **console.log**, na verdade irá retornar **undefined** no primeiro e 7 na segunda. Veja como o código de fato se transforma:
```
// É ASSIM QUE ACONTECE
function testarHoisting(){
	var a;
	console.log(a);
	a = 7;
	console.log(a);
}

// NÃO É ASSIM QUE ACONTECE
function testarHoisting(){
	var a = 7;
	console.log(a);
	console.log(a);
}
```

Assim como em variáveis, hoisting também existe com funções. A diferença é que não só a inicialização é "elevada" pro topo do escopo, mas também o seu próprio corpo.

```
testarHoisting();
function testarHoisting(){
	console.log('Be Mean');
}
```
Nenhum tipo de erro acontecerá ao executar o código acima. Será imprimido normalmente "**Be Mean**" como se espera. Isso por que o hoisting levou a função e todo o seu corpo para o topo, antes de qualquer coisa. O código na verdade se torna esse:
```
function testarHoisting(){
	console.log('Be Mean');
}
testarHoisting();
```

## Closure ##

**Closure** (que significa "encerramento" em português) é quando uma função interior tem acesso as variáveis da função exterior, basicamente como se ela se lembrasse do ambiente em que foi criada. Este exemplo pode tornar mais claro:

```
function closure(valor){
    setTimeout(function(){
            alert(valor);
    }, 2000)
}

closure("Alerta!");
```

Mesmo que a função **setTimeout** seja assíncrona, quando ela for executada ela vai saber exatamente qual é a variável "valor", pois ela está "presa" na função **closure**.

Esse método pode ser utilizado para simular encapsulamento no JavaScript:

```
var aluno = function(nome){
    var nome = nome;

    this.getNome = function(){
        return nome;
    }
}

var pedro = new aluno("aluno");
aluno.nome; //PRIVADO
aluno.getNome(); //PUBLICO
```

Graças ao **closure**, a função **getNome** tem acesso a variável da função externa "aluno".

## Variável Global ##

Em JavaScript, toda variável declarada fora de qualquer função se tornará uma variável global, sendo acessível para toda e qualquer outra parte do código.

```
var variavelGlobal1 = "teste";

function exibir(){
    console.log(variavelGlobal1);
}

exibir();
```

No exemplo acima declaramos uma variável global e portanto ela será acessível dentro de qualquer função. Também podemos criar uma variável global apenas omitindo a palavra chave **var** de sua declaração.

```
function teste(){
    variavelGlobal2 = "a";
}

teste();

console.log(variavelGlobal2);
```

É importante ressaltar que toda variável global do JavaScript se torna uma propriedade do objeto **window**. Por isso, também conseguimos criar uma variável global assim:

```
window.variavelGlobal3 = "a";
```

Assim como acessá-la através do objeto **window**:
```
console.log(window.variavelGlobal3);
```

## Variável por parâmetro ##

Quando uma variável global é passado por parâmetro para uma função, pode-se ocorre duas coisas: passagem de parâmetro por referência ou por valor. Passagem de referência por valor acontece com os tipos primitivos (string, número) e passagem de parâmetro por referência acontece com objetos e arrays.

### Passagem por valor ###

Nesse caso, a variável global irá permanecer a mesma independente da variável passada no parâmetro ser alterada. Isso por que, no momento da passagem, o JavaScript irá copiar o **valor** da variável e criar uma nova instância com esse valor.

```
var variavel = 10;

function passagem(valor){
    valor = 15;
    console.log(15);
}

passagem(variavel);
console.log(variavel);
```

Nesse caso, o programa irá exibir **15** e depois o valor **10** da variável global, mostrando que ela permaneceu a mesma.

### Passagem por referência ###

Já com objetos o JavaScript faz passagem por referência. Em vez de copiar os valores e criar um objeto novo, ele simplesmente copia o endereço da memória onde se encontra o objeto passado e passa para o parâmetro. Basicamente, são duas variáveis apontando pro mesmo lugar na memória. Ou seja, qualquer alteração que ocorrer na variável dentro da função, também irá alterar a variável global.

```
var objetoGlobal = {nome: "Pedro"};

function alterarGlobal(objeto){
    objeto.idade = 15;
    console.log(objeto.idade);
}

alterarGlobal(objetoGlobal);
console.log(objetoGlobal.idade);
```

Observe que o "objetoGlobal" agora possui a propriedade "idade", pois a referência dentro da função adicionou essa nova propriedade no objeto global.

É importante ressaltar que toda e qualquer parâmetro de uma função só é acessível dentro dela, e nunca fora.

## Instanciação usando uma IIFE ##

**IIFE** é a sigla pra "Immediately-invoked function expression", que significa exatamente isso, uma função que é executada assim que é criada. Uma **IIFE** normalmente tem a seguinte estrutura:

```
(function(){
        alert("Teste");
})();
```

O uso de **IIFE** normalmente é feito para se evitar sujar o escopo global e evitar que suas variáveis possam colidir com outras de mesmo nome. Um dos usos no qual eu pude observar foi na criação de plugins em **jQuery**. É recomendado o uso do **IIFE** para que não ocorra possíveis colisões com outras bibliotecas que fazem o uso do símbolo **$**. Então, a estrutura básica é mais ou menos essa:

```
(function($){
        console.log($);
})(jQuery);
```

Dessa maneira, o objeto **jQuery** é passado como parâmetro para a **IIFE**, cujo parâmetro é **$**. Portanto, localmente naquela função, sabemos que a variável **$** será uma referência ao objeto **jQuery**.

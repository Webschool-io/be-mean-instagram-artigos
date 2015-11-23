# Artigo
**autor**: Felipe R S Abbud

## Hoisting

Hoisting nada mais é do que a elevação de variaveis ou funções para o topo de seu escopo atual. Antigamente nas linguagens, por exemplo C, as variavies sempre tinham que ser declaradas na frente para não ter erro no código. Por um bom tempo esse método de declarar variaveis e funções funcionou, mas com o tempo programadores queriam deixar o código mais fácil de ler. Agora o script é lido por completo para ver quais funções e variáveis você declarou e só depois ele seja executado. O hoisting acontece por que o Javascript não obriga você a declarar as funções ou variáveis no começo do script como acontecia antes, isso por que ao ler seu script, todas as variáveis e funções são automaticamente movidas para o topo do seu escopo atual sem que você se preocupe.

## Hoisting de variáveis
Toda e qualquer variável é declarada e levantada ao seu escopo atual com o valor de undefined:

```js
    function example(){
        console.log('Antes de declarar o valor é ' + variavel);
        var variavel = 20;
        console.log('Depois de declarar o valor é ' + variavel);
    }
```

No código acima você verá que o resultado do primeiro console.log é undefined, isso por que a variável ao ser inicializada depois, foi somente declarada como undefined, e depois de atribuído um valor a variável tera o valor de 20. Abaixo como que o console vai interpretar esse código.

```js
     function example(){
        var variavel;
        console.log('Antes de declarar o valor é ' + variavel); // Antes de declarar o valor é undefined

        var variavel = 20;
        console.log('Depois de declarar o valor é ' + variavel); // Depois de declarar o valor é 20
    }
```
Para não ter complicações e confusão de código, tente declarar suas variáveis antes de ser usadas.

 ## Hoisting de funções
 Funciona basicamente igual ao hoisting de variáveis, a única diferança é que somente funções literais são levantadas ao topo do escopo atual onde são declaradas, se por acaso essa função for atribuida a uma variável, essa variável ao ser levantada vai ter o valor undefined, conforme foi visto no exemplo anterior.

```js
 function example(){
    num1 = 5;
    num2 = 10;

    return sum();

    function sum(){
        return num1 + num2;
    }
 }
```
 Aqui, mesmo com o return sendo antes da função, o hoisting é usado e a função literal é levantada para o topo do seu escopo atual, ao invocar  example(), será mostrado o resultado da função sum, que será a soma dos dois números. Funções literais vão ser disponíveis em todo o escopo local que ela for definida.

```js
  function example(){
    function sum(){
        return num1 + num2;
    }

    num1 = 5;
    num2 = 10;

    return sum();    
 }

```
E se a função for atribuída a uma variável?

```js
   function example(){
    num1 = 5;
    num2 = 10;

    return sum();

    var sum =function sum(){
        return num1 + num2;
    }
 }

```

O javascript vai interpretar isso como:

```js
    function example(){
    num1 = 5;
    num2 = 10;
    var sum;

    return sum();

    sum = function sum(){
        return num1 + num2;
    }
 }
```

Isso vai trazer no console que a função sum não existe("sum is not a function"), por que o javascript joga todas as variáveis para o topo do escopo, a variável sum
fica com o valor de undefined, vai retornar como se fosse uma função sum(), que não existe e somente depois de retornar o erro é que sum vai ser inicializada com o valor da nossa função sum.

Basicamente é assim que funciona o Hoisting com variáveis e funções.


## Closure
Closure são funções interior que tem acesso a variáveis da função exterior e seus parâmetros, do escopo global e do seu próprio escopo.
Possuem 3 cadeias de escopo:
1º - O seu próprio escopo - variáveis declaradas no seu próprio escopo;
2º - Escopo da função exterior- onde tem acesso a váriaveis e parâmetros da função exterior;
3º - Escopo Global - onde consegue acesso a variáveis globais.

```js

function course( name, level){
    var teacher = "Suissa";
    function fullCourse(){  // função que consegue ter acesso ao escopo da função exterior course() pegando suas variaveis e parâmetros.
        return 'O curso ' + name + ' com o professor ' + teacher + ' é ' + level + '!';
    }

    return fullCourse();
}

course( "Be-mean", "FODA DEMAIS");
```

## Variável Global
Variável Global nada mais é do que variáveis declaradas fora do escopo de uma função que podem ser acessadas por funções desde que não sejam declaradas novamente dentro do escopo da função que vaiser usada.

```js
var a = 20;

function globalVar(){
    console.log( a );
}

globalVar(); // 20

function localVar(){
    var a = 15;
    console.log( a );
}

localVar(); // 15
```
Automaticamente ao ser declarada dentro de uma função, a variável se transforma em variável local e não mais global.

** Variáveis declaradas fora da função são variáveis globais, mas variáveis declaradas dentro de funções sem o 'var' também são variáveis globais.

```js
function myFunction (){
    number = 1
    return number;
}

function myFunction2 (){
    return number;
}
```
Nesse caso, ao ser declarada a variável number sem o 'var' dentro da função myFunction(), ela pode ser acessada pela função myFunction2(), sendo assim uma variável global( podendo ter muitos problemas com isso!). Para não haver erros comsuas funções e variáveis, declare sempre com o 'var' na frente, separando variáveis globais e locais.


## Variável por parâmetro

Explique o que acontece dentro da função qnd um parâmetro é passado e também explique quando uma GLOBAL é passada por parâmetro.


## Instanciação usando uma IIFE

Explique como uma variável pode receber um valor de uma IIFE.
Explique como passar uma variável por parâmetro para a IIFE e acontece com ela dentro da função.


## Considerações

Quanto mais explicado melhor.

Lembre que isso fará parte do seu currículo como aluno e será disponilizado no sistema de vagas, ou seja, o contratante poderá ver todos seus projetos e trabalhos feitos nesse curso.

Boa sorte.

# Envio

1. Fork [esse repositório](https://github.com/Webschool-io/be-mean-instagram-artigos/) 
2. Nomeie seu artigo usando o seguinte padrão: artigo-instanciacao-githubuser-nome-completo.md
3. Adicione seu exercício aqui na Pasta Variables
4. Faça um Pull Requst enviando seu artigo.


# Artigo
**autor**: Roland Gabriel

Explique, com teoria e código, nesse artigo como o JavaScript cria e instancia as variáveis, seguindo os seguintes tópicos.

## Hoisting

Em JavaScript, funções e variáveis são hoisted (ou "levados ao topo"). Hoisting é um comportamento do JavaScript de mover declarações para o topo de um escopo (o escopo global ou da função em que se encontra).

Isso significa que você é capaz de usar uma função ou variável antes mesmo de tê-las declaradas, ou em outras palavras: uma função ou variável podem ser declaradas depois de já terem sido utilizadas.
```javascript
foo = 2
var foo;

// é implicitamente entendido como:

var foo;
foo = 2;
```

## Closure
Uma *closure* é um tipo especial de objeto que combina uma função e o ambiente no qual essa foi criada. 
O ambiente consiste em qualquer variável que estava no escopo no momento que a closure foi criada. 
 
Closures podem ser usadas para amarrar determinados dados a uma função, sendo esses dados de certa forma configuráveis 
devido ao fato de todo o escopo interno ser armazenado em um objeto que executará a closure.

```javascript
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
};

var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2));  // 7
console.log(add10(2)); // 12
```

1. Closures são particularmente relacionadas com programação baseada a eventos, 
ou seja, é possível configurar uma ação que vai ser a reação do sistema a uma ação do usuário. 

2. Podem usadas também para encapsular funções privadas.





## Variável Global

Qualquer variável instanciada fora de uma função é tratada como variável global perante às funções naquele arquivo js. 
```js
var yourGlobalVariable;
function foo() {
    // ...
}

```


## Variável por parâmetro

Parâmetros primitivos são passados por valor, porém se um objeto é passado para uma função esse é passado por referência sendo, então, visível externamente a alteração feita dentro da função que a chamou.

O mesmo acontece com uma variável global, sendo uma alteração a um objeto vista por todas as funções no arquivo. 



## Instanciação usando uma IIFE
```
Explique como uma variável pode receber um valor de uma IIFE.
Explique como passar uma variável por parâmetro para a IIFE e acontece com ela dentro da função.
```
IIFE é uma forma de modularizar variáveis privadas com acesso via métodos, assim como pode ser uma maneira de evitar *hoisting*. 
* 1. Uma variável pode receber um valor de uma IIFE executando a função que foi retornada pela IIFE em questão.
```js
var counter = (function(){
  var i = 0;

  return {
    get: function(){
      return i;
    },
    set: function( val ){
      i = val;
    },
    increment: function() {
      return ++i;
    }
  };
}());

var result = counter.get(); // 0
```
Na declaração `var result = counter.get();` a variável *result* executa a função retornada pela IIFE para ser atribuída com o valor interno a IIFE.
* 2. Como a estrutura de uma IIFE é composta de uma função dentro de uma expressão, como segue: 
```js
(function(a, b) {
  // a == 'hello'
  // b == 'world'
})('hello', 'world');
```
para passar uma variável para dentro da IIFE basta que passemos como parâmetro da expressão que envolve a que está sendo executada. 

## Referências
[MDN](https://developer.mozilla.org)

[Ben Alman](http://benalman.com/news/2010/11/immediately-invoked-function-expression/)

Wikipedia

StackOverflow


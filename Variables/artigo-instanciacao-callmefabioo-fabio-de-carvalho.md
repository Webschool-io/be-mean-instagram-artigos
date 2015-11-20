# Artigo - Instanciação de variáveis no Javascript
**Autor**: Fábio de Carvalho

Neste artigo, iremos abordar alguns temas importantes sobre como o Javascript trata a instanciação de variáveis ou funções através de alguns mecanismos realmente interessantes. O primeiro deles é o **Hoisting**.

## Hoisting
Seguindo o glossário da [Mozilla](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting), hoisting é:

> "JavaScript's behavior of moving declarations to the top of a scope (the global scope or the current function scope)."

Ou seja, **hoisting** é o comportamento de mover uma variável ou função para o topo do escopo, ele podendo ser global ou o de uma função.

#### Variable Hoisting
Por padrão, todas as variáveis ou funções **declaradas** são *hoisted*. Com as variáveis é bastante interessante, e importante de saber que elas são movidas para o topo do escopo e não recebem valor algum. São *undefined*. Vamos a um exemplo para explicar o porquê deste comportamento.

```js
console.log(x);
var x = 5;
```
[Exemplo no JS Bin](http://jsbin.com/kadazag/edit?js,console,output)

A saída foi *undefined*, correto? Isso acontece exatamente por causa do hoisting. Mas *"behind the scene"*, o JavaScript interpreta o exemplo acima dessa maneira:

```js
var x;
console.log(x);
x = 5;
```

Repare que somente a **declaração** da varíavel foi para o topo do escopo, não sua inicialização. E por isso recebemos como valor o *undefined*.

#### Function Hoisting

O hoisting em funções acontece de uma maneira semelhante, porém com um detalhe: **todo o corpo da função é *hoisted* junto com ela.**

```js
theLastQuestion();

function theLastQuestion() {
  console.log('how to reverse entropy?'); // Resultado
}

var theLastQuestion = function() {
  console.log("42");
}
```
[Exemplo no JS Bin](http://jsbin.com/guxodu/edit?js,console,output)

O mecanismo é o mesmo, porém com detalhe mencionado acima, **todo o corpo da função é movido para o topo do escope junto com ela.** E a maneira que o JavaScript interpreta não é alterada.

Funções também podem ser declaradas como expressões. Assim:

```js

var theLastQuestion = function() {
  console.log("42");
}
```

Desta maneira, eles serão tratadas como variáveis, seguindo as regras de hoisting em variáveis. Apenas sua **declaração** é *hoisted*.

```js
function theLastQuestion() {
  console.log('how to reverse entropy?');
}

var theLastQuestion;

theLastQuestion();

theLastQuestion = function() {
  console.log("42");
}
```

## Closure

**Obs:** Este trecho foi massivamente inspirado no artigo: [JavaScript Closures Demystified](http://www.sitepoint.com/javascript-closures-demystified/). Então tire um tempo para lê-lo, vale muito a pena.

Bom, deixe-me explicar. **Closures** são basicamente funções que residem dentro de outras funções. Confuso, não é mesmo?
Vamos entrar em detalhes, porque o assunto é interessante.

Um closure é uma função criada dentro de uma outra função, e possui a capacidade de acessar diferentes escopos, que são: **seu próprio escopo, o escopo da função "pai" e o escopo global**, possibilitando o acesso à variáveis desses escopos.

Um exemplo de Closures em Javascript:
```
function add(x) {
  return function sum(y) {
    return x + y;
  };
}

var add = add(1);
var addAgain = add(2);
console.log(addAgain);
```
[Exemplo no JS Bin](http://jsbin.com/sudaka/edit?js,console,output)

Closures são úteis para uma série de coisas, como por exemplo o conceito de encapsulamento. Infelizmente o Javascript não é totalmente orientado a objetos, por isso ele não tem suporte a dados privados, ou protegidos. Mas é possível emular este comportamento usando closures.

Uma vez que as variáveis do ambiente de referência são acessíveis apenas a partir de um closure, elas são essencialmente de dados privados.

```
function Person(name) {
  var _name = name;

  this.getName = function() {
    return _name;
  };
}

var person = new Person('Goku');

person._name = 'Vegeta';

console.log(person.getName());
```
[Exemplo no JS Bin](http://jsbin.com/rohure/edit?js,console,output)

Mesmo você tendo a capacidade de alterar a variável "_name" manualmente, é garantido que o valor retornado será o valor passado como parâmentro no construtor. E assim nós emulamos o comportamento *private* de linguagens com maior suporte à encapsulamento.

## Variáveis Globais

Para uma variável ser considerada global, ela só precisa ser declarada fora do escopo de uma função. E em qualquer parte da sua aplicação elas podem ser acessadas.

Exemplo de variáveis globais:
```
// Escopo global
var sayajin;

getSuperSayajin();

function getSuperSayajin() {
  sayajin = 'Goku'

  console.log(sayajin);
}
```
[Exemplo no JS Bin](http://jsbin.com/qajeqit/edit?js,console,output)

## Variáveis por parâmetro

Uma variável quando passada por parâmetro de uma função, ela será utilizada somente dentro do escopo dequela função, o que é chamado de: **escopo local**.

```
log(1); // 1

function log(x) {
  console.log(x);
}

console.log(x); // "ReferenceError: x is not defined
```

Porém, quando uma variável global é passada por parâmetro e seu valor é modificado, esta alteração de valor pertencerá somente dentro do escopo da função, não alterando o valor original da variável global.

```
var x = 1;

log(x);

function log(x) {
  console.log(x);
  x = 2;
  console.log(x);
}
console.log(x);
```
[Exemplo do JS Bin](http://jsbin.com/mitolo/edit?js,console,output)

## Instanciação usando uma IIFE
O termo IIFE significa `Immediately-Invoked Function Expression` e define uma função que é executada no momento da sua definição.
É bastante utilizado para criar escopos que não deixe `"vazar"` as varíaveis para o escopo global da aplicação.

Uma variável pode receber um valor de uma IIFE da seguinte forma:

```
(function() {
    var x  = 5;
    console.log(x);
}());

console.log(x); // "ReferenceError: a is not defined
```
[Exemplo do JS Bin](http://jsbin.com/qiyoxu/edit?js,console,output)

Para passar variáveis por parâmetro em uma IIFE, ela deve ser colocada na última abertura de parênteses da sintaxe, assim:

```
(function (x) {
  console.log(x);
}('Parâmetro passado lindamente!'));
```
[Exemplo do JS Bin](http://jsbin.com/sexeso/edit?js,console,output)

#### Referências

##### Hoisting
[https://developer.mozilla.org/en-US/docs/Glossary/Hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
[http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/](http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/)
[http://www.sitepoint.com/demystifying-javascript-variable-scope-hoisting/](http://www.sitepoint.com/demystifying-javascript-variable-scope-hoisting/)

##### Closures
[http://www.sitepoint.com/javascript-closures-demystified/](http://www.sitepoint.com/javascript-closures-demystified/)
[http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/](http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/)

##### IIFE
[http://benalman.com/news/2010/11/immediately-invoked-function-expression/](http://benalman.com/news/2010/11/immediately-invoked-function-expression/)


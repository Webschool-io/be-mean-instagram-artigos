# Artigo - Instanciação
**Autor**: Eduardo Pinheiro Quaresma Castro - [edupinheiro](https://github.com/edupinheiro)
**Data**: Wed Feb 03 2016 17:02:47 GMT-0300 (BRT)

## Hoisting

Antes de falar sobre hoisting, é bom saber sobre escopo.
Escopo é o local onde as variáveis e expressões serem válidas no código. No Javascript, o escopo é criado em declarações funções.
```js
//escopo global
var a = 0;

function bee(){
    //escopo criado por uma função
    var b = 0;
}
```

Hoisting é "elevar" a declaração de uma variável ou uma função e seu 'corpo' para o topo do escopo.

## Hoisting com variáveis
Quando criamos uma variável, sua **declaração** é *hoisted*, isto é, a declaração da variável vai para o topo do escopo antes mesmo do código ser executado, mas com valor *undefined*, pois não é elevado sua declaração.

```js

try {
  console.log(a)
} catch (e) {
  console.error('A variável `a` não foi declarada.')
}

try {
  console.log(a)
  var a = 2
} catch (e) {
  console.error('A variável `a` não foi definida.')
}
```
No primeiro 'try', o comando 'console.log' tentará imprimir a variável 'a', mas ela não foi declarada. Assim, imprirá o erro dentro do 'catch'.

Já no segundo 'try', a variável 'a' é declarada e inicializada. Mesmo após o comando 'console.log', o *hoisting* 'subirá' a declaração de 'a' para o início do seu escopo, tornando-a visível para o comando 'console.log'. O output será <code>undefined</code>, visto que apenas a declaração de uma variável é hoisted, e não sua declaração.

## Hoisting com funções
Com uma função, o *hoisting* acontece de maneira diferente: seu nome **e** seu 'corpo' são *hoisted*.
```js
function bee(){
  function baa() {
    return 2
  }

  return bae()

  function baa() {
    return 5
  }
}

console.log(foo())
```
Nesse caso, o output é 5. Como o nome da função e seu corpo são levantados para o início do escopo, a primeira declaração da function baa() é hoisted. Após o return, a function baa() é novamente declarada, logo, é hoisted também. Por ser declarada por último, ela sobrescreve o valor da primeira declaração, retornando o valor 5.

Uma função pode ser declarada como uma expressão, mas dessa forma, será tratada como uma variável pelo hoisting.
```js
bee();
var bee = function(){}
```
Nesse exemplo, será retornado um erro do tipo 'TypeError', avisando que 'undefined' não pode ser usado como uma função.

## Closure

Closure (em português, 'Clausura') se refere ao modo como funções definidas em um escopo (ou contexto léxico, como algumas fontes o chamam) acessam variáveis definidas nesse escopo.

Em javascript, apenas Funções podem definir novos escopos, visto que algumas linguagens tem suas prórias regras a respeito de closures. Algumas nem as suportam.
```js
//contexto global
var x = 1;

function bee(){
  // contexto particular desta function
  var y = 5;
  }
```
Cada novo contexto criado dentro de um contexto já existente, tem acesso à variáveis já criadas no contexto externo, ou seja, cada function tem acesso às variáveis declaradas em seus contextos mais externos.
```js

var x = 1; // variável global

function bee(){ // function 'bee' tem acesso à 'x' e a 'y';
  var y = 5;
  function baa(){ // function 'baa' tem acesso à 'x', 'y' e 'z'
    var  z =  4;
  }
}
```
Vale ressaltar que funções internas (confinadas, nesse caso, "enclausuradas"), só podem ser executadas dentro do escopo na qual foram criadas.

## Variável Global
Variáveis globais são variáveis declaradas no escopo global, isto é, são declaradas fora de funções. Tendo isto em mente, podemos afirmar que variáveis globais são acessadas por qualquer função declarada dentro do escopo global, pois são declaradas no contexto léxico(escopo) 'externo' ao criado nas funções.

```js
var x = 1; // variável global

function bee(){ // function tem acesso à 'x' e a 'y';
  var y = 5;
}
```

Todas as variáveis que não são decladaras com a palavra reservada 'var', são consideradas pertencentes ao escopo global. São consideradas variáveis globais, mesmo estas com sua declaração dentro do escopo de alguma função.
```js
var x = 1; // variável global

function bee(){
  var y = 5; //variável da função 'bee'
  z = 3;     // variável global, pois não foi declarada junto com a palavra reservada 'var'
}
```

Existem muitas vantagens ao se utilizar closures. Por exemplo, implementar *currying*, simular variáveis privadas, e o clássico *acumulador*
## Variável por parâmetro
Quando uma função é criada, temos a opção de passar variáveis por parâmetro para elas. Estas variáveis recebem valor apenas na hora de execução destas funções e seus valores são apenas utilizados dentro destas funções.
```js
function soma(n1, n2){
 console.log(n1+n2);
}
soma(7, 2) // a saída será 9
```

Caso se utilize uma variável global por parâmetro, esta não se altera.
```js
var a = 10;
function soma(a, n2){
 console.log(a+n2);
}
soma(7, 2) // a saída será 9
console.log(a); // o valor continua 10
```

## Instanciação usando uma IIFE
IIFE nada mais é que uma Expressão de Função Invocada Imediatamente. Essa função é executada no instante que é interpretada.
```js
(function(){
 // escopo da função
}())
```

IIFE's também podem ser armazenadas em uma variável.
```js
var x = (function(){
 // escopo da função
}())
```

Os parênteses que envolvem a função a tornam uma expressão, e os parênteses no final desta função são responsáveis por executá-la.
Toda IIFE pode sim receber parâmetros, mas eles são passados nos parênteses responsáveis pela execução desta IIFE.
```js
var idade = (function(idade){
 // escopo da função
 console.log("Idade do escritor deste artigo: " + idade); // 20
}(20)) // parâmetro passado para a IIFE
```

Com IIFE's o código é modularizado, deixa tudo mais organizado, além de evitar poluimento de código.
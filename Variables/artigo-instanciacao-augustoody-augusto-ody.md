# Artigo - Instanciação
**Autor**: Augusto Ody - [augustoody](https://github.com/AugustoOdy)

Artigo sobre instanciação em Javascript, abrangendo Hoisting, Closures, Variáveis Globais, Variáveis por parâmetro e IIFE.

## Hoisting


No Javascript, variáveis e funções podem ser utilizadas antes de sua instanciação ou definição. Este comportamento é chamado de Hoisting, onde o Javascript move apenas a declaração destes elementos para o topo de um escopo, sendo este global ou local.

No momento em a variável é "levada" ao topo do escopo ela e inicializada com `undefined`,

Variáveis:

```js
foo = 2
var foo;

// é implicitamente entendido como:

var foo;
foo = 2;
```

Funções:

```js
hoisted(); // logs "foo"

function hoisted() {
  console.log("foo");
}
```


## Closure


Closure é uma função interior que tem acesso às variáveis de uma função exterior. As closures possuem três cadeias de escopo: seu próprio escopo, o escopo da função exterior e o escopo global. A função interior tem acesso não somente as variáveis da função exterior, mas também aos seus parâmetros.

```js
function showName (firstName, lastName) {
	var nameIntro = "Your name is ";

	//esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
	function makeFullName () {
		return nameIntro + firstName + " " + lastName;
	}

	return makeFullName ();
}

showName ("Michael", "Jackson"); //Your name is Michael Jackson
```


## Variável Global


Variáveis no Javascript podem possuir dois tipos de escopo, global ou local. Uma declaração fora de uma função é considerada uma variável global, autorizando a auteração do seu valor em qualquer local da apliação.
Variáveis dentro de uma função são consideradas locais. Seu ciclo de vida acompanha a execução da função.

```js
// Declaração global
var aCentaur = "a horse with rider,";


function antiquities(){
  // Declaração local
   var aCentaur = "A centaur is probably a mounted Scythian warrior";
}

antiquities();
aCentaur += " as seen from a distance by a naive innocent."; // Concatenação com o valor Global
```

No exemplo anterior, caso dentro da função não fosse utilizada a palavra reservada `var`, seria acessada a sua declaração global. Modificando o seu valor. Outro caso seria se além da instaciação, fosse feito um `alert()` utilizando a variável `aCentaur`, a linguagem priorizaria o escopo local.


## Variável por parâmetro


Com a chegada do ECMAScript 6, foram determindados dois tipos de parâmetros:


#Parâmetros padrão


Os parâmetros são setados como `undefined` por padrão nas funções em Javascript. Mas pode ser necessário ser utilizado um valor padrão para algum destes parâmetros. Com isto este tipo de parâmetro possibilita a utilização abaixo:

```js
function multiplicar(a, b = 1) {
  return a*b;
}

multiplicar(5); // 5
```

Sem parâmetros padrão, a verificação se o parâmetro foi setado teria de ser realizada corpo da função. Agora simplesmente é colocado `1` como valor padrão para b no campo de declaração de parâmetros.


#Parâmetros rest


A sintaxe dos parâmetros rest permite que seja representado um valor indefinido de argumentos como um array. É utilizada a sintaxe `...nome_do_argumento` para que seja representado o conjunto de valores passados.

```js
function multiplicar(multiplicador, ...args) {
  return args.map(x => multiplicador * x);
}

var arr = multiplicar(2, 1, 2, 3);
console.log(arr); // [2, 4, 6]
```

## Instanciação usando uma IIFE


O termo IIFE se refere a `Immediately-Invoked Function Expression`. São funções executadas no momento que são definidas. São pricipalmente utilizadas para criação de um escopo local para a situação em que ela se encontra.
Este tipo de estratégia é possibilita que não sejam criadas varias variaveis no escpo global.

```js
(function () {
  'use strict'

  var sayHi = 'oi'
  console.log(sayHi) // oi
}())

console.log(sayHi) // ReferenceError: sayHi is not defined
```

# Instâncias em Javascript
**autor**: Romulo de Araújo Mourão

## Hoisting
Quando declaramos uma variável ou função em Javascript, sua delaração é "elevada" para o topo do escopo. Esse comportamento é o chamado Hoisting

### Variable Hoisting
Toda vez que uma variável é definida, sua declaração é elevada (hoisted), mas sua inicialização não. Ou seja, antes de executar o código a declaração vai pra o topo do escopo, porém ela não recebe nenhum valor e permanece como undefined

```
//EXEMPLO 1:
try {
    console.log(foo);
} catch (e) {
    console.error('variável `foo` não definida.');
}
// Cai no catch porque não foi declarada a variável
```

```
//EXEMPLO 2
try {
    console.log(foo)
    var foo = 2
} catch (e) {
    console.error('A variável `foo` não foi definida.')
}
// Dessa vez a variável foi definida, aí ocorre um hoisting da declaração e seu valor fica 'undefined'


```

### Function Hoisting
Com funções o hoisting acontece de maneira diferente. A definição da função e o seu corpo são elevados (hoisted)

```
function foo(){
    function bar() {
        return 3
    }

    return bar()

    function bar() {
        return 8
    }
}
console.log(foo())

// Dentro do escopo da função foo temos duas definições de função com o nome de
// bar. A primeira já está no topo, mas a segunda vai ser hoisted e sobrescreverá
// a primeira por ser a segunda função com o mesmo nome.
// Com isso o valor retornado será 8

```

## Closure
Closure é uma função interna que tem acesso à variáveis de uma função externa (cadeia de escopo)
O Closure tem três cadeias de escopo: Ele tem acesso ao seu próprio escopo (variáveis definidas entre suas chaves), tem acesso as variáveis e parâmetros da função exterior, e tem acesso as variáveis globais.

Você cria um closure adicionando uma função dentro de outra função

```
//EXEMPLO

function showName (firstName, lastName) {
	var nameIntro = "Your name is ";

	//esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
	function makeFullName () {
		return nameIntro + firstName + " " + lastName;
	}

	return makeFullName ();
}

showName ("Romulo", "Mourão"); //Your name is Romulo Mourão
```

Uma da mais importante e delicada característica dos closures é que a função interior continua tendo acesso as variáveis da função exterior mesmo após ela ter retornado. Isto pode ser um problema em alguns casos e para "consertar" isso podemos utilizar uma IIFE que veremos no final deste artigo.

## Variável Global
Todas as variáveis declaradas que estejam fora de funções, são globais. Estas variáveis são acessíveis em todos escopos. Em outras palavras, se você altera o valor de uma variável Global dentro do escopo de uma função, ela será alterada também fora do escopo desta função.

```
var bar = 0;
function foo(){
    console.log(bar);
    // -> 0
    bar = 1;
}
foo();
console.log(bar);
// -> 1
```
Variáveis globais quase sempre são uma má ideia. Para melhor isso:

```
// Usando de forma certa
(function ($, ko){

        $(document).ready(function(){
            ko.applyBindings({});
        });

    } (jQuery, ko));

    ```
Importamos os modulos para nossa closure. As variáveis jQuery e ko são passadas como parametro da closure anônima. Com isso evitamos conflitos no caso de algumas bibliotecas usarem a mesma variável:

## Variável por parâmetro
O parâmetro passado em uma função se torna uma variável local para o escopo daquela função. É como se fizessemos uma declaração de uma variável dentro do escopo daquela função.

```
function foo(bar) {
    console.log(bar)
}
foo("foobar");
// -> `foobar`

console.log(bar);
// -> bar is not defined
```

O mesmo ocorre para as variáveis globais. Quando estas são passadas como parâmetro é como se fizessemos uma cópia de seu conteúdo para uma variável com o mesmo nome mas de escopo local. Qualquer alteração feita nesta variável permanecerá no escopo local, não sendo replicado para o escopo global.

```
var a = 'foobar';
function foo(bar) {
    bar = 'barfoo'
    console.log(bar)
}
foo(a);
// -> `barfoo`

console.log(a);
// -> `foobar`
```

## Instanciação usando uma IIFE
Uma "Immediately-invoked function expression" (IIFE) é utilizada quando queremos criar um escopo no Javascript para que as variáveis definidas dentro da função não poluam o escopo global.

```
(function () {
  'use strict'

  var sayHi = 'oi'
  console.log(sayHi) // oi
}())

console.log(sayHi) // ReferenceError: sayHi is not defined

```

Outra coisa interessante é que no segundo parênteses, onde invocamos a function, podemos passar qualquer parâmetro como se fosse qualquer outra função.

```
(function (string) {
  console.log(string)
}('oi'))

// Log 'oi'
```

Conclusão: Você deve usar uma IIFE sempre que surgir a necessidade de isolar as variáveis que você precisa do escopo global. Isso é uma boa prática e é uma forma de fazer com que seu código funcione bem independente de onde ele for usado

## Referências
```
http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/
http://tutsmais.com.br/blog/javascript-2/o-que-e-iife-no-javascript/

```

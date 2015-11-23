# Artigo
**autor(a)**: Késsia Castro


Linguagens de programação mais antigas como C/C++/Java e outras, possuem escopo baseado em bloco. JavaScript, por outro lado, é uma linguagem que pode confundir programadores iniciantes por possuir um escopo baseado em função. No qual, variáveis que estão fora de uma função são consideradas variáveis globais e variáveis declaradas dentro de uma função são consideradas locais.

##Exemplo:

```
var numero = 5;
console.log(numero) //=> O resultado será 5.

if (true) {
	var numero = 10;
	console.log(numero); //=> O resultado será 10.
}

console.log(numero); //=> O resultado será 10, pois o valor de "numero" foi alterado dentro do if.
```

Nesse exemplo, tanto a declaração da variável "numero" inicial, quanto a declaração de "numero" dentro do bloco if pertencem ao mesmo escopo, então a variável tem o seu valor alterado de 5 para 10 e, quando chamada na última linha, o valor alterado será mostrado.

## Hoisting

Quando se trata de variáveis, o interpretador de JavaScript pode realizar duas funções que nem todo mundo está ciente: declarar e definir. Quando uma variável é criada o interpretador coloca a declaração de todas as variáveis no TOPO de seus respectivos escopos, como se você tivesse declarado a variável no início mesmo que você a tenha declarado no meio do código. Se nenhum valor foi atribuído, o interpretador a coloca com o estado padrão de "undefined".

```
var a;
console.log(a) //=> Será mostrado "undefinied" no console, pois a variável "a" foi declarada, mas nenhum valor foi atribuído a ela.
var b = 1; //=> Nos bastidores, para o interpretador primeiro "var b;" será declarado como "undefined" e depois "b = 1;" será definido.
console.log(b); //=> Será mostrado o valor 1, atribuído a b.
```

Já uma função, mesmo que uma variável não seja definida no início, o console não mostrará um erro, pois o interpretador considera que aquela variável é undefined por padrão e foi "hoisting", ou seja, declarada no início do escopo.

```
function test(){
	console.log(a);
	var a = 10;
}

test(); //=> O console irá mostrar "undefined", pois o interpretador declarou "a" como "undefined" e o valor 10 só foi atribuído após o console.log. Então no momento que a função test(); é chamada o estado de "a" era "undefined" e não 10.
```
Nesse caso, é como se o interpretador visse o código acima como o seguinte:
```
function test(){
	var a; //=> Devido ao Hoisting, o interpretador coloca a declaração de "a" no início do escopo com o estado "undefined".
	console.log(a);
	var a = 10;
}

test(); //=> O valor mostrado continua sendo "undefined";

```

É aconselhado, como boa prática, que as variáveis sejam declaradas no início do escopo.

```
function test(){
	var a = 10;
	console.log(a);
}

test(); //=> Agora "a" tem o valor 10, pois foi declarada antes do console.log() e seu estado de "undefined" foi alterado para o valor 10 atribuído.

```


## Closure

O escopo baseado em funções permite algumas vantagens ao JavaScript, entre elas o Closure. Considerando que as variáveis declaradas dentro de uma função só são acessadas por essa função, o Closure nada mais é do que a possibilidade de uma função que está dentro de outra acessar as variáveis e métodos da função "mãe" ou externa. Para criar um Closure, você deve criar uma função dentro de outra, como no exemplo abaixo:

```
function dobro(a) {
	return function(b) {
		return a * b;
	}
}

var dob1 = (dobro(2)); //=> O valor 2 é salvo na funcão externa dobro como o valor de "a".
var dob2 = (dobro(5)); //=> O valor 5 é salvo na funcão externa dobro como o valor de "a".

console.log(dob1(2)); //=> O valor 4 é retornado já que a = 2 e b = 2.
console.log(dob2(2)); //=> O valor 10 é retornado já que a = 5 e b = 2.
console.log(dob1(7)); //=> O valor 14 é retornado já que a = 2 e b = 7;
```

No exemplo mostrado acima, a função dobro é armazenada em duas variáveis diferentes, com valores diferentes para o "a". Isso mostra que um Closure continua tendo acesso às variáveis da função "mãe" mesmo depois dela ter sido retornada. Na declaração das variáveis dob1 e dob2, a função é retornada e os valores 2 e 5 são atribuídos ao "a". Dessa forma, quando console.log() chama a função passando o valor, esse é atribuído ao "b", retornando assim o resultado de a * b.

Considerando que JavaScript não é uma linguagem orientada a objetos pura, o uso de Closures é essencial para simular variáveis privadas de um objeto, evitando o acesso ou a modificação desses dados.

## Variável Global

As variáveis globais, em JavaScript, podem ser definidas e usadas por qualquer função. Da mesma forma, variáveis globais podem ser também criadas dentro de uma função e serem usadas em outras. Veja o exemplo a seguir: 

```
var nome = "Fulana"; //=> Variável global declarada

function meuNome() {
	nome = "Cicrana"; //=> A variável nome tem o seu valor alterado dentro da função myName
	return nome;
}

console.log(nome); //=> Retorna "Fulana", pois a função não foi chamada e a variável nome continua com o seu valor inicial
meuNome(); //=> A função myName é chamada e altera o valor da variável nome
console.log(nome); //=> Agora o retorno é "Cicrana", pois o valor da variável foi alterado na função.

```
Quando uma variável global é criada dentro de uma função, seu valor pode ser alterado em outras funções ou usado por estas. Em muitos casos não é aconselhável criar variáveis globais dentro de funções, mas é possível fazê-lo em JavaScript ao definir uma variável sem a palavra chave var. Automaticamente, essa variável torna-se parte do escopo global.

```
function meuNome() {
	nome = "Fulana";
}

console.log(nome); //=> O console gera um erro "ReferenceError: nome is not defined", pois a variável ainda não existe
meuNome(); //=> Ao chamar a função a variável global "nome" é criada 
console.log(nome); //=> É possível chamar a variável fora da função, pois "nome" é uma variável global

```

## Variável por parâmetro


Os parâmetros, ou argumentos de uma função, tem escopo local e são passados por valor. Mesmo que a função altere o valor de um argumento, esse valor só é alterado dentro do escopo da função e não de forma global. Sendo assim, mesmo que uma variável global seja passada como parâmetro, seu valor não será alterado globalmente.


```
var num = 123; //=> Variável global declarada

function teste(a){ //=> A função teste recebe um parâmetro e altera o seu valor para 321
    a = 321;
    console.log(a);
}

teste(num); //=> Ao passar a variável global "num" para a função "teste" ela retorna 321, pois o valor de num é alterado dentro da função.
console.log(num); //=> Retorna 123 mesmo depois de ter chamado a função teste, pois  o valor da variável num não é alterado globalmente, apenas dentro da função.

```

## Instanciação usando uma IIFE

Uma IIFE é uma função anônima que é executada logo após a sua declaração. Por ser anônima e auto-chamada, seu retorno é perdido logo após a sua execução. É possível armazenar a referência para o retorno de uma IIFE em uma variável.

```
var hello = (function () {
     console.log("Hello, World!");
}); //=> O retorno da IIFE é armazenado na variável "hello", dessa forma a função anônima poderá ser chamada apenas quando for conveniente.

hello(); //=> O retorno será "Hello, World!" após a chamada da função pela variável "hello".
```

Assim como as outras funções de JavaScript, a IIFE aceita variáveis como parâmetros e essas variáveis, mesmo que modificadas, não terão o seu valor alterado no escopo global. Dessa forma, o valor inicial de uma variável permanece o mesmo fora da função.

```
var hi = "Hello"

var hello = (function (hi) {
    hi = hi + ", World!";
    console.log(hi);
});

hello(hi);
console.log(hi);
```

# Artigo - Instanciação
**Autor**: Felipe José Lopes Rita - [felipelopesrita](https://github.com/felipelopesrita)
**Data**: 1455810639641

## Hoisting
Hoisting é um comportamento do JavaScript de mover declarações para o topo de um escopo (o escopo global ou da função em que se encontra). Em JavaScript, funções e variáveis são "içadas" ao topo(hoisted), possibilitando sua utilização antes mesmo da declaração.
**Em uma funcão:**
```js
exibeNome('Felipe');
function exibeNome(nome) {
	console.log(nome);
}
//Saída: Felipe
```
## Closure
Closures no javascript são funções que possuem acesso a variáveis existentes em funções externas e consequentemente "pais" delas, isso é, uma função que utiliza de variáveis existentes na função onde a closure é declarada.
Assim, dizemos que a função ``montaNome()``(no exemplo abaixo) é uma closure
```js
function exibeNome( nome, sobrenome ) {

	function montaNome() {
    	return nome+" "+sobrenome;
    }

	console.log( montaNome() );
}
```

## Variável Global
Variáveis globais são variáveis que podem ser acessadas em qualquer trecho da aplicação. Isso é, em javascript, qualquer variável declarada fora de qualquer função, além de objetos como window, são variáveis globais
```js
var x = 10; //Variável global, pode ser acessada em qualquer trecho do código

var y = (function() {
	return x + 5;
}());

function z( num ) {
	return x+num;
}
```

## Variável por parâmetro
Em funções javascript, é possível passar valores para que a função realize operações com estes valores. No entanto, os parâmetros das variáveis apenas copiam os valores informados, o que significa que os parametros da função tem escopo apenas da função em que se encontram. Assim, quando uma variavel global é passada como parâmetro, seu conteudo não é alterado, apenas a variavel do parametro tera seus valores mudados
```js
var x = 10; //variável global
function soma( x ) {
	x += 5;
    return x;
}

console.log(soma(x)); //15
console.log(x); //10
```

## Instanciação usando uma IIFE
Immediately-Invoked Function Expression, IIFE, ou traduzido Expressão de Função Imediatamente Invocada, são funções que são evocadas imediatamente ao serem declaradas.
Há vários locais que podemos usar uma IIFE, porém o mais comum é quando queremos criar um escopo no JavaScript para que as variáveis definidas dentro da função não poluam o escopo global.
Isso é tudo a respeito de evitar ao máximo criar variáveis globais no JavaScript, e criar escopos quando e onde precisarmos.Assim mantemos as variáveis todas dentro do nosso código, evitando que o JavaScript de outras pessoas alterem nossas variáveis.
```js
(function () {
  'use strict';
  var msg = 'oi'
  console.log(msg) // oi
}())
console.log(msg) //undefined
```
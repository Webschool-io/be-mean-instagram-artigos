# Artigo - Instanciação no Javascript
**autor**: Alex Miranda


## Hoisting
Antes de falarmos sobre hoisting é importante lembrar como funciona escopo em JavaScript. Escopo nada mais é do que um contexto criado para os valores e expressões terem sua validade. Em JavaScript o escopo é criado com a declaração de funções. Vamos a um exemplo:

```js
// Escopo global
var num = 0; 

// Escopo criado pela função
function imprimir(){
	var num = 1;
	console.log(num);
}

// Executar a função e o que tem em seu escopo
imprimir(); // 1

// Imprimindo a variável do escopo global
console.log(num); // 0
```
No exemplo acima temos o seguinte: A variável "num" foi declarada com o mesmo nome em 2 lugares diferentes: No escopo global e no escopo criado pela função imprimir. Por elas estarem em escopos diferentes, não tem problema terem o mesmo nome.

Mas **cuidado** ! As variáveis declaras sem a palavra reservada "var" passam a ser parte do escopo global. Olha só:

```js
// Escopo global
var num = 0;

// Escopo criado para a função imprimir
function imprimir(){
	num = 1; 
	console.log(num);
}

// Executar a função e o que tem no escopo
imprimir(); // 1

// Acessando a variável do escopo global
console.log(num); // 1
```

Uma das boas praticas em JavaScript é sempre declarar as variaveis com a palavra reservada 'var' para conter ela 
em seu escopo e manter o escopo global limpo. 

Legal, agora que já relembramos o escopo em Javascript vamos entender o que é o hoisting.

> Hoisting pode ser traduzido como levantar, erguer ou içar.

Esse comportamento na linguagem JavaScript vale para funções e variáveis. Vamos falar primeiro sobre o hoisting de variaveis. Quando declaramos uma variável em JavaScript a mesma é erguida, ou hoisted, para o topo do escopo, no caso de variaveis somente a sua declaração é levada para o topo do escopo mas sua inicialização não. Por exemplo:

```js
function nome(){
	var nome = "Alex";
	console.log("total: " + nome + " " + sobreNome);
	var sobreNome = "Miranda";
}
nome(); // Alex undefined
```
O valor da variável sobreNome é undefined, ou seja, ela esta sendo considerada na função mas o seu valor não. 
E é assim que funciona o hoisting de variável. ;)

No caso de funções o hoisting ocorre de um jeito diferente. Tanto a sua declaração quanto o seu escopo é içado 
para o topo. Olha que interessante:

```js
nome(); // Alex
function nome(){
	var nome = "Alex";
	console.log(nome);
}
```
Sim, a função foi executada antes da sua declaração por conta do hoisting. Porém, aqui vale um lembrete, uma das formas de declaramos funções em JavaScript é armazenando elas em variaveis, nesse caso a regra para hoisting em variaveis entra em cena novamente. Vamos ver o que o nosso exemplo anterior retornaria neste caso:

```js
nome();
var nome = function(){
	var nome = "Alex";
	console.log(nome);
}
```
Declarando a função desta forma o JavaScript retorna um erro dizendo que "nome" não é uma função.

## Closure
A tradução para a Closure em português seria clausura que quer dizer confinamento ou ambiente fechado. Para conseguir esse confinamento basta declarar uma função dentro de outra, a função externa confina a função interna.

O confinamento acontece por conta da regra do JavaScript referente a escopo. Sabemos que o escopo é criado por funções, isso quer dizer que a função externa cria um escopo em que a função interna fica confinada podendo ser executada somente dentro desse escopo. As variáveis e parâmetros da função externa podem ser acessados pela função interna.

Vamos a um exemplo:

```js
// função externa
function lancamentoDeNota(nome, exercicio , nota){
	// função interna
	function fechamento(){
		var mensagem = "Avaliação do exercício : " + exercicio;
		mensagem += "\n Aluno: " + nome;
		mensagem += "\n Nota: " + nota;
		console.log(mensagem)
	}
	// executa a função interna
	fechamento();

} // fecha função externa

lancamentoDeNota("Alex", "Importando collections", "10"); // Avaliação do exercício : Importando collections Aluno: Alex Nota: 10
```
Acabamos de ver um exemplo de closure em JavaScript, mas ainda temos uma diferença muito bacana na linguagem. Em JavaScript é possível escapar a função interna do seu confinamento. Vamos avaliar o código abaixo:

```js
// função externa
function lancamentoDeNota(nome, exercicio , nota){
	// função interna
	function fechamento(){
		var mensagem = "Avaliação do exercicio: " + exercicio;
		mensagem += "\n Aluno: " + nome;
		mensagem += "\n Nota: " + nota;
		console.log(mensagem)
	}
	// escapando a função interna retornando ela de forma literal para função externa. Malandragem é pouco pro JS kkkk
	return fechamento;
}
var primeiroExercicio = lancamentoDeNota("Alex", "Importando collections", "10");
var segundoExercicio = lancamentoDeNota("Alex", "Inserindo Pokemons", "10");

primeiroExercicio(); // Avaliação do exercício : Importando collections Aluno: Alex Nota: 10
segundoExercicio(); // Avaliação do exercício : Importando Pokemons Aluno: Alex Nota: 10
```

O return faz com que a função interna seja retornada de forma literal podendo ser executada fora do confinamento.

## Variável Global
Variáveis globais são todas aquelas definidas fora de alguma função. Isso porque cada função gera seu próprio escopo. A variável global pode ser acessada por qualquer função. Fizemos isso **segundo exemplo**. 

```js
// Escopo global
var num = 0;

// Escopo criado para a função imprimir
function imprimir(){
	num = 1; 
	console.log(num);
}

// Executar a função e o que tem no escopo
imprimir(); // 1

// Acessando a variável do escopo global
console.log(num); // 1
```
Todas as variaveis que **não** forem declaradas com a palavra reservada 'var' serão consideradas parte do escopo 
global.

## Variável por parâmetro
Quando declaramos uma função temos a opção de indicar alguns parâmetros para elas. Tais parâmetros são considerados como variáveis que recebem um valor na hora da execução da função. Esses valores são utilizados dentro da função. Exemplo:

```js
function sub(num1, num2){
 console.log(num1-num2)
}
sub(10, 2) // 8
```

Caso esse parametro seja uma variável global o valor dela não se altera. Por exemplo:

```js
var global = 12;
function sub(global, num2){
 console.log(global-num2);
}
sub(15, 2) // 13
console.log(global); // 12
```

## Instanciação usando uma IIFE
IIFE é a abreviação para Imediately Invoked Function Expression, que pode ser traduzida para Expressão de Função Invocada Imediatamente. Esse tipo de função é executada no mesmo momento que esta sendo interpretada, veja a sintaxe dela:

```js
(function(){
 // corpo da função
}())
```

Os parenteses que envolvem a função fazem dela uma expressão e os parenteses no final da declaração executa a 
função. Esse tipo de função também pode ser armazenada em uma variável. Dessa forma:

```js
var nome = (function(){
 // corpo da função
}())
```

Como toda função, a IIFE também pode receber parâmetros. Mas agora pense o seguinte, se ela é chamada imediatamente em tempo de execução, como podemos passar os parâmetros ? Vamos ver:

```js
var nome = (function(nome){
 // corpo da função
 console.log("Artigo escrito por: " + nome); // Alex
}("Alex"))
```
Bem bacana né ? A IIFE é um pattener em JavaScript que evita poluição no escopo global e com ela também é possivel modularizar o código e deixar tudo mais organizado. 
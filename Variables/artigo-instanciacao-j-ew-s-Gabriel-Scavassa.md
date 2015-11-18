# Artigo
**autor**: Gabriel Alves Scavassa

Explique, com teoria e código, nesse artigo como o JavaScript cria e instancia as variáveis, seguindo os seguintes tópicos.

## Hoisting

Em tradução do inglês para português, *Hoist* significa *içar*. 
Diferente de linguagens como C e C++, em JS o escopo não é definido por blocos, mas por funções.
Ao declarar uma variavel em JS esta será *hosted* para o topo do escopo. 

##### Variaveis
Ao declarar uma variável, após um *console.log()*, por exemplo, sua declaração será *hoisted* para o topo do escopo.
A seguir, o **EX1** declara uma variável *a* atribuindo o valor *0* para ela:
```
	// EX 1
	console.log(a) //=> undefined
	var a = 0
```
Ao executar o código, ele será interpretado da seguinte forma:
```
	// EX 2
	var a
	console.log(a) //=> undefined
	a = 0
```
Como resultado, o console irá registrar *undefined*, isto ocorre por que o *hoist* apenas ocorre para a declaração e não para a atribuição de valor à variável. 

Veja esta situação em JavaScript :
```
	// EX 3
	var a = 1;
	console.log(a);//=> 1
	if(true){
		var a = 3;
		console.log(a);//=> 3
	}
	console.log(a);//=> 3
```
 Ao declararmos uma variável *a* com o valor atribuido *1* e printar, teremos o resultado *1*. Ao entrar no *if*  estamos novamente criando uma variável *a* com o valor *3* e ao printar, temos o valor *3*. Ao sair do *if* printamos novamente a variável *a*, o que teremos o valor *3*.  Em JavaScript o escopo é a propria função, e a variavel *a* foi *hoisted* para o topo do escopo. Veja:
```
	// EX 4
	var a = 1;
	a = 1;
	console.log(a); // => 1
	if(true){
		a = 3;
		console.log(a); //=> 3
	}
	console.log(a); //=> 3
```
Se codificar o mesmo em C++ teriamos o primeiro print como *1*, ao entrar no *if* teriamos o print *3* e ao sair do *if* teriamos o print *1* novamente, isto por que o escopo do C++ em outras linguagens, se da ao bloco de código(if, while, do, for...).

Para que nós conseguissemos reproduzir a mesma situação que ocorre em C++ e tantas outras, nós usamos o *let*.
Ao declarar uma variável com *let* esta variável fica restrita em um novo bloco (criado dentro de *if*, *for*..), ou seja, ao usar o *let* não ocorre o *hoisting*. Esta nova forma de declaração de variável veio no ECMAScript na versão 6.

```
	// EX 5
	var a = 1;
	console.log(a); //=> 1
	if(true){
		let a = 3;
		console.log(a); //=> 3
	}
	console.log(a); //=> 1
```

Reparem que o valor printado após o *if* permanece igual ao declarado antes do *if* e dentro do *if*, usando o *let* seu valor é printado de acordo com o valor atribuido a variável *a*.

O mesmo ocorre para a declaração de expressão de função:
```
   // EX 6
	var func = function(){
		console.log('Entrou na func!');
	}
```

##### Funções

Em função, ocorre algo parecido. quando declaramos uma função, não só sua declaração é *hoisted* como também todo seu escopo. 
Por exemplo:
```
	// EX 7
	func();

	function func(){
		console.log('Func executada'); //=> Func executada
	}
```
Como visto, chamar a função *func()* antes de sua declaração, faz com que ela seja executada normalmente sem erros. Porém, como observado no EX 7 a cima, uma situação parecida geraria erro, veja:
```
	// EX 8
	func();
	var func = function(){
		console.log('Entrou na func!'); // => Type error func is not a function
	}
```
Outra situação que ocorre com o *hoist* de funções é a sobriscrição. 
Veja o exemplo:
```
	// EX 9
	function func(){

		function func1(){
			console.log('1');
		}

		return func1();

		function func1(){
			console.log('2');
		}

	}

	console.log(func()); // => 2
```
O resultado será *2* já que quando chamamos o a função *func()*  temos a declaração de *func1()* duas vezes. Devido ao *Hoisting*, será içado para o topo do escopo, as declarações de *func1()* onde retornam *console.log('1')* e *console.log('2')* para então, realizar o *return*. Como há duas funções com o mesmo nome, e ambas são *hosted* a função *func1()* que retorna *console.log('2')* sobrescreve a que retorna *console.log('1')*. Por isto o return printado é *2*. Veja como fica na execução:
```
	// EX 10
	function func(){

		function func1(){
			console.log('1');
		}

		function func1(){
			console.log('2');
		}

		return func1();

	}

	console.log(func()); // => 2
```

Concluindo, *Hoist* em JS é o içamento de váriaveis e funções para o topo do contexto atual, onde para as váriaveis inicializadas,o valor atribuido na inicialização não é içado. Já com funções, tanto seu nome quanto seu contexto são içados para o topo do contexto.

## Closure

*Closure* é um termo para definir a disponibilidade de variáveis de uma função exterior dentro de uma fuñção interior, ou seja, uma cadeia de escopos.

Por exemplo:
```
	//EX 11
	function(){  // 1° escopo 
		var a = 3;
		function(){ // 2º escopo
			var b = 5;
			function(){  // 3° escopo
				var c = 7;
			}
		}
	}
```
No EX11, existem 3 funções e consequentemente 3 escopos. A 1ª função tem uma váriável disponível *a* a função seguinte tem disponível as váriaveis da função "pai"( ou seja 1º escopo) *a* e mais a variável declarada dentro de seu contexto *b* e a próxima função tem disponível as variáveis *a* e *b*, que são dos escopos anteriores e *c* que foi declarada dentro do própio escopo.


Desta maneira podemos ter:
```
	// EX 12
	function foo(){
		
		var a = 28;

		soma();

		function soma(){
			var b = 2;
			console.log(a + b);
		}

	}

	foo(); //=> 30
```

No exemplo a cima podemos ver que é possivel utilizar a variável *a* dentro da *soma()*.

A função interna pode também ter acesso aos parametros passado para a função externa e às variáveis globais. A função interna pode ter acesso às variáveis da função externa mesmo após ao final de sua execução, o que pode deixar algumas informações expostas.

Por exempo: 

```
	// EX 13
	
	function foo(a){ // FUNÇÃO EXTERIOR 1º CONTEXTO
		var b = 28;
		function soma(c){ // FUNÇÃO INTERIOR 2º CONTEXTO
		    return a + b + c;
		}
		return soma;
	}
	
	//RES RECEBE A FUNÇÃO SOMA()
	var res = foo(10);

	//CHAMA RES PASSANDO UM PARAMETRO , ESTA USARÁ O PARAMETRO PASSADA NA FUNÇÃO EXTERIOR ALÉM DA VARIÁVEL DECLARADA NELA 
	res(20); // => 58
```

O *Closure* pode ser utilizado para declaração de variáveis globais, também é utilizado no Node.js e Jquery. 

## Variável Global

Podemos declarar variaveis que não estão dentro de funções e que podem ser explicitas ou implicitas. Estas variaveis podem ser acessadas e alteradas por quais quer função.

A criação de variáveis no JS pode ser feita sendo explicita ou implicita. Para cria-la de maneira explicita nós utilizamos o *var* ou implicita, sem utilizar o *var*. 

Criar váriaveis globais sem utilizar o *var* não é recomendado, pois torna a manutenção mais difícil, deixando o código menos acoplado.

Veja a seguinte situação:

```
	// EX 14
	var a = 1;

	function PrintaValor(){
		console.log(a);
	}

	function MudaValor(){
		a  =  2
		console.log(a); 
	}

	PrintValor(); //=> 1
	MudaValor(); //=> 2
	PrintValor(); //=> 2

```

Uma solução para este caso é declarar localmente a variável *a* dentro da função *MudaValor()* utilizando o *var*.

## Variável por parâmetro

São variáveis que podem ser utilizadas dentro do contexto de uma função. Estas podem ser uma *var* ou *function*.
Veja a seguir com uma *var*:

```
   // EX 15

   function Soma(a){
   		console.log(a + 1); //=> 11
   }

   Soma(10);

```
Veja a seguir com uma *function*

```
	//EX 16 

	function Soma(a){
		console.log(a + 1);
	}

	function RealizaOperacao(funcao, variavel){
		funcao(variavel);
	}

	RealizaOperacao(Soma, 2); // => 3
```

Ao receber uma variavel global em uma função como parametro, o valor que estava atribuido à variável global passa a ser atribuido à variável local e toda alteração feita nela, não afetará a global.

```
	// EX => 17
 	var a  = 1;

 	function Soma(variavel){
 		console.log('Recebido',variavel); // => 1
 		console.log( 'Alterado' variavel += a); // => 2
 		console.log( 'Global', a); // => 1
 	}

 	Soma(a);
```

## Instanciação usando uma IIFE

o IIFE é uma função que é executada assim que criada. Desta maneira, não há como chama-las em outro tempo dentro da execução.

Por exemplo:

```
	// => 18

	(function(){
		var a = 1;
		console.log(a); // => 1
	})();
```
Passar um parametro para uma IIFE é simples, basta passar o parametro dentro do parenteses que recebem a função.
```
	// => 19

	(function( a){
		var b = 1;
		console.log(a + b); // => 11
	})( 10);
```
O parâmetro passado para uma IIFE é tratado como um parâmetro em uma função.


## Referências

https://developer.mozilla.org/en-US/docs/Glossary/Hoisting
http://stackoverflow.com/questions/15395347/does-a-browser-truly-read-javascript-line-by-line-or-does-it-make-multiple-passe
http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures
http://nodebr.com/passando-funcao-como-parametro-no-javascript-com-node-js/



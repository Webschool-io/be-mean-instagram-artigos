# Artigo - Instaciação
autor: Anderson da Silva Souza 

## Hoisting

É uma declaração de uma variavél ou função que esteja sendo executada no inicio do escopo.

# Exemplo
 
 a(); //3
function a(){
	
	console.log(b);
}
var b = 3;

A função declara no inicio do escopo imprimi o seu resultado sem nenhum erro mesmo sendo executa antes da sua função pois ela obdece a regra do hoisting.


## Closure
Quando uma função passa a executar acesso a variáveis interna e externas, pode - se considerar como funcão clousure. 
O closure tem três cadeias de escopo: ele tem acesso ao seu próprio escopo (variáveis definidas entre suas chaves), ele tem acesso as variáveis da função exterior e tem acesso as variáveis globais.

# Exemplo

function estado(estado){
	
	var cid = "salvador";
		function cidade(){

			console.log("O meu estado é a "+estado+" e resido na cidade de "+cid+".")
		}

		cidade()
}

estado('bahia');

## Variável Global

Uma variável que estaja acessivel a todos os escopos de uma aplicação é considerada como variável global.

# exemplo

		var a =1;
		b();

		function b(){
			
			var c =1;

			console.log(c+a)
		} 

## Variável por parâmetro

No javascript, as funções são atribuidas a uma variável, passada como parâmetro que pode ser atribuida a qualquer tipo de dados por exemplo: int,string,boolen,objeto e funções.

#Exemplo

		var a = 1;
		var b = 2;


		function soma(a,b){

		console.log(a+b);
			
		}

		soma(a,b);//3

## Instanciação usando uma IIFE

	O IIFE significa “Immediately-invoked function expression”, mas podemos chamá-lo de função imediata. Ou seja IIFE é considerado tudo que esteja dentro de parênteses , o que irá retornar qualquer coisa que estiver dentro dele.

	# Exemplo 1

		(1 + 2) // retorna 3
	# Exemplo 2
	(function (){
			
			console.log('Execução da função!') //Execução da função!
		}())
		//Invocação da função anônima


## Fontes

http://tutsmais.com.br/blog/javascript-2/o-que-e-iife-no-javascript/
http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/
http://www.w3schools.com/js/js_hoisting.asp
http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/









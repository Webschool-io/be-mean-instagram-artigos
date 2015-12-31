#Instanciação de Variáveis no Javascript
 > **Autor:** Vagner Pereira Silva
 > **Data:** 31/12/2015
 
## Resumo

A seguir veremos um pouco de alguns conceitos bastante importante no universo JavaScript. Veremos a importância de se entender melhor o conceito de escopo e como podemos manipulá-lo a nosso favor , evitando assim dores de cabeça, pelo simples fato de desconhecer básico desses conceitos.

 **Palavras-Chave: Hoisting, escopo, variável global, variável local, função, IIFE, Invocação imediata, parâmetro**

## Abstract

The following are some very important concepts in JavaScript universe. We will see the importance of a better understanding of the concept of scope and how we can manipulate it to our benefit, avoiding headaches, simply for lack of knowledge of these concepts


**Keywords: Hoisting, scope , global variable , local variable , Function , IIEF , invocation Immediate , parameter**


## 1. Hoisting

   Antes de entendermos o conceito de **hoisting**, em JavaScript, iremos primeiramente aborda o conceito de escopo.
>“... O escopo refere-se ao local onde os objetos, as variáveis e as funções são **criadas e acessíveis** e em que **contexto** estão sendo chamadas. Basicamente, os objetos, as variáveis e as funções pode ser definida em um espoco **local** ou **global**.” (BENEDETTI e CRANLEY, 2013)


Escopo local abrange apenas um trecho do código, um bloco de execução especifico, por assim dizer. De forma mais sucinta, são delimitadas dentro de funções, por exemplo;
```js
var n1 = 5;
function calc(){
var n1 = 10;
return n1 + n1;
};
console.log(calc());
```
Com o código acima, temos como resultado o valor 20, pois a declaração da variável n1 **dentro do espoco calc** (função), restringi o seu uso apenas dentro do seu bloco de execução, iniciado e terminado por chaves {}.
Já o escopo global e acessível em qualquer lugar no código.

```js
var n1 = 5;
function calc(){
return n1 + n1;
};
console.log(calc());
```

Dado a explicação referente a escopo. Vejamos o código a seguir:

```js
	add(1,2);
		function add(x, y){
			return x + y;
};
```

Segundo (ZAKAS, 2014),  só e possível compilarmos o código acima devido a engine  do JavaScript efetuar o **hoisting**, ou seja, a função add que está sendo chamada e carregada no topo e em seguida executa o código, como se o mesmo tivesse sido escrito da seguinte forma.
```js
	
	function add(x, y){
		return x + y;
};
              add(1,2);
```

Ainda, segundo (ZAKAS, 2014)  o hoisting de funções ocorre somente em declarações de funções porque o nome da função é previamente conhecido. 
Já expressões de função, não podem sofre hoisting devido as funções serem referenciadas somente por meio de uma variável.
O termo hoisting, dá-nos a possibilidade de declararmos uma variável sem necessidade de inicializarmos de forma imediata.
Mesmo sendo possível a declaração de variáveis em diversões "ordens", por padrão as variáveis devem ser declaradas no início do seu escopo

```js
	//Padrão

             var  x,y,z;
              
              x = 0;
              y = 1;
              z = 2;
              
              function calc(x,y,z){
              		var a,b;
              		a = 10;
              		b = 15
              		
              		return a + b + x + y + z;
              }
              
```


## 2. Closure

Antes de falarmos de closure, iremos consolida o conceito de **escopo estático**.

##### 2.1 Escopo Estático

“... O escopo estático e chamado assim porque o escopo de uma variável pode ser determinado estaticamente – ou seja, antes da execução. Isso permite a um leitor de programas humano (e um compilador) determinar o tipo de cada variável. “ (SEBESTA, 2011)

“... Devido ao escopo estático ser baseado na estrutura gramatical de um programa, é às vezes chamado **de escopo léxico** ...” (TUCKER e NOONAN, 2009)

Segundo (LEMAY e CADENHEAD, 2005)  se declaramos uma variável em um bloco, ela só será visível e utilizável apenas por esse bloco, ou seja, quando o bloco acaba de ser executado todas as variáveis declaradas nele desaparecem. 
Com isso no escopo estático (ou léxico) podemos dizer que em uma estrutura aninhada os blocos internos, tem acesso as variáveis do(s) bloco(s) externo(s). 
Exemplo:

```js
	var x = 10; // variavel global.

		soma = function(){ // bloco externo.

			var y = x;

			 return function (){ // bloco interno.

				var z = y; // variavel local sendo inicializada com a variavel do bloco externo.

				return x + y + z;

			};

	}
```
##### 2.2 Definindo Closure

(BENEDETTI e CRANLEY, 2013) definem closure do seguinte modo:

* Um closure é a variável local para uma função, mantida viva depois que a função foi retornada.
* Sempre que vir a palavra-chave function dentro de outra função, a função interna possui acesso às variáveis externas.

Exemplo:
```js
var x  = 10;
 	function calc(){  
 		
 		var y = x;

 		return function (){ // função anônima

 			return x + y; // variáveis declarada fora da função

 		} ();

 }
calc();

```

Com o exemplo acima podemos dizer que, todas as vezes que uma função for declarada no bloco ({}) de outra função e utilizarmos as variáveis ou objetos dessa função externa, a essas funções damos o nome de closure.
Um bom exemplo da utilização de closure e na utilização de callback.
```js
var informacao; 

 function log(msg) {

    var info = function () {
        alert(msg);

    };
    
    return info;
};

informacao = log("Estou utilizando Closure");

setTimeout(error, 3000);  // 3 segundos
```

## 3. Variável Global

Como citado anteriormente, no capitulo 1 (Hoisting) , variável global, como o próprio texto sugeri, e a variável na qual podemos obter acesso em qualquer parte do código. Normalmente e por padrão, declaradas no início do documento.

```js
	var a,b,c,d; // variáveis globais
	
	a = 1;
	b = a;

	function e(x){
	
		c = x;

		return b + c;
	}

	d = e(b);
```

Acima vemos, claramente, o comportamento de uma variável global, onde o valor da variável **b** e utilizada dentro da função **e**.

Porém, e importante temos em mente, que, ao utilizarmos variáveis com o a mesma nomenclatura de uma variável global no corpo de uma função, faz com que a variável utilizada localmente “sobrescreva” o conteúdo da variável global, apenas dentro de sua estrutura e  mantendo a variavel global com seu valor de inicialização, por assim dizer.

```js
var a, b;
	
	a = 1;

	function exemplo(){

		var a  = 0;

		return a;

	}

	b = exemplo (); // retorna a variável a dentro da função e.
	alert("a será igual a " + b)  // a será igual a 0.
	
	b = a; // varialve a global.
	alert("a será igual a " + b)  // a será igual a 1
	
``` 

## 4. Variável por parâmetro

Algumas vezes há a necessidade de passar argumentos para uma função, essa atribuição de dados na função e efetuada através de parâmetros, onde podemos ter n parâmetros atribuídos e para cada atribuição devemos efetuar a separação por virgula (,).

Exemplo de uma função com parâmetro:
```js
		function(parametro1, parametro2){}
```
No javascript a atribuição de um argumento a uma função com parâmetro e opcional, ou seja, não retorna erro ao invocarmos tal função sem passarmos argumentos a ela, mas e importante ressaltar, que, a não atribuição de valor a um parâmetro e tratado pelo javascript como **undefined** e com isso, de acordo com a necessidade, somos obrigados a efetuar a tratativa desse parâmetro.
 ```js
// considerando que o parâmetro y e opcional.
	var soma  = function(x, y){


		//  === corresponde a exatamente igual ao valor e tipo.
		if(y === undefined) {  //se y for exatametente igual a undefined atribuir 0.

			y  =  0;

		};
			return x + y; // Caso o parâmetro  não fosse tratado teriamos 
				      // como retorno  NaN = Not a Numbuer (não é um número))
			              // indicando que os tipos não são iguais.
	}

	alert(soma(10));
```

Segundo (LEITE, 2006) quando atribuímos valor a um parâmetro na verdade o que estamos passando e a “cópia” do valor dessa variável e a ligação ente as duas pode ser considerada fraca. Com isso, a função que está sendo chamada pode altera o valor do parâmetro dentro do seu corpo, mas a alteração efetuada nesse valor não altera o valor da rotina chamadora. 
Dessa forma podemos dizer que ao passarmos uma variável global como argumento de uma função com parâmetro, não e alterado seu valor.

```js
		var  x,soma // variavies global
		x = 10;

		soma = function(x,y){

			x = x + y;

			return x; // x  =  20
		}

		alert(soma(x,10));

		alert(x) //  retorno da varial global = 10
```

## 5. Instanciando Usando Uma IIFE (Immediately Invoked Function Expression)

“Uma IIFE é uma expressão de função definida e chamada imediatamente para gera um resultado. Essa expressão de função pode conter qualquer número de variáveis locais que não sejam acessíveis de fora dessa função” (ZAKAS, 2014)
Ou seja, com isso podemos evitar conflitos com variáveis e função do espoco global, ou, até mesmo, com conteúdo de terceiros. A grosso modo IIFE encapsula, tornando privado o conteúdo do seu escopo.

Estrutura IIFE:
```js
;(  // () faz com que a função se torne um expressão
	// para que assim possa ser invocada.
	function(){}

	() // invoca a função

	);

```
Variações que podem ser utilizadas

```js

!function(){}();
+function(){}();
~function(){}();
-function(){}();

```

O IIFE facilita o trabalho e é amplamente utilizado, pois podemos definir melhor com quais objetos do escopo global iremos interagir, evitando muita dor de cabeça (conflitos). Para isso, passamos as variáveis globais atravéz de parâmetro.

```js
(
function($){  /* código com acesso a variável global JQuery através do parâmetro $ */}
(JQuery)
);
```

Também, há possibilidade de exportarmos objetos, para isso, retornamos funções

```js
var Pessoa = (function () {
  var nome;
  var setNome = function(Pnome) {
    nome = Pnome;
  };
  var getNome = function() {
    return nome;
  };

  return {

    setNome: setNome,
    getNome: getNome
  };
}());

Pessoa.setNome("Vagner"); 
Pessoa.getNome();

```
Ao executar o código acima, será adiciona dos novos métodos públicos ( Pessoa.setNome e Pessoa.getNome) , porém todo conteúdo desses dois novos métodos serão mantido privados. Com isso não teremos problemas se terceiros utilizarem por exemplo a variável nome em seu escopo, pois qualquer alteração feita por conteúdo de terceiros ou nossa no escopo global não terá nem um efeito no conteúdo interno da expressão IIFE.

## Conclusão
Todos os conceitos vistos neste artigo, estão ligados entre si e claramente vemos isso. Mas do que isso, são conceitos bastantes básicos, mas de suma importância no desenvolvimento.
Tê-los em mente, e bem definidos seus conceitos, contribui muito em soluções de problemas, assim como, em evita-los. Saber o que é e de que forma podemos manipular um escopo no javascript e fundamental. Ter claro a ideia de escopo global e local faz muita diferença na implementação de códigos coesos, assim, como a manipulação de variáveis, objetos e funções. 
Saber que através do conceito de closure podemos isolar um trecho do nosso código, não permitindo que variáveis possam ser alteradas externamente, tal como, a utilização da inovação de uma função de forma imediata através da expressão IIFE, consolida ainda mais o conceito de privacidade do conteúdo em relação a influência externa, ou seja, não permitindo alterações fora do escopo local. Faz com que possamos concluir que tais conceitos devem ser disseminados desde do contato iniciar com a linguagem, pois é peça fundamental para construção de códigos em javascript. 

#Instanciação de Variáveis no Javascript
 > **Autor:** Vagner Pereira Silva
 
 > **Data:** 24/12/2016
 
## Resumo

## 1. Hoisting

Antes de denominarmos o conceito de **hoisting**, em JavaScript, iremos primeiramente aborda o conceito de escopo.

“... O escopo refere-se ao local onde os objetos, as variáveis e as funções são **criadas e acessíveis** e em que **contexto** estão sendo chamadas. Basicamente, os objetos, as variáveis e as funções pode ser definida em um espoco **local** ou **global**.” (BENEDETTI e CRANLEY, 2013)

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
Já o escopo global e acessível em qualquer lugar no código, sendo declarado no corpo do documento.

```js
	var n1 = 5;
		function calc(){
			return n1 + n1;
		};
	console.log(calc());

```

Dado a explicação referente a escopo. Vejamos o código a seguir:

```js
	var  soma = add(1,2);
		function add(x, y){
			return x + y;
		};
```
Segundo (ZAKAS, 2014),  só e possível compilarmos o código acima devido a engine do JavaScript efetuar o **hoisting**, ou seja, a função add que está sendo chamada pela variável soma e carregada no topo e em seguida executa o código, como se o mesmo tivesse sido escrito da seguinte forma.

```js
	
	function add(x, y){
		return x + y;
	};
        var  soma = add(1,2);
```

Ainda, segundo (ZAKAS, 2014)  o hoisting de funções ocorre somente em declarações de funções porque o nome da função é previamente conhecido. 
Já expressões de função, não podem sofre hoisting devido as funções serem referenciadas somente por meio de uma variável.
Logo o seguindo trecho de código nos retornará erro.

```js
	//Erro - Uncaught TypeError: add is not a function(…)

             var  soma = add(1,2);
              
             var add  = function(x, y){
				return x + y;
			};
```

O termo hoisting, dá-nos a possibilidade de declararmos uma variável sem necessidade de inicializarmos de forma imediata.
Mesmo sendo possivel a declaração de variavies em diversões "ordens", por padrão as variaveis devem ser declaradas no ínicio do seu escopo

```js
	//Pradrão

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

(BENEDETTI e CRANLEY, 2013) definem closure dos seguintes modos:

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

Variável Global

Como citado anteriormente, no capitulo 1, variável global, como o próprio texto sugeri, e a variável na qual podemos obter acesso em qualquer parte do código. Normalmente e por padrão, declaradas no início do documento.

```js
	var a,b,c,d; // variáveis globais
	
	a = 1;
	b = a;

	function e(x){
		var b = 0;
		c = 2;

		return b + c;
	}

	d = e(b);
```

Acima vemos, claramente, o comportamento de uma variável global. Porém, e importante temos em mente, que, ao utilizarmos variáveis com o a mesma nomenclatura de uma variável global no corpo de uma função, faz com que a variável utilizada localmente “sobrescreva” o conteúdo da variável global, dentro de sua estrutura.

```js
var a, b;
	
	a = 1;

	function exemplo(){

		var a  = 0;

		return a;

	}

	b = exemplo ();
	alert(“a =” + b)  // a será igual a 0
``` 


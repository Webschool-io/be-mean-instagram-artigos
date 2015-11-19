# Artigo

**Autor**: Angelo Rogério Rubin

## Tópicos do Artigo
* Hoisting
* Closure 
* Variáveis Locais e Globais
* IIFE (Immediately Invoked Function Expression)

## HOISTING

### O que é Hoisting

Hoisting é uma ação do interpretador do javascript com o objetivo de mover as declarações feitas (tanto de variáveis como de funções) para o topo do seu escopo, quer ele seja global ou local.

### Hoisting de variáveis

Quando ocorre o hoisting da variável o comportamento é o seguinte, apenas a declaração da variável é movida até o topo, ela não é inicializada e nada do que é atribuído a ela é movido, somente a sua declaração.

Exemplo - Hoisting de Variável

    console.log(myVar); // undefined

    // Variável declarada no escopo global
	var myVar = 'I am a invisible assign, i hate you hoisting !!!';

Como podemos observar no código acima, o resultado foi undefined, isso ocorreu porque quando a variável sofreu o hoisting, ela não foi inicializada e nem suas atribuições foram movidas, portanto obtivemos o comportamento esperado através da ação do hoisting.

### Hoisting de funções

No caso das funções, tanto a declaração da função como tudo que esta em seu interior será movido para o topo do seu escopo na execução do hoisting.

Exemplo - Hoisting de Função

    console.log(myFunc()); // Hi

	// Função declarada no escopo global
	function myFunc() 
	{ 
	 	return 'Hi'; 
	}

Portanto notamos que ao ser executada a função myFunc() que foi declarada posteriormente a sua chamada, (através do hoisting) é elevada ao topo do contexto global onde esta inserida e funciona como esperado.

## CLOSURES

Um closure é uma função interna que tem acesso a variáveis de uma função externa, conhecido como cadeia de escopo. Um closure possui três cadeias de escopo: 

* Ele tem acesso ao seu próprio escopo (variáveis definidas entre suas chaves);
* Ele tem acesso as variáveis da função exterior;
* Ele tem acesso as variáveis globais.

Como no javascript não existe uma forma de se declarar métodos e propriedades privadas ( como no PHP, por exemplo - public/private/protected ), podemos concluir que o intuíto maior das closures é o isolamento de escopo entre o que é local e o que é global.

Exemplo - Closure

	var add = (function () {
    	var counter = 0;
    	return function () {return counter += 1;}
	})();

	console.log(add()); // 1
	console.log(add()); // 2
	console.log(add()); // 3

## VARIÁVEIS GLOBAIS

Em um documento web, as variáveis globais pertencem ao objeto da janela o (window object).

As variáveis globais podem ser utilizadas e alteradas por todos os scripts no documento e na janela.

Exemplo - Variável Global

	// Variáveis declaradas no escopo global
	var a, b, c;

	function x() {
		// Variáveis declaradas no escopo local da função
		var a, b, c;
	}

## VARIÁVEIS POR PARÂMETRO

Ao passar uma variável do tipo primitivo como uma string ou um número, o argumento é passado por valor. Isto significa que quaisquer alterações a essa variável dentro da função é completamente distinta de qualquer coisa que acontece fora da função. Vejamos um exemplo:

	function myfunction(x)
	{
      // x é igual a 4
      x = 5;
      // x agora é igual a 5
	}

	var x = 4;
	alert(x); // x é igual a 4
	myfunction(x); 
	alert(x); // x ainda é igual a 4


Porém se passarmos um objeto como argumento a função, este será passado por referência. Neste caso, qualquer propriedade do objeto é acessível dentro da função. Vejamos outro exemplo:

	function myobject()
	{
		this.value = 5;
	}

	var o = new myobject();
	alert(o.value); // o.value = 5
	
	function objectchanger(fnc)
	{
		fnc.value = 6;
	}

	objectchanger(o);
	alert(o.value); // o.value agora é igual a 6

## IIFE (Immediately Invoked Function Expression - Função Anônima Auto Executável)

As IIFE's são funções anônimas, ou seja, sem assinatura (nome), que são executadas no mesmo momento de sua instanciação.
Abaixo podemos ver a estrutura de uma IIFE:

	(function() {
  		// o código aqui é executado uma vez em seu próprio escopo
	})();


# Referências

http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade
https://en.wikipedia.org/wiki/Closure_(computer_programming)
https://www.hugobessa.com.br/posts/entendendo-escopo-e-hoisting-no-javascript
http://benalman.com/news/2010/11/immediately-invoked-function-expression
http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/
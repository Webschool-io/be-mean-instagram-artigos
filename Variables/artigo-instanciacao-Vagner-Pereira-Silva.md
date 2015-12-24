#Instanciação de Variáveis no Javascript
 > **Autor:** Vagner Pereira Silva
 
 > **Data:** 24/12/2016
## Resumo

## 1. Hoisting

Antes de denominarmos o conceito de **hoisting**, em JavaScript, iremos primeiramente aborda o conceito de escopo.

“... O escopo refere-se ao local onde os objetos, as variáveis e as funções são **criadas e acessíveis** e em que **contexto** estão sendo chamadas. Basicamente, os objetos, as variáveis e as funções pode ser definida em um espoco **local** ou **global**.” (BENEDETTI e CRANLEY, 2013)

Escopo local abrange apenas um trecho do código, um bloco de execução especifico, por assim dizer. De forma mais sucinta, são delimitadas dentro de funções, por exemplo;

````js
	var n1 = 5;
		function calc(){
			var n1 = 10;
			return n1 + n1;
		};
	console.log(calc());
````
	
Com o código acima, temos como resultado o valor 20, pois a declaração da variável n1 **dentro do espoco calc** (função), restringi o seu uso apenas dentro do seu bloco de execução, iniciado e terminado por chaves {}.
Já o escopo global e acessível em qualquer lugar no código, sendo declarado no corpo do documento.

```js
	var n1 = 5;
		function calc(){
			return n1 + n1;
		};
	console.log(calc());

```
resultado =  5
```
	
```


	

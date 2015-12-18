# Variáveis JS - Instânciação

###### `Author: Luan Rafael - 22-11-2015`

### Resumo

Este Artigo apresentará rápidamente, alguns conceitos e funcionalidades utilizadas no JavaScript. Boa Leitura!

### Hoisting

Em JavaScript uma variável pode ser usada antes mesmo de ser declarada, isto é possível pois as declarações de variáveis são executadas antes de qualquer código ser executado, logo os código 1 e 2 abaixo são equivalentes.

```
    foo = 'bar';
	var foo;
```
	
```
    var foo;
	foo = 'bar';
```

Uma variável que é atribuida antes de ser declarada será considerada uma variável global, por mais interessante que seja este recurso do JS recomenda-se sempre que as declarações estejam presentes na parte superior do seu código.

O estado de hoisted também acontece com funções, porém de uma forma um pouco diferente, o JavaScript emplilha as funções de acordo com seus escopos e as reescreve caso aja uma nova declaração no mesmo escopo, com isto o código a saída do código abaixo é "foobar"


```
	function foo(){

		function bar(){
			return 'bar';
		}

		return bar();

		function(){
			return 'foobar';
		}

		}
```


### Closure

Closure é uma função interior que tem acesso a variáveis de uma função exterior. Clouseres trabalham em uma cadeia de escopos, sendo elas: Local, Exterior e Global.

 ```
 	//Escopo Global
 	function teste(){
 		
 		//Escopo Exterior
 		
 		function teste2(){
 			//Escopo Local

 		}
 	}
 ```

Uma closure é criada quando criamos uma função dentro de outra função, como os exemplos abaixo.

```
	$(function(){
		var contador = 0;
		$('.btn').click(function(){
			contador += 1;
		});
	});

```

### Variável Global

O uso de variáveis globais em JavaScript é bem simples, para isto basta declará-la no escopo globar da aplicação, ou seja, ela não deve estar dentro de nenhum escopo, como o exemplo abaixo.

```
 	var count = 0;
 	$(document).ready(function(){
 		$('.add').click(function(){
 			count+=1;
 		});
 	});
```
 Lembrando que quando a variável não é declarada (inicialização sem o `var`) entende-se que ela pertence ao contexto global, então logo o código abaixo é equivalente ao código anterior.

 ```

 	$(document).ready(function(){
 		count = 0;

 		$('.add').click(function(){
 			count+=1;
 		});

 	});

 ```
Porém é altamente recomendado que não se use variáveis globais, por diversos motivos, um deles é o fato de uma variável global dificultar o entendimento do código, pois fica difícil definir sua real função.

### Variável por parâmetro

Toda vez que uma variável é passada como argumento em uma função ela tem seu acesso restrito ao escopo local, mesmo que esta variável seja global, vejamos um exemplo:

```
	function soma(a,b){
		b + 1;
		return a + b;
	}

	console.log(soma(1,1)); // 3

	var b = 2;

	console.log(soma(1, b)); // 4
	console.log(b); // 2

 ```

 Reparem que mesmo atualizando o valor da variável `b` dentro da função `soma` seu valor não foi alterado no escopo global


 ### Instanciação usando uma IIFE

 O IIFE (Immediately-Invoked Function Expression) é uma função que se autoexecuta, pode ser chamada de função imediata ou anônima também. A usamos para criar escopos e encapsularmos nosso código.

 ```
 	(function(){
 		console.log('foobar');
 	}());
 ```

Criar uma IIFE é o mesmo que atribuir uma função a uma variável, logo o exeplo a seguir é equivalente ao anterior.


 ```
 	var foobar = function(){
 		console.log('foobar');
 	};
 	foobar();
 ```

A passagem de parâmetros acontece da mesma maneira que uma função normal, ou seja, se minha IIFE espera um parâmetro basta eu passar o parâmetro na sua chamada, vajamos o exemplo;
```
 	(function(foobar){ // declaração da IIFE
 		console.log(foobar);
 	}('foobar')); // Execução
```

O código acima mostra que para passarmos um parâmetro para uma IIFE basta adicionarmos ele na chamada da função;

### Referências

http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/
http://stackoverflow.com/questions/8228281/what-is-the-function-construct-in-javascript
https://nandovieira.com.br/design-patterns-no-javascript-singleton
https://brunosouza.org/functions-no-javascript.html
http://nodebr.com/passando-funcao-como-parametro-no-javascript-com-node-js/
http://pt.stackoverflow.com/questions/3924/como-passar-par%C3%A2metros-em-chamadas-de-fun%C3%A7%C3%B5es-por-refer%C3%AAncia-em-javascript
http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/
https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/var
http://www.w3schools.com/js/js_hoisting.asp
http://code.tutsplus.com/tutorials/javascript-hoisting-explained--net-15092
# Artigo
**Autor**: Helton Barbosa

Explique, com teoria e código, nesse artigo como o JavaScript cria e instancia as variáveis, seguindo os seguintes tópicos.

## Hoisting
### Explique o que é, o porquê acontece e como acontece com variável e função.
É a elevação ao topo do escopo as declarações das variáveis e as funções declaradas dentro do escopo.
	
**Exemplo 01 -  Hoisting Variável**
```javascript
	// Irá imprimir o erro dentro do `catch`, já que não houve declaração da variável `a` dentro do escopo.
	try {
		console.log(a);
	} catch (e) {
		console.error('A variável `a` não foi definida.');
	}
```
[Clique aqui e execulte exemplo acima no JsBin.com](https://jsbin.com/miqiwe/edit?js,console)
	
**Exemplo 02 -  Hoisting Variável**
```javascript
	// Irá imprimir `undefined`, pois a declaração da variável `a` foi elevada para o topo do escopo.
	try {
		console.log(a);
		var a = 2;
	} catch (e) {
		console.error('A variável `a` não foi definida.');
	}
```
[Clique aqui e execulte exemplo acima no JsBin.com](https://jsbin.com/yakibu/edit?js,console)
	
**Exemplo - Hoisting Função**
```javascript
	// Irá imprimir `bar`, em razão da função `foo` ter sido elevada ao topo do escopo.
	foo();
	function foo() {
		console.log('bar');
	}
```	
[Clique aqui e execulte exemplo acima no JsBin.com](https://jsbin.com/fameqa/edit?js,console)
	
## Closure
### Explique o que é, o porquê acontece e como usar.
É uma função interna de outra função, que tem acesso as variáveis globais e também aos argumentos e variáveis da função externa, da mesma maneira que os seus argumentos e variáveis.
Uma função closure tem acesso aos três níveis de escopo(o seu, o da função externa e ao global), mesmo após o retorno da função exterior.

### Cite situações que você usaria.
Utilizaria em botões para ajustar o tamanho do texto de uma página.
	
**Exemplo - Closure**
```javascript
	function myName (firstName) {
		var nameIntro = "Meu nome é ";
	
		// Esta função interna tem acesso a variável da função exterior, incluindo o parâmetro.
		function lastName (theLastName) {
			console.log(nameIntro + firstName + " " + theLastName);
		}
    return lastName;
	}

	var Name = myName ("Helton");
	Name ("Barbosa");
```	
[Clique aqui e execulte exemplo acima no JsBin.com](https://jsbin.com/vuhomo/edit?js,console)
	
## Variável Global
### Explique como se usa uma var Global dentro de uma função.
Uma variável declarada no escopo global pode ser acessada por todos os scripts e funções em uma págian web.
	
**Exemplo - Variável Global**
```javascript
	var myName = "Helton Barbosa";

	// A variável myName pode ser acessada aqui

	function myFunction() {

		// A variável myName pode ser acessada aqui

	}
```
	
## Variável por parâmetro
### Explique o que acontece dentro da função quando um parâmetro é passado e também explique quando uma GLOBAL é passada por parâmetro.
Os parâmetros atuarão como variáveis locais dentro do escopo da função.
Ao passar parâmetros para uma função com argumentos especificados, os valores dos parâmetros serão atribuídos as variáveis locais com o mesmo nome dos argumentos.
O mesmo ocorre quando passamos variáveis globais por parâmetro.
Em funções que não existe a especificação dos argumentos, os valores passados por parâmetros poderão ser acessados pelo array arguments.
	
**Exemplo - Variável por parâmetros**
```javascript
	var a = 1; // variável global

	function main() {
    var b = 2; // variável local
  
    // Função com especificação de argumentos
    // Os parâmetros recebidos por essa função serão inicializados com os nomes especificados nos argumentos
    function soma (c, d) {
        return c + d;
    }
    console.log(soma(a, b));
  
    // Função com argumentos não especificados
    function subtracao () {
    // Os parâmetros passados podem ser acessados dentro da função pelo array arguments
        return arguments[1] - arguments[0];
    }
    console.log(subtracao(a, b));
	}

	main();
```
[Clique aqui e execulte exemplo acima no JsBin.com](https://jsbin.com/jeloci/edit?js,console)

## Instanciação usando uma IIFE - Immediately-Invoked Function Expression
IIFE é um padrão de projeto JavaScript, comumente utilizado em bibliotecas para inserir código em um escopo local.
Permitindo que seu código possa ficar “isolado”, protegendo o escopo de uma porção de código ou módulos do ambiente em que a função foi utilizada.

### Explique como uma variável pode receber um valor de uma IIFE.
Uma variável pode receber um valor através de um retorno de função ou a função em si.
		
**Exemplo - IIFE - Atribuição de valor através de uma IIFE
```javascript
	var v, getValue;
	v = 1;
	
	getValue = function() { return v; }; // Variável getValue recebendo a função em si

	v = 2;
	
	console.log(getValue); // Exibição da função armazenada na variável
	console.log(getValue()); // Execultando função e exibindo o retorno
```	
[Clique aqui e execulte exemplo acima no JsBin.com](https://jsbin.com/zaqebu/edit?js,console)
	
### Explique como passar uma variável por parâmetro para a IIFE e acontece com ela dentro da função.
As variáveis que recebem a função em si como valor podem acessar suas funções e assim passar parâmetros.
	
**Exemplo - IIFE - Passagem de parâmetros para uma função IIFE**
```javascript
	var num = (function(){ var i = 0;
                return {
                  get: function(){ return i; },
                  set: function( val ){ i = val; },
                  increment: function() { return ++i; }
                };
			})();

	console.log(num); // Exibição da função armazenada na variável
	console.log(num.get()); // Zero
	num.set( 3 );  // Alteração de valor de variável local da IIFE
	console.log(num.increment()); // Quatro
	console.log(num.increment()); // Cinco
	console.log(num.get()); // Cinco
```
[Clique aqui e execulte exemplo acima no JsBin.com](https://jsbin.com/picivi/edit?js,console)
	

# Referências

* http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/
* http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/
* https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures
* http://www.w3schools.com/js/js_scope.asp
* Livro: JavaScript: The Good Parts
* https://en.wikipedia.org/wiki/Immediately-invoked_function_expression




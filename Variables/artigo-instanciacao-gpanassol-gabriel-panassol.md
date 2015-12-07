# Artigo
**autor**: Gabriel Panassol

## Hoisting - Hasteamento

Hoisting (hasteamento) é um comportamento onde as declarações de variáveis e/ou função são processadas antes de qualquer código executado.
Para entender melhor, o javascript implicitamente irá declarar primeiramente todas as variáveis antes de iniciar a execução do programa. O termo 
hoisting literamente eleva as declarações de variáveis ao "topo do programa". Vamos ao exemplo:

```
bla = 2
var bla;
// ...

// é implicitamente entendido como:

var bla;
bla = 2;
```

Porém deve entender uma caracteristica, o processo de hoisting somente eleva a declaração da variável o seu valor de inicialização
não será elevador, vamos ao exemplo:

```
var x = 5; // Initialize x

elem = document.getElementById("demo"); // Find an element 
elem.innerHTML = x + " " + y;           // Display x and y

var y = 7; // Initialize y
```

Ao rodar esse exemplo no console do seu navegador será apresentado a mensagem:

```
x is 5 and y is undefined
```

Por essa razão recomenda-se a declaração das variáveis sempre primeiramente em seu código.

## Closure

Antes de falarmos de Closures precisamos entender escopo de variáveis. Escopo de Variáveis é o contexto onde existe a variável, é onde você pode ou não acessa-lá.
Variáveis Locais e Variáveis Globais, vamos ao exemplo de uma variável local com bloqueo de escopo local:

```
var name = "Gabriel";

function showName(){
	var nome = "Natalia";
	console.log(nome) //Natalia
}

console.log(name) //Gabriel
```

Aqui declaramos a variável com o mesmo nome tanto local quanto global porém a variável name dentro da função showName() está alocada em outro espaço, ou seja, não é o mesmo 
valor. Agora veja a seguinte situação:

```
var name = "Gabriel";

function showName(){
	nome = "Natalia";
	console.log(nome) //Natalia
}

console.log(name) //Natalia
```

Nesse exemplo não declaramos var para a variável 'name' dentro da função showName() com isso fica acessivel a variável name global alterando o seu valor. 
Tendo isso em mente podemos entender melhor as funções closure.

O que é closure?

Closure é uma função interior que tem acesso a funções exteriores - cadeia de escopos. O closure tem três cadeias de escopo:

- Acesso ao seu próprio escopo;
- Acesso as variáveis da função exteriore;
- Acesso as variáveis globais

A função interior tem acesso a variáveis declaradas em suas chaves e também acesso aos paramêtros da sua função exterior. Veja:

```
function name(firstName, lastName){

	function showName(){
		console.log(firstName + " " + lastName);
	}
	
	return showName();
}

name('Gabriel', 'Panassol');
```

Nesse exemplo vemos que a função name() recebe dois paramêtros, firstName e lastName, e dentro dessa função temos uma função closure chamada showName() que concatena essas 
duas variáveis. Um detalhe importante veja que a função name retorna showName().

## Variável Global

Todas as variáveis declaradas fora de uma função são variáveis globais. E assim estará disponível para toda a aplicação. Vamos ao exemplo:

```
var a = 1;

function alterarGlobal(novoValor){
	alert('Valor Atual.: ' + a);
	a = novoValor;
	alert('Novo Valor.: ' + a);
	
}

alterarGlobal(3);
```

Nesse exemplo super simples temos a variável global definida com o nome 'a' e atribuindo o valor 1. Na função 'alterarGlobal' atribuimos um novo valor para 'a';
Uma caracteristica interessante no javascript é que toda variável criada sem a chave 'var' passa para o escopo global, exemplo:

```
function showAge(){
	age = 30;
	console.log(age);
}
console.log(age);
```
Veja que após criar 'age' passa a ser acessivel fora da função pois esta no escopo global.


## Variável por parâmetro 

Como mostrado anteriormente as variáveis passadas por parâmetro, ou seja, na assinatura do metodo serão usadas internamente na função. Mesmo as variáveis globais passadas por paramêtro será somente usado internamente e seu valor externo não será modificado, veja:

var global = 3;

function soma(a,b){
	b=a+b;
	return b;
}

console.log(soma(1,global));
//4
console.log(global);
//3

Os escopos de variáveis exige cuidado pois a forma de utilização devem ser bem definidas para não haver erro durante sua utilização, isso pode trazer bugs dificil de "pegar".

## Instanciação usando uma IIFE

O IIFE significa “Immediately-invoked function expression”, conhecido como funções imediatas. Com isso ele irá executar a função exatamente no mesmo momento que foi criado. Isso é encapsulmento. 
Ele é muito utilizado para evitar a poluição do escopo global e possíveis conflitos de variáveis e/ou funções com a mesma nomeclatura. Vamos ao exemplo:

```
var adder = (function() {
	var frase = "";
 	return function(x) { 
 		return frase = 
 		!!frase ? frase.concat(" ", x) : frase.concat(x);
 	}
})();
 
adder("Gabriel"); // "Gabriel"
adder("Panassol"); // "Gabriel Panassol"

frase; //frase is not defined
```

Veja que nesse exemplo a variável 'frase' foi definido no escopo da IIEE e não na função retornada. Com isso esse variável não será acesso fora da função anônima.

Sua sintaxe:

(function doSomething() { /* codigo */ })();

// função anônima
(function() { /* codigo */ })(); // undefined

## Referências

- https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/var
- http://www.w3schools.com/js/js_hoisting.asp
- http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/
- http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/
- https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures
- http://tutsmais.com.br/blog/javascript-2/o-que-e-iife-no-javascript/
- http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/
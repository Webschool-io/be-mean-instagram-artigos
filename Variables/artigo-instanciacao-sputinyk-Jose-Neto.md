# Instanciação de veriáveis no Javascript
**Autor**: José Neto

## Hoisting

*Hoisting* é o comportamento do javascript de mover todas as declarações de variáveis para o topo do escopo atual (o topo do script atual ou a função atual).

Isso éo possível porque os compiladores leem todo o programa para saber que funções e variáveis foram declaradas no código. Só depois disso, é que a a execução real acontece, e aí ele já sabe onde está cada coisa.

#### Como acontece com variável
````
test = 5; // Atribuindo 5 a test
console.log(test); // Utilizando test
var test; // Declarando test
````

#### Como acontece com função
````
var test = 'Teste'; 
  
(function() { 
  console.log(test); // Teste
})();
````

## Closure

Closures são funções criadas dentro de outras funções que possuem variáveis independentes e podem acessar variáveis do próprio escopo, acessar as variáveis da função exterior e acessar variáveis globais. Uma função definida em uma closure 'lembra' do ambiente no qual ela foi criada.

````
function start() {
    var name = "Pikachu"; // name é uma variável local criada por start()
    function mostraPokemon() { // função interna (closure)
        alert (name); // usando variável criada no escopo exterior
    }
    mostraPokemon();
}
start();

// Aonde eu usaria: A maioria do código javascript é event-based, ou seja, algum comportamento é definido a associamos tal comportamento a algum evento (como um clique, por exemplo). Quando estamos associando eventos a vários elementos com um loop, por exemplo, precisamos que algumas variáveis desse comportamento não alterem com o loop, e seja único para cada elemento da iteração (como o iterador, por exemplo). Armazenar esse comportamento em uma closure evitaria que tais valores fossem poluídos em outras iterações, ou se existir alguma variável com o mesmo nome.
````

## Variável Global

Basta declarar ela sem o *var*. Ela estará acessível globalmente. A declaração sem var torna a variável global de forma implícita.

````
teste = "Algum valor";

function foo(){
	console.log(teste); // usará o valor global, mesmo definido fora dessa função
}
````

## Variável por parâmetro

Ao passar uma variável como parâmetro para uma função, caso ela sofra alguma alteração dentro da função, seu valor fora dela permanece inalterado. Dizemos que **o argumento é passado por valor**.

Ex.:
````
function foo(bar){
	bar = 1;
}

var bar = 2;
console.log(bar); // 2
foo(bar);
console.log(bar); // ainda é 2, fora da função nada acontece a ela
````

Obs.: O que foi mencionado acima só é válido se o que for passado for uma variável do tipo primitivo (como String ou números). Se um objeto for passado como argumento para a função, o objeto será passado por referência. Então, se alterarmos um atributo do objeto que foi passado, o objeto de fora da função também sofre essa alteração. (Eu também acho esse comportamento estranho).

## Instanciação usando uma IIFE

IIFE é uma função anônima, ou seja, não possuem um nome (assinatura), e são executadas no mesmo momento de sua instanciação.

Ex.:
````
(function(){
	/* conteúdo da função anônima */
}();
````

#### Como uma variável pode receber um valor de uma IIFE
Basta coloca um *return* dentro da IIFE:
````
var getV = (function(){
	return 'v';
}();
console.log(getV);
````

#### Como passar uma variável por parâmetro para a IIFE
````
(function(param){
	/* param pode ser usado aqui dentro */
}(param);
````
Nesse caso, dentro da função ela está em um outro escopo, e não é sensível ao que a contece com ela fora deste ambiente. E o contrário é recíproco, se ela mudar de valor, quem a originou permanece inalterado.
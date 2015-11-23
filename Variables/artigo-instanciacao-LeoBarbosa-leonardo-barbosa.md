Artigo

autor: Leonardo Barbosa de Oliveira

Prazo: até dia 23 de Novembro de 2015 [PRAZO EXTENDIDO]

Explique, com teoria e código, nesse artigo como o JavaScript cria e instancia as variáveis, seguindo os seguintes tópicos.


##RESUMO

Este artigo tem como objetivo explicar com teoria e código, como o JavaScript cria e instancia as variáveis, seguindo os seguintes tópicos.

## HOISTING

Explique o que é, o porquê acontece e como acontece com variável e função.

Hoisting é o procedimento do JS que move declarações para o topo de seus escopos atuais.
E isso acontece em JS quando declaramos uma variável ou função. Para entendermos o que é 'hoisting' precisamos entender sobre o Escopo em JS.

Escopo é basicamente a seção em que suas declarações estarão visíveis.
No JavaScript, temos o escopo global, que pode ser acessado por qualquer script da página, e o local, que está visível apenas para quem está do lado de dentro. Funções são a principal forma de criar escopos locais no JavaScript.

Ex. Variavel:     
var numero = 5;
function show(){
	console.log(numero);
	var numero = 10;
}

Essa função irá imprimir 'undefined' mesmo a variavel numero sendo declarada antes da função ser executada no escopo global. 
Isso acontece porque toda vez que uma variável é definida, sua declaração é hoisted, mas não sua inicialização.
O que quer dizer que a declaração da variável vai para cima do escopo antes mesmo do código ser executado, mas esta variável não recebe nenhum valor e permanece como undefined.

Ex. Função:
function foo(){
  function bar() {
    return 3
  }

  return bar()

  function bar() {
    return 8
  }
}

console.log(foo());

Essa função ira retornar 8, porque dentro do escopo da função foo, primeiro temos a definição da função bar que já está no topo. Depois do return temos uma outra definição de função com o nome de bar. Apesar de aparecer ao fim do escopo, ela é hoisted — vai para o topo antes da execução. 

**

## CLOUSURE

Clousure é uma função interior que tem acesso a outras variáveis de um função exterior.
O clousure tem três cadeias de escopo: 
- Acesso ao próprio escopo ou seja variaveis declaradas na sua propria função.
- Acesso a variáveis de uma função exterior.
- Acesso a variáveis declaradas no escopo global.

Ex.:  
function welcome (firstName, lastName) {
	var intro = "Seja Bem-vindo ";

	//esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
	function nomeCompleto () {
		return intro + firstName + " " + lastName;
	}

	return nomeCompleto ();
}

welcome ("Leonardo", "Oliveira"); //Seja Bem-vindo Leonardo Oliveira


Closures são usados extensivamente no Node.js; eles são complicados no Node.js assíncrono, com arquitetura não bloqueante. Closures também são frequentemente usados no jQuery e em todo pedaço de código JavaScript que você lê.

Outro exemplo muito comum é Closures em jQuery que provavelmente que ja mexeu com jQuery ja utilizou sem saber que se tratava de uma Closures.

Ex.: 
$(function () {
    var selections = [];
    $(".niners").click(function () { //este closure tem acesso as variáveis de selections
        selections.push(this.prop ("name")); //atualiza a variável selection no escopo da função exterior
    });
});


## VARIÁVEL GLOBAL

A linguagem JavaScript tem dois escopos: global e local.
Uma variável declarada fora de uma definição de função é uma variável global, e seu valor será acessado e modificado em todo o seu script.
Uma variável declarada dentro de uma função é local.

Ex.:
	var nomeCompleto = "Leonardo Barbosa de Oliveira"; //variavel no escopo global

	function primeiroNome(){
		var partsName = nomeCompleto.split(' ');//função utilizando a variavel global
		document.write('Seja Bem-vindo Sr. ' + partsName[0]);// variavel partsName somente no escopo local nao é acessada fora da função
	}


Para que uma variavel global seja utilizada em uma função a variável não pode ser utilizada com o 'var' na frente se nao o JS cria uma variavel local com o mesmo nome, não alterando entao a variavel global

Ex.:

var nomeCompleto = "Leonardo Barbosa de Oliveira"; //variável no escopo global!

function primeiroNome(){
	var nomeCompleto = "Goku";//criando uma variável local, e nao alterando a global
	console.log(nomeCompleto);
}


//Executando

> primeiroNome() //executando a função
> Sayajin

> nomeCompleto //mostrando a variavel fora da funçao que não foi alterada!
> "Leonardo Barbosa de Oliveira"


## VARIÁVEL POR PARÂMETRO

Ao declarar uma funçao em JS podemos ou não passar parâmetros para essa função, parâmetros separados por vírgula.
Ao passarmos parâmetros a uma função serão atribuídos a eles variáveis de mesmo nome dentro da função.
Ao invés de serem inicializados com undefined, são inicializados diretamente pelo arguments.

Ex.:
var primeiroNome = "Leonardo";

function nomeCompleto(sobrenome){
	console.log(primeiroNome +" "+sobrenome);
}

- E se passarmos uma variavel global?

Ex.:
	> var primeiroNome = "Leonardo";
	
	//Executando
	> primeiroNome
	> "Leonardo"
	
	> function mostraNome(primeiroNome){
	 	primeiroNome = "Goku";
	 	console.log(primeiroNome);
	}
	
	//Executando função
	> mostraNome()
	> Goku
	
	//Executando a variavel global
	> primeiroNome
	> "Leonardo"

Como foi explicado ao passarmos parâmetros a uma função serão atribuídos a eles variáveis de mesmo nome dentro da função.
Ou seja se tentarmos alterar a variavel global passada por parametro na função, so alteramos a 'local criada' e não a global realmente.


## INSTANCIAÇÃO USANDO UMA IIFE


O IIFE significa “Immediately-invoked function expression”, mas podemos chamá-lo de função imediata. Como o próprio nome diz, ela executa a função imediatamente depois de criada. Mas por que usar? Encapsulamento! Tenha em mente que variáveis em Javascript têm como escopo a função pela qual elas foram definidas (podem ser acessadas somente dentro da função, jamais fora). Ao criar uma função anônima com execução imediata, podemos criar um escopo temporário para nossas funções e variáveis. Com isso, evitamos poluição no nosso escopo global e possíveis conflitos de variáveis ou funções com o mesmo nome.


Exemplo: considere o código a seguir
	var adicionar = (function() {
	 var minhaFrase = "";
	 return function(x) { 
	 return minhaFrase = 
	 !!minhaFrase ? minhaFrase.concat(" ", x) : minhaFrase.concat(x);
	 }
	})();
	  
	adicionar("Olá"); // "Olá"
	adicionar("Mundo!"); // "Olá Mundo!"
	  
	minhaFrase; // minhaFrase is not defined

Neste exemplo, criamos uma função anônima imediata que retorna uma outra função que concatena uma string na variável chamada minhaFrase. 
Note que a variável criada minhaFrase está no escopo da IIFE e não na função retornada.
Portanto, o minhaFrase não é definido toda vez que invocamos a função adicionar.
Mas o mais importante disso tudo é que o minhaFrase está limitado a escopo da função anônima imediata, não permitindo o seu acesso direto de maneira alguma.


Sintaxe :

(function fazAlgo() { /* codigo */ })(); // undefined

// Agora com função anônima
(function() { /* codigo */ })(); // undefined


- Função imediata com parâmetro

(function fazAlgo(x) { console.log(x); })(1); // 1
// Agora com função anônima
(function(x) { console.log(x) })(1); // 1


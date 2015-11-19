# Artigo
**autor**: Alexandre Teixeira

Este artigo é parte das atividades do curso de Be-Mean-Instagram e irá abordar como o JavaScript cria e instancia as variáveis, seguindo os seguintes tópicos.

## Hoisting

	Hosting é o nome dado a um comportamento que ocorre em JavaScript onde as declarações das variáveis são movidas para o inicio da função ou do código global.
	Como as declarações de variáveis (e declarações em geral) são processads antes de qualquer código seja executado, declarar uma variável em qualquer lugar no código é equivalente a declarar no inicio. O mesmo ocorre com funções que são chamadas antes de sua criação o JavaScript as move para antes da chamada e as processa.
	
	Segue um Exemplo:
	
	```
	poke = "Pikachu";
	var poke;
	
	// Para o JavaScript é equivalente e será processado assim:
	
	var poke;
	poke = "Pikachu";
	```
	O mesmo ocorre com função
	
	```
	...
	MostraPoke();

	function MostraPoke() {
		console.log(poke);
	}
	
	// Na Execução
	...
	function MostraPoke() {
		console.log(poke);
	}
	MostraPoke();
	```
## Closure

	Closure é uma função que é "lembrada" no ambiente em que ela foi criada mesmo após seu fechamento/finalização, fica na memória e pode ser referenciada.
	
	Vamos a um exemplo prático:
	
	-> Temos a necessidade de oferecer ao usuário em tempo de execução a possibilidade de personalização de sua tela aumentando o tamanho da fonte do texto.
	```
	// Estilo
	body {
	  font-family: Helvetica, Arial, sans-serif;
	  font-size: 12px;
	}

	h1 {
	  font-size: 1.5em;
	}
	
	// JavaScript
	function makeSizer(size) {
	  return function() {
		document.body.style.fontSize = size + 'px';
	  };
	}

	var size12 = makeSizer(12);
	var size14 = makeSizer(14);
	var size16 = makeSizer(16);

	document.getElementById('size-12').onclick = size12;
	document.getElementById('size-14').onclick = size14;
	document.getElementById('size-16').onclick = size16;
	
	// Chamada da funcionalidade - Criado Links para alteração do tamanho da fonte.
	<p>Apenas um Parágrafo de exemplo</p>
    <h1>Exemplo de cabeçalho h1</h1>
    <h2>Exemplo de cabeçalho h2</h2>

    <a href="#" id="size-12">12</a>
    <a href="#" id="size-14">14</a>
    <a href="#" id="size-16">16</a>
	
	```

	Nesses links interativos de tamanho de texto podem alterar a propriedade font-size do elemento body, e os ajustes serão refletidos em outros elementos graças à unidade relativa. Shooowwww!!!
	
## Variável Global

	Variável global fica disponível para todo codigo e refere-se a qualquer variável fora do escopo de uma função ou dentro de uma função desde que tenha iniciada não tenha o "var".
	
	Exemplo dentro de uma função:
	```
	function MostraPoke(){
		poke = "Pikachu";
		console.log(poke);
	}
	
	//Aqui a váriavel poke também fica acessível pois foi iniciada e não foi iniciada com "var"
	MostraPoke();
	console.log(poke);
	```
	

## Variável por parâmetro

  Uma variável pode ser passada para uma função facilmente. Basta, na hora da instanciação da função deixar entre parênteses os parametros que ela irá receber, sem possibilidade de explicitar o tipo de variáveis que pode receber (isto da uma maior flexibilidade à linguagem segundo algúns desenvolvedores). Estas variaveis passadas como parametro passam a formar parte do Scope Lexico da função, por tanto, se referenciadas dentro da função, serão encontradas, sem necessidade de ir para outro Scope superior.

	```
	function msg(a){
		console.log(a);
	}

	msg("Teste");
	```

## Instanciação usando uma IIFE

	Uma IIFE é comunmente utilizada para encapsular as variáveis e funções que se encontram dentro delas sem poluir o Scope Global, pois nenhuma referência é criada em ela. Mesmo assim, as IIFE's são funções, portanto podem receber valores por parametros como retornar valores que podem ser armazenados dentro de uma variável.

	Evitar variáveis globais é uma forma de deixar o código menos confuso e a prova de erros, com isso o IIFE (Immediately-Invoked Function Expression) veio como uma forma de "isolar" um bloco de código envolvido entre ( ) do escopo global. Eles basicamente retornam qualquer coisa que pode ser definida em seu interior.

	//variavel mongodb não é acessível no escopo global
	(var mongodb = "oi")

	//varivel mongodb com o seu valor atribuído através de um IIFE
	var mongodb = (function () { return 'oi' })

	Utilizando um segundo parentese ao final da declaração é possível passar uma variavel ou uma lista de variáveis que serão referenciadas por atributos dentro da função invocadora. Também é importante lembrar que mesmo passando uma variável global dentro do IIFE será criada uma variável local.
	
	```
	(function(name) {
		console.log(name);
	})("José da Silva");

	var name = "Maria";

	(function(name) {
		name = "João";
		console.log(name);
	})(name);

	console.log(name);
	```
	
	Saída no console:
	José da Silva
	João
	Maria
	
	--> No final imprimiu "Maria porque não altera o escopo global da variável name;


## Considerações

	Colaborar com este artigo foi interessante pois tive conhecimento do tão complicado porém flexivel e poderosa é a lingagem JavaScript. Nele Foi abordado alguns conceitos básicos sobre variáveis e seu uso de maneira global e restrita.
	
	Grato pela atenção!!! Abraço a todos.

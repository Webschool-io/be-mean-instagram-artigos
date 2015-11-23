# Artigo Instanciação

**Autor:** Marcelo H M Dias

Neste artigo pretendo explicar um pouco sobre alguns conceitos de instanciação no  ecossistema do Javascript. O porquê de alguns comportamentos. E como possibilitar um desenvolvimento sem "dor de cabeça". Afinal a gente está aqui é pra se divertir.

Então mãos a obra!!!

Primeiramente, precisamos entender o que é instanciação. Segundo o site [Dicionário Informal](http://www.dicionarioinformal.com.br/instancia%C3%A7%C3%A3o/), instanciação é:

>Ação de instanciar; Criar algo sempre diferente do seu conceito básico dado o nível de complexidade e capacidade polissêmica de inovar e mudar constantemente para dar conta de atender um conceito para determinado conexto real, se traduz em um novo conceito.

O site ainda completa:

>"A instanciação significa a criação de um nova instância, por meio da criação de uma variante do material"

Podemos chegar a conclusão de que instanciação é o ato criação de novos "objetos" (entenda objeto aqui como um item genêrico), ou mesmo a réplica de um já existente.

Ok, mas como isso é feito em Javascript? Isso vamos descobri a seguir.

- [Artigo Instanciação](#)
	- [Hoisting](#hoisting)
	- [Closure](#closure)
	- [Variável Global](#variavel-global)
	- [Variável por parâmetro](#variavel-por-parametro)
	- [Instanciação usando uma IIFE](#instanciacao-usando-uma-iife)
	- [Considerações Finais](#consideracoes-finais)
	- [Referencial Teórico](#referencial-teorico)

######**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

## Hoisting

Em Javascript *hoisting* é a elevação ou hasteamento das declarações de funções e/ou variáveis, estes são elevados para o topo do escopo (*Escopo* é a área no código que delimita a visibilidade das declarações). Isso ocorre devido a precedência do processamento em relação à execução dos códigos.

Para tornar mais clara a explicação veja o exemplo logo abaixo, ou se preferirem executem o código para ver como a mágica acontece.

```javascript
bar = 10;

function foo () {
	// Função imprime variável inicializada antes da declaração
	console.log(bar);
}

var bar; // Variável declarada após a inicialização.

foo(); // Irá imprimir o valor da variável

console.log(bar + 1); // Impime o valor da soma: 11
```

Como podem perceber, mesmo a iniciação da variavel ocorrendo bem antes de sua declaração (que no JS é feita através da palavra `var` para variáveis locais) o código roda sem problema nenhum. Em outras palavras, nossa variável foi içada para o topo do nosso código em tempo de execução.

Quando utilizamos variáveis, apenas a declaração é *hoisted*. Mesmo que ela seja elevada, o valor com o qual efetuamos sua inicialização não sofre este processo, o que pode gerar alguns erros e/ou eventos inesperados.

```javascript
console.log(foo); //Console imprime undefined

var foo = 5; // Váriavel é declarada e inicializada

console.log(foo); // Imprime o valor de foo: 5
```

No exemplo anterior, o valor é impresso como *undefined*, isso por que a variável `foo` já existe no escopo, apenas não possui um valor atribuido. No segundo console por outro lado, o valor de nossa variável é impresso sem nenhum erro.

Já no caso das funções o hasteamento ocorre de forma diferente. Toda a estrutura do código é *hoisted*, e não apenas seu nome, como acontece com as variáveis.

```javascript
function foo(){
  function bar() {
    return 3
  }

  return bar()

  function bar() {
    return 8
  }
}

console.log(foo())

//Exemplo retirado do site Loop Infinito
```

Neste exemplo o retorno da função `foo()` é `8` issó ocorre, pois as duas funções são hasteadas para o topo do seu escopo, porém a segunda função `bar()` sobrescreve a primeira, devido a ordem da leitura do scripr, havendo portanto, a sobreposição no valor de retorno do nosso código.

Para funções declaradas como expressão `var foo = function () {...}` a regra é a mesma das variáveis, o *hoisting* ocorre apenas na declaração da variável, seu conteúdo permanece na ordem em que foi escrito.

## Closure

Closure (fechamento) são funções que referenciam variáveis intependentes a ela, ou seja, variáveis que se encontram em uma outra cadeia do escopo. Essa cadeia é formada por três níveis, que são:

- Variáveis declaradas dentro do seu escopo;
- Variáveis declaradas em um escopo superior;
- E variáveis globais (falaremos sobre essa última a seguir).

De um forma mais clara, closures são criada quando há a delcaração de uma ou mais funções dentro de outra função. Dessa maneira as funções internas tem acessão não só as variáveis declaradas em seu escopo, mas também as varáves declaradas de sua função "pai".

Veja o exemplo.

```javascript
function init() {
  var name = "Mozilla";
  function displayName() {
    alert(name);
  }
  displayName();
}
init();

// Exemplo retirado do site mozila.org
```

Como podem perceber, a variável `name` não foi declarada no mesmo escopo da função 'displayName', onde está sendo utilizada `alert(name)`, mesmo assim  a função tem acesso a seu conteúdo através da *Closure*.

Um efeito colateral da *Closure* é a permanência do acesso a estes valores mesmo após seu retorno, isso ocorre devido ao armazenamento da referência das variáveis.

No JS, as funções são execultadas utilizando o mesmo escopo de quando foram declaradas, isso siginifica que as funções internas permanecem tendo acesso aos valores declarados nas funções externas, veja o exemplo para facilitar o entendimento.

```javascript
function celebrityName (firstName) {
  var nameIntro = 'This celebrity is ';

  //essa função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
  function lastName (theLastName) {
    return nameIntro + firstName + ' ' + theLastName;
  }
  return lastName;
}

var mjName = celebrityName ('Michael');
//Nesta conjuntura, a função exterior celebrityName foi retornada

//O closure (lastName) é chamado aqui depois da função exterior ter retornado acima
//Sim, o closure continua tendo acesso as variáveis da função exterior e parâmetros
console.log(mjName ('Jackson')); //This celebrity is Michael Jackson

// Exemplo retirado do site JavaScript Brasil
```

## Variável Global

>Em programação, uma variável global é uma variável acessível em todos os escopos de um programa de computador. O mecanismo de interação com variáveis globais é chamada ambiente global. Em contraste o ambiente local é um mecanismo no qual as variáveis são locais (conceito básico de variável local) e sem memória compartilhada. [Wikipédia](https://pt.wikipedia.org/wiki/Vari%C3%A1vel_global)

De todos os conceitos que irei apresentar, este é de longe o mais simples, mas o que pode gerar mais estrago em nosso código JS.

Traduzindo o texto do Wikipédia, variáveis globais são variáveis que podem ser acessadas e/ou alteradas em qualquer lugar do código, independente de onde sejam declaradas, por este motivo devem ser utilizadas em momentos muito específicos, ou seja, evitadas ao máximo.

```javascript
function foo1() {
	// Variável Local
	// Acessada apenas dentro da função foo1
	var bar1 = 1;

	// Variáveis globais
	bar2 = 2;
	window.bar3 = 3
}

// Variável global
var foo2 = 2

```

Mesmo possuindo várias formas para declarar uma variável global, existem algumas diferenças entre elas. No caso das variáveis declaradas dentro de `foo1()`, a variável `bar3` é declarada explicitamente no escopo de `window`  - **E por que window?** Window é o mais baixo nível do nosso escopo, é a rais do navegador, logo todas as variáveis globais são anexadas em `window`. - já a variável `bar2` não é utilizada a palavra reservada `var`, o que por sinal é considerado uma má pratica de programação.

Por outro lado a variável `foo2` utiliza a palavra `var` e também é global. Isso ocorre devido ao local de delcaração. Como esta foi declarada fora de uma função em nosso arquivo, também, pode ser acessada a partir de `window`.

Para entenderem melhor o problema que pode ser gerado com o uso de variáveis globais vejam o exemplo.

```javascript
function defineValor() {
    num = 1;
}

function foo() {
  return num;
}

function bar(value) {
  num = value;
  return num;
}


defineValor();

console.log(bar(2)); // 2
console.log(foo()); // 2
```

No exemplo, mesmo a função `foo()` pretendendo apenas imprimir o valor de `num` declarado anteriormente, o resultado é outro. Isso ocorre devido ao fato de as duas funções referênciarem a mesma variável.

## Variável por parâmetro

Variáveis por parâmetro, são aquelas declaradas entre os parênteses de uma função, permitindo assim o recebimento externo de valores. Essas varáveis são acessíveis apenas no escopo interno dessa função, não sendo possível a utlização direta por outros objetos.

```javascript
function foo(bar, bar2) {
	console.log(bar + bar2);
}

foo(1, 5); // 6
foo(5, 3); // 8
```

Porém, o fato das funções serem declaradas sem algum argumento não quer dizer que não possamos passar parâmetros para elas. Da mesma maneira funções que esperam receber algum parâmentro podem ser utilzadas mesmo sem a passagem do mesmo.

```javascript

// Função sem parâmetro
function foo() {
	console.log(arguments[0]);
}

// Função com parâmetro
function bar(value) {
	var num = 3;
	if (value) {
		num = value
	}
	console.log(num);
}

foo(5);
bar();
```
Na primeira função foi utilizada o parâmetro `arguments` esta variável é implícita em todas funções, utilizamos os colchetes `[]` para definir qual delas iremos utilizar, isso pelo fato de `arguments` ser um array.

Na segunda função não é passado nenhuma função e mesmo assim não ocorre erro na execução. É claro que isso sempre vai depender da estrutura da função.

## Instanciação usando uma IIFE

IIFE (Immediately-Invoked Function Expression - Definição de Função Imediatamente Executável) são funções declaradas sem um nome específico (funções anônimas) e que são executadas no final da declaração.

```javascript
(functon () {
	console.log('Olha como funciona!!!');
})();
```

Pelo fato de não possuir um nome no qual referenciar essas funções são executadas uma única vez. Ainda assm essas funções são muito úteis, visto que, como sabemos, o escopo em javascript é declarado através de funções, vejamos:

```javascript
(function () {
	var sayHi = 'oi'
	console.log(sayHi) // oi
})();

console.log(sayHi) // ReferenceError: sayHi is not defined

// Exemplo retirado do site Tutsmais
```

Neste exemplo tornamos nossa variável privada. Caso precisemos utilizar esta variável em algum código, podemos apenas declara-la em uma variável ou no retorno de uma função.

Para passarmos algum valor para estas funções seguimos a regra da passagem de parâmetros. Declaramos as variáveis dentro dos parênteses, porém aqui isso ocorre me dois locais.

```javascript
(function (str) {
	console.log(str);
})('Viu como é simples!!!');
```

## Considerações Finais

Viu como entender os conceitos nos ajuda a tornar a programação menos custosa. Ainda assim, tem muito chão pela frente. Ecma2015 mal saiu e já estão falando da próxima, a linguagem cresce e nós crescemos juntos.

Espero que tenham gostado, e principalmente entendito um pouco sobre o que tentei passar sobre os conceitos de instanciação, e também como funciona essa incrível linguagem.

Bem, até a proxima! Isso mesmo vai ter próxima.

## Referencial Teórico

1. [Alex Tercete: O problema com as variáveis globais no Javascript](https://alextercete.wordpress.com/2010/01/15/o-problema-com-as-variaveis-globais-no-javascript/)
2. [JavaScript Brasil: Entenda Closures no JavaScript com Facilidade - Eric Oliveira](http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/)
3. [JavaScript Brasil: Escopo de Variável e Hoisting no JavaScript – Explicado - Eric Oliveira](http://javascriptbrasil.com/2013/10/11/escopo-de-variavel-e-hoisting-no-javascript-explicado/)
4. [Loop Infinito: Hoisting e escopo em JavaScript - Caio Gondim](http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/)
5. [MDN - Closures](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures)
6. [Tutsmais: O que é IIFE no JavaScript? - Felquis](http://tutsmais.com.br/blog/javascript-2/o-que-e-iife-no-javascript/)
[Wikipédia - Variável Global](https://pt.wikipedia.org/wiki/Vari%C3%A1vel_global)

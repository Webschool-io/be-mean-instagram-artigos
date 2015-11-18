# Instanciação com JavaScript

Autor: [Gilson Filho](http://github.com/gilsondev)

Nesse artigo vamos explicar sobre a criação de variáveis e sua instanciação, iniciando a teoria sobre escopo de variáveis, como o Javascript comporta a isso. Depois vamos explicar algumas abordagens no Javascript como Hoisting, Closure, o uso de variáveis globais e por parametro, além da instanciação usando uma IIFE.

## Escopo

O escopo, é uma delimitação, um limite para atingir determinada operação. No caso do escopo de variáveis, podemos dizer que são regras que definem o uso de variáveis em um programa, ou de uma forma simples, é o contexto em que a variável existe, especificando onde acessar e se podemos acessar (Javascript Brasil, 2013).

Dessa forma, essa especificação, no Javascript obedece algumas: Variáveis Locais, Variáveis Globais e Hasteamento de Variáveis (conhecido como *Hoisting*). Vamos para elas.


## Variáveis Locais

No Javascript, as variáveis locais ficam a nível de função, ou seja, elas vão estar ser locais quando estiverem dentro de uma função. Ela não um escopo em nível de bloco. Segue um exemplo.

```javascript
function teste() {
	var variavel = "10";

	// Ela vai imprimir normalmente
	console.log("Valor da Variavel: " + variavel);
}

// Disparar um erro, já que essa variável não existe fora do escopo da função acima.
// Uncaught ReferenceError: variavel is not defined
console.log(variavel);
```

## Variável Global

As variáveis globais, são variáveis que podem ser acessados por todo o código em que está, incluindo alterar o seu valor.

```javascript
// Criando uma variavel global
var global = "Joao";

function imprimir() {
	console.log("Meu nome e " + joao);
}

imprimir();

// Mudando o valor
global = "Carlos";

imprimir();
```

Aqui estou criando uma variável global, e imprimindo o seu valor por meio da função `imprimir`. Veja que ele consegue pegar o valor da variável, porque ele é global. Agora veja uma situação, em  que podemos mudar o seu valor dentro de uma função

```javascript
// Criando uma variavel global
var global = "Joao";

function alterar_nome() {
	global = "Carlos";
}

function imprimir() {
	console.log("Meu nome e " + global);
}

imprimir();
alterar_nome();
imprimir();
```

Primeiramente é impresso no console o valor da variável `global` e depois invocamos a função `alterar_nome`, perceba que não colocamos o `var`. O Javascript vai percorrer de dentro para fora dos escopos, até chegar em uma variável que seja do mesmo nome. Caso tiver uma variavel global declarada com `var` na função, ela será usada, caso não encontrar vai para o escopo acima, que nesse caso é o global. Com isso, ele altera a variável, e vemos o seu valor quando imprime novamente.

## Hoisting (Hasteamento)

No caso do hasteamento, todas as variaveis declaradas sofrem esse procedimento. O que acontece é que as variáveis criadas são leventadas e declaradas no topo da função, caso criar dentro dela, ou no topo do escopo global, se forem variáveis globais. As suas atribuições não são levantadas, lembre-se disso.

```javascript
// Criando variavel global
var global;

function imprimeNome() {
	// Imprime: Meu nome e: undefined
	console.log("Meu nome e: " + nome);
	var nome = "Silva";

	// Imprime: Meu nome e: Silva
	console.log("Meu nome e: " + nome);

	// Imprime: Valor da global e: undefined
	console.log("Valor da global e: " + global);
	global = "Teste";

	// Imprime: Valor da global e: Teste
	console.log("Valor da global e: " + global);
}

// Imprime: Teste
console.log(global);
```

Vamos olhar o código acima com calma. Primeiro criamos a variavel `global`, mas sem atribuir nenhum valor, com isso ele será `undefined`. Depois criamos a função `imprimeNome` em que ele vai: 

 - Imprimir o valor de `nome`;
 - Atribuir valor a `global`;
 - Imprimir valor da `global`.

É nesse caso que ocorre o hasteamento. Quando imprime a primeira mensagem de `console.log()` ele mostra que a variavel `nome` é `undefined`. Isso é porque a variável foi hasteada para o topo da função, e por isso não gerou um erro. Depois você atribui um valor, e a segunda mensagem mostra o valor da variável, que é `"Silva"`. A mesma coisa aconteceu com a variável `global`.

No caso das funções, isso também ocorre. Ele faz o hasteamento pelo nome da função, tanto que uma função pode sobrescrever uma variável. Segue um exemplo do site javascriptbrasil.com:

```javascript
//variável e função, ambas são nomeadas "myName"
var myName;
function myName () {
	console.log ("Rich");
}

//A declaração da função sobrescreve a variável name
console.log (typeof myName); //function
```

Aqui ele criou uma variável e função com o mesmo nome. Depois verificamos qual é o tipo da variável, e como a variável não tinha nenhum valor definido, o hasteamento fez com que ele pegasse a referência da função. Tanto que se fizermos isso:

```javascript
myName();
```

Ele vai imprimir o texto que está no `console.log()` da função. Agora, se a variável tivesse com um valor já atribuido, ele não será hasteado para apontar a função com mesmo nome.

## Closure

Esse é um dos recursos mais poderosos do Javascript, e para explicar ela, nada melhor que a definição de [Johel Carvalho](http://programadorobjetivo.co/):

	Closure é dar continuidade à vida de variáveis locais de uma função, após essa função ter sido invocada.

Vamos fazer um pequeno exemplo:

```javascript
function operacoes(numero) {
    function ao_quadrado() {
        return numero * 2;
    };

    function ao_cubo() {
        return numero * 3;
    };

    return {
        ao_quadrado: ao_quadrado,
        ao_cubo: ao_cubo
    };
}

var op = operacoes(2);

// Retorna 4
op.ao_quadrado();

// Retorna 6
op.ao_cubo();
```

No nosso código acima, passamos um parametro `numero` que vai receber o inteiro que for passado. Esse valor vai ser usado pelas funções internas, mesmo depois de invocarmos a função `operacoes`. Quando invocamos o `ao_quadrado`, ele ainda tem acesso ao valor de `numero` e faz o calculo necessario. Ou seja:

	É como se a função interna falasse: "numero", você não está no meu escopo, mas eu dependo de você pra funcionar corretamente. Então, não se preocupe, eu vou lhe deixar vivo até que eu cumpra minha missão na Terra. E para a função externa: "Não se preocupe, o legado que você deixou comigo está bem guardado e ninguém vai vê-lo a não ser eu." - Javascript Closures - Conceiro e Aplicações Práticas (Johel Carvalho, com adaptações)

## Variavel por parâmetro

Quando cria funções no Javascript, você pode passar parametros na sua assinatura:

```javascript
function foobar(arg1, arg2, arg3) {}
```

No caso do seu escopo, depende da forma em que os dados são passados. No exemplo abaixo, os valores atribuidos as essas variaveis, faz com que por padrão, eles tenham um escopo de função:

```javascript
function foobar(arg1) {
	console.log("Arg1: " + arg1);
}

foobar("Teste")

// Dispara erro: Uncaught ReferenceError: arg1 is not defined
console.log(arg1);
```

Agora, quando se usa uma variável global, é diferente. Claro, que passando o valor e usar dentro da função, e fora dela vai conseguir conforme explicamos acima. Vamos ver:

```javascript
var global = "Teste Global";

function foobar(arg1) {
	console.log("Arg1: " + arg1);
}

// Imprime "Arg1: Teste Global"
foobar(global);
```

Mas se tentarmos modificar o valor dentro da função?

```javascript
var global = "Teste Global";

function foobar(arg1) {
	console.log("Arg1: " + arg1);
	arg1 = "Outro Valor";
	console.log("Arg1: " + arg1);
}

// Imprime "Arg1: Teste Global"
// Imprime "Arg1: Outro Valor"
foobar(global);

// Imprime "Teste Global"
console.log(global);
```

Vendo acima, mesmo atribuindo um valor ao arg1, que passamos a variavel global na invocação da função, ele não mantem a mudança. Isso porque o `arg1` é uma variavel local como falamos, assim, ele só recebe a atribuição de valor da variavel `global`. Se desejamos alterar o valor de global, teriamos que fazer assim:

```javascript
function foobar(arg1) {
	// (...)
	global = "Outro Valor";
	// (...)
}

// (...)

// Imprime "Outro Valor"
console.log(global);
```

Agora o valor é mudado.

## Instanciação usando IIFE

IIFE é um padrão de modularização muito usado no Javascript, para fazer o encapsulamento do seu código. Existem situações em que você precisa isolar o seu código, e usando o [Immediately-Invoked Function Expression (IIFE)](http://benalman.com/news/2010/11/immediately-invoked-function-expression) vai ajudar nisso. Veja um exemplo:


```javascript
var foo = "Valor Global";

(function() {
    var foo = "Teste";
})();

// Imprime: Valor Global
console.log(foo);
```

O código acima usa esse padrão, e estamos isolando uma variável, o `foo`. Veja que antes dele criamos outro com o mesmo nome, mas com valor diferente. Depois que isolamos a variavel, nós invocamos imediatamente com `()` no final. Quando imprime o valor de `foo`, ele pega da variável global, porque o que esta dentro da função anonima, esta realmente isolado.

Os parenteses iniciais servem para que seja reconhecida como uma expressão, senão gera esse erro:

```javascript
// Esse erro é justamente por causa do "()" para invocar a funcao anonima
Uncaught SyntaxError: Unexpected token (
```

## Fontes
[Escopo de Variável e Hoisting no Javascript Explicado - Javascript Brasil](http://javascriptbrasil.com/2013/10/11/escopo-de-variavel-e-hoisting-no-javascript-explicado/)

[Javascript Closures - Conceito e Aplicações Praticas](http://programadorobjetivo.co/closures/)

[Quando se deve usar var no javascript? - Stack Overflow](http://pt.stackoverflow.com/questions/2513/quando-se-deve-usar-var-no-javascript#answer-93713)

[Property isolate your variables in Javascript](http://nicoespeon.com/en/2013/05/properly-isolate-variables-in-javascript/)

[Modularização em JavaScript - Tableless](http://tableless.com.br/modularizacao-em-javascript/)

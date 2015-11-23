# Artigo
**autor**: Anderson da Luz Moraes

**Data de entrega: **: 18 de Novembro de 2015

Nesse artigo iremos ver como o JavaScript cria e instancia as variáveis, seguindo os seguintes tópicos:

## Hoisting

Antigamente em linguagens como C, se usavam funções ou procedimentos para dividir um programa, mas havia um problema: as declarações deveriam ficar sempre na frente, funções no início do programa resolveu o problema por um tempo, pois todas as funções e variáveis eram declaradas antes de serem usadas, sendo assim não se tinha erros de referência.
“Por que ler o código de baixo para cima e, não de cima para baixo?”
Tornando as coisas mais amigáveis, agora podemos colocar as definições em qualquer lugar do código e usá-los, mesmo antes de realmente serem definidos.
O que acontece agora é que os compiladores ou até mesmo linguagens runtime leem todo o programa para saber que funções e variáveis você declarou no código. Após isso, a execução real acontece e ele já sabe onde está cada coisa. JavaScript faz exatamente isso, o que chamamos de Hoisting.

###### Exemplo:

```
var a, foo = 'bar';
var bar = function(){
  var foo = 'foo';
  console.log('local: '+foo);
};
bar();
console.log('global: '+foo);
//local: foo
//global: bar 

```
Nesse exemplo, podemos ver que a variável **foo** foi declarada no início e dentro da função, ao chamar a função **bar()** é mostrado no console o valor da variável declarada dentro da função, já quando mandamos mostrar o valor da variável, é mostrado o valor da variável externa.


## Closure

Uma closure ocorre normalmente quando uma função é declarada dentro do corpo de outra, e a função interior referencia variáveis locais da função exterior. Em tempo de execução, quando a função exterior é executada, então uma closure é formada, que consiste do código da função interior e referências para quaisquer variáveis no escopo da função exterior que a closure necessita.


###### Exemplo:

```
var digaSeuNome = function( nome ) {
    var msg = "Olá " + nome + ". Seja bem-vindo!";
    var exibeMensagem = function() {
        console.log( msg );
    };

    exibeMensagem();
};

digaSeuNome("João");    // Olá João. Seja bem-vindo!

```


## Variável Global

Variáveis globais são todas as variáveis declaradas no escopo global, ou seja, fora das funções, e estão disponível para toda a aplicação.

###### Exemplo:

```
var title = 'Be-MEAN';

function titleHome(){
	console.log(title + ' - Home');
}

function titleContato(){
	console.log(title + ' - Contato');
}

titleHome()    // Be-MEAN - Home
titleContato() // Be-MEAN - Contato

```

## Variável por parâmetro

Podemos passar valores para nossas funções, o que chamamos de variáveis por parâmetro, é criada uma instância local dassa variável dentro da função, e toda modificação que a mesma sofrer ficará limitada ao escopo da função.

###### Exemplo:

```
function printName(name){
	console.log('Name: ' + name);
}

printName('Anderson Moraes'); // Name: Anderson Moraes


function printIdade(idade, anos){
	console.log('Hoje você tem ' + idade + ' anos, daqui ha ' + anos + ' anos você terá ' + (idade + anos) + ' anos.');
}

printIdade(34, 10); // Hoje você tem 34 anos, daqui ha 10 anos você terá 44 anos.
printIdade(34, 25); // Hoje você tem 34 anos, daqui ha 25 anos você terá 59 anos.

```

Quando uma variável global é passada por parâmetro ela é referenciada localmente à função passada, então se elas sofrerem alterações dentro da função, não altera o valor da variável declarada globalmente.


###### Exemplo:

```
var saldo = 100;

function cursoBeMEAN(saldo){
	saldo = saldo * 1000;
	return saldo;
}

console.log('Após o curso Be-MEAN o meu saldo será 1000 vezes o atual, meu saldo será de ' + cursoBeMEAN(saldo) + ', pois hoje ele é de ' + saldo); // Após o curso Be-MEAN o meu saldo será 1000 vezes o atual, meu saldo será de 100000, pois hoje ele é de 100

```


## Instanciação usando uma IIFE

IIFE (Immediately-Invoked Function Expression), podemos traduzir como "Definição de Função Imediatamente Executável", ou seja, é uma função que é executada imediatamente logo após ela ser definida.

Há vários locais que podemos usar uma IIFE, porém o mais comum é quando queremos criar um escopo no JavaScript para que as variáveis definidas dentro da função não poluam o escopo global (variáveis definidas no window).

Isso é tudo a respeito de evitar ao máximo criar variáveis globais no JavaScript, e criar escopos quando e onde precisarmos.

###### Exemplo:

```
(function () {
  'use strict'
 
  var hello = 'Fala galeraaaaaa'
  console.log(hello) // Fala galeraaaaaa
})()
 
console.log(hello) // ReferenceError: hello is not defined

```

###### Vamos ver como uma variável pode receber um valor de uma IIFE:

```
var conte = (function () {
  var numero = 0;
 
  return function () {
    return numero ++;
  }
})()
 
conte() // 0
conte() // 1
conte() // 2
conte() // 3
conte() // 4

```

###### como passar uma variável por parâmetro para a IIFE:

No segundo parênteses, onde invocamos a function, podemos passar qualquer parâmetro como se fosse qualquer outra função.


```
(function (string) {
  console.log(string)
})('Fala galeraaaaaa')
 
// Log 'Fala galeraaaaaa'

```

Estas variáváveis passadas serão tratadas como variáveis de escopo daquela função e apenas serão acessíveis dentro dela, o que permite a prática do IIFE para utilizar variáveis em um script sem que interfira com outros.



## Considerações

Entendo que um bom programador além de ter uma boa lógica de programação, deve saber retirar o melhor da linguagem de programação que está utilizando, com isso evitamos muitos problemas, como reescrita de códigos, perda de desempenho, perda de produtividade, etc.
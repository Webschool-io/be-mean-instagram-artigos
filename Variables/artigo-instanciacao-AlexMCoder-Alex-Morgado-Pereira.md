# Artigo - Instanciação em Javascript

**Autor**: Alex Morgado Pereira - [AlexMCoder](https://github.com/AlexMCoder)

##Hoisting

As funções e variáveis, podem ser declaradas depois de terem sido utilizadas. Um exemplo que podemos abstrair essa ideia é usarmos uma função de abrir porta, vejamos: Você pode abrir uma porta - essa seria nossa função, mas não sabemos o que tem por trás dessa porta, que seria exatamente nossas declarações. Veja um exempo prático:

```
abrePorta(); // aqui estamos abrindo a porta sem declarar a função antes;

function abrePorta(){
var porta = "por trás dessa porta existem diversos objetos que você pode utilizar!";
console.log(porta);
}
```

Podemos fazer essa mesma lógica com variáveis. Basta chamar uma váriavel antes de declarar.

```
var porta = "por trás dessa porta existem diversos objetos que você pode utilizar!";
var porta;
```

##Closure

Podemos retornar funções dentro da função externa, antes mesmo de ser executada. A **Clousure** é um tipo especial de objeto, além da função também tem o ambiente onde ela foi criada.

Vejamos um exemplo com base em nossa porta:
```
function casa(){

var porta = "por trás dessa porta existem diversos objetos que você pode utilizar!";

function abrePorta(){
console.log(porta);
}

}

var minhaCasa = casa();
casa();
```

A nossa Clousure é a **minhaCasa**, ela incorpora tanto a função **abrePorta**, quanto a variável **porta**.

##Variável Global

Para que uma variável seja **Global**, temos que declarar ela fora de uma função, com isso, podemos usar essa variável em todos os lugares do código, seja dentro de uma função ou fora. Vale lembrar que seu valor é acessivel e modificável em todo seu programa.

Se criarmos duas variáveis com o mesmo nome, porém, uma dentro da função e a outra fora da função, podemos observar que a função irá dar preferência a variável local - que está dentro da função.

Vejamos um exemplo pratico sobre a variável Global:
```
var carroLigado = true; // essa é a nossa variável global

function carro(nomeDoCarro){

nomeDocarro = false;

if(nomeDoCarro == carroLigado){
return console.log("O carro está ligado");
}else{
console.log("O carro está desligado");
}
}

carro();
```

##Variável por parâmetro

Para que possamos explicar como funciona as varáveis dentro do parâmetro, precisamos primeiro saber para que serve o parâmetro. Os paŕâmetros é uma lista de argumentos que a função irá trabalhar, podemos ter como exemplo, uma função que recebe os argumentos de um determinado formulário ou usuário.

Agora podemos dizer que esses argumentos são consideradas uma lista de variáveis, que a função irá trabalhar. Abaixo temos uma função com um parâmetro como exemplo:

```
function carro(nomeDoCarro){
console.log('Nome do carro é: ' + nomeDoCarro);
}
```

Observe que a função irá imprimir exatamente o que foi passada no parâmetro.

##Instanciação usando uma IIFE

As funções IIFE são executadas assim que são definidas. Há vários locais que podemos usar uma IIFE, o mais comum é quando queremos criar um escopo no JavaScript para que as variáveis definidas dentro da função não poluam o escopo global, vejamos um exemplo abaixo:

```
(function(){
'use strict'

var carroLigado = true; // essa é a nossa variável global

function carro(nomeDoCarro){

nomeDocarro = false;

if(nomeDoCarro == carroLigado){
return console.log("O carro está ligado");
}else{
console.log("O carro está desligado");
}
}
}())
```

Assim iremos manter todas as váriáveis dentro do nosso código, evitando assim conflitos de JavaScripts de outras pessoas, como libs de terceiros e até mesmo curiosos com o DevTools. Podemos evitar também pessoas alterando preços de produtos, simplesmente porque a variável é global.

Para escrever IIFE em JavaScript, basta colocar a função dentro dos parênteses, como no exemplo abaixo:

```
(function(){
//código aqui
}())
```
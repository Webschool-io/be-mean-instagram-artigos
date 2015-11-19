# Artigo - Instanciação com Javascript
**autor**: <a href="https://github.com/filipebsb04">Filipe Fernandes</a>


## Hoisting

No JavaScript é possível instaciar uma varíavel e só depois declarar isso ocorre graças a proprieda chamada Hoisting que literalmente traduzindo seria algo como “elevar” ou “inçar” e é exatamente isso que o JS faz: eleva para o topo do seu escopo atual a declaração da variável ou da função. Para evitar confução é indicado declarar as variáveis no inicio do documento (Escopo Global) ou no início de uma função (Escopo Local).

<code><pre>

teste = 1;
var teste;
console.log(teste);

//As duas opções apresentam o mesmo resultado

var teste;
teste =  1;
console.log(teste);


</pre></code>

**Hoisting de variáveis X Hoisting de funções**
A grande diferença entre os dois tipos de Hoisting é que quando a variável é definida, sua declaração é hoisted, mas não sua inicialização e por sua vez na função tanto a declaração quanto a inicialização são elevadas (hoisted)

<code><pre>

//Imprime: undefined e só depois 2
var a
console.log(a)
a = 2


//Imprime: Bar
Hoisting();
function Hoisting() {
    console.log('bar');
}

</pre></code>


## Closure

Em poucas palavras um closure é uma função dentro de outra que tem acesso a variáveis de uma função exterior. 
Utilizando o closure é possível acessar três cadeias de escopo:
 - Próprio escopo (variáveis definidas entre suas chaves)
 - Acessar as variáveis da função exterior 
 - Acessar as variáveis globais.

</pre></code>

**Hoisting de variáveis X Hoisting de funções**
A grande diferença entre os dois tipos de Hoisting é que quando a variável é definida, sua declaração é hoisted, mas não sua inicialização e por sua vez na função tanto a declaração quanto a inicialização são elevadas (hoisted)

<code><pre>

function MostrarNomes (Nome, SobreNome) {
    var texto = "Seu nome é ";

    // Esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
    function nomeCompleto () {
        return texto + Nome + " " + SobreNome;
    }

    return nomeCompleto ();
}

MostrarNomes ("Filipe", "Fernandes");

//Resultado Seu nome é Filipe Fernandes
</pre></code> 


## Variável Global
Variáveis globais no JS são aquelas que podem ser usadas por todas as funções ou em qualquer lugar do escopo, sendo dentro ou fora de qualquer outra função.

<code><pre>

var global = "Eu sou uma variavel global"; // variavel global

function varLocal () {
	var global = "Eu sou uma variavel local"; // variavel local
	console.log(global);
}

document.write(varLocal());

//Resultado: Eu sou uma variavel local

</pre></code> 


## Variável por parâmetro
Uma função no JS pode ou não receber variáveis como parâmetro porem quando uma função que foi definida para receber parâmetros e não passado todos os valores estes ficam ficam setados como: undefined. Quando uma variável Global é passada por parâmetro o valor é auterado apenas dentro da função e o valor inicial sempre é mantido.

<code><pre>

var x = 20;

function teste(number){
    number = number + 10;
    console.log(number);
}

teste(x); // 30
console.log(x); // 20

</pre></code> 


## Instanciação usando uma IIFE

**IIFE** sigla de "Immediately-Invoked Function Expression" e são função que são execudatas logo em seguida que são criadas não precisando ser chamadas depois. O uso  mais comum é quando queremos criar um escopo no JavaScript para que as variáveis definidas dentro da função não poluam o escopo global.

<code><pre>
(function () {
  'use strict'
 
  var sayHi = 'oi'
  console.log(sayHi) // oi
}())
 
console.log(sayHi)
</pre></code> 



## Considerações

Sabendo um pouco mais sobre instanciação de variáveis em JS é possível escolher a melhor forma de trabalhar mas isso vai depender de cada situação. O importante é saber dominar cadas funcionalidade e explorar o melhor de cada um.

Obrigado.


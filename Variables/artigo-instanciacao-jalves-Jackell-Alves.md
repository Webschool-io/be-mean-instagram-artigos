Instanciação de variáveis no JavaScript

autor: Jackell Alves

Hoisting
Em JavaScript, uma variável pode ser declarada depois de ter sido usada. Em outras palavras; uma variável pode ser utilizada antes de ter sido declarada.

x = 5; // Atribuindo
console.log(x);
var x; // Declarando

Closure
Um closure é uma função interior que tem acesso a variáveis de uma função exterior – cadeia de escopo. O closure tem três cadeias de escopo: ele tem acesso ao seu próprio escopo (variáveis definidas entre suas chaves), ele tem acesso as variáveis da função exterior e tem acesso as variáveis globais.

var a = 10;
function f() {
    var b = 20;
    if ( x ) {
        var c = 30;

Variável Global
A variável global é acessível tanto de dentro de uma função como também estando fora dela. O funcionamento é através de encapsulamento dos blocos, liberando ou não o acesso das variáveis.

var n = 1;
function exibe()
{
    alert(n);
}
function altera()
{
    n = 2;
}
exibe();
altera();
exibe();

Variável por parâmetro
Os parâmetros se usam para mandar valores à função, com os quais ela trabalhará para realizar as ações. São os valores de entrada que recebem uma função. Por exemplo, uma função que realizasse uma soma de dois números teria como parâmetros a esses dois números. Os dois números são a entrada, assim como a saída seria o resultado

function escreverBoasvindas(nome){
    document.write("<H1>Ola " + nome + "</H1>")
} 

Instanciação usando uma IIFE
O IIFE significa “Immediately-invoked function expression”, mas podemos chamá-lo de função imediata. Como o próprio nome diz, ela executa a função imediatamente depois de criada.

var adder = (function() {
var myPhrase = "";
return function(x) { 
return myPhrase = 
!!myPhrase ? myPhrase.concat(" ", x) : myPhrase.concat(x);
}
})();
	  
adder("Olá"); // "Olá"
adder("Mundo!"); // "Olá Mundo!"
myPhrase; // myPhrase is not defined





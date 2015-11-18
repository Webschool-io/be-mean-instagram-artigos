##Instanciação com Javascript  
#####Autor: Rony Souza


##Hoisting
O nome hoisting, elevação ou até mesmo içamento, significa levantar ou suspender algo através de um aparato mecânico. Em bom português, significa usar o guindaste para elevar um objeto. E é isto o que acontece em JavaScript quando declaramos uma variável ou função. Sua declaração é  “elevada” para o topo do escopo.


```Exemplo:

var a = 1
console.log(a) //=> 1

if (true) {
  (function() {
    var a = 2
    console.log(a) //=> 2
  }())
}

console.log(a) //=> 1

```

É só um termo especificado, pois ele arranca as declarações até o topo do seu escopo.

##Closure
Uma closure ocorre normalmente quando uma função é declarada dentro do corpo de outra, e a função interior referencia variáveis locais da função exterior. Em tempo de execução, quando a função exterior é executada, então uma closure é formada, que consiste do código da função interior e referências para quaisquer variáveis no escopo da função exterior que a closure necessita.

```Exemplo:

function showName (firstName, lastName) {
    var nameIntro = "Your name is ";
 
    //esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
    function makeFullName () {
        return nameIntro + firstName + " " + lastName;
    }
 
    return makeFullName ();
}
 
showName ("Michael", "Jackson"); //Your name is Michael Jackson

```
##Variável Global

As variáveis globais são aquelas que foram declaradas fora de uma definição/escopo de função, seu valor e propriedades ficam acessíveis em todo o escopo do seu código. Para acessar uma variável global dentro de uma função, basta fazer o acesso diretamente a variável como se ela já existisse no ecopo atual.

```Exemplo:

function metodo01(){
    x = 1+2; 
    variavel_global = x; 
    document.write(x + "<br>") // RESULTADO IGUAL A 3
}

function metodo02(){
    var valor = variavel_global; 
    document.write(valor + "<br>"); // RESULTADO UNDEFINED 
}

metodo01();
metodo02();
variavel_global++;
document.write(variavel_global + "<br>");

```
##Variável por parâmetro

Quando uma variável é passada como parâmetro para uma função, é como se fizessemos uma declaração de uma variável dentro do escopo daquela função.

```Exemplo:

function falar(palavra) {
  console.log(palavra);
}
function executar(funcao, valor) {
  funcao(valor);
}
executar(falar, "Oi JavaScript!");

```
##Instanciação usando uma IIFE

IFE significa "Immediately-invoked function expression. São funções que são executadas no mesmo momento em que são definid

```Exemplo:

(function (str) {
  var vader = str;
  console.log(vader);
}('I need a friend...')); // imprime 'I need a friend...'

console.log(vader); // ReferenceError: vader is not defined

```

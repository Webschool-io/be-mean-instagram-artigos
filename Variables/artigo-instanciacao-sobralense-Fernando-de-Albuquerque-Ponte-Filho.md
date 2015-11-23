# Artigo
**autor**: Fernando de Albuquerque Ponte Filho

Este artigo consta a teoria e código do JavaScript no processo de criação e instanciação das variáveis.

## Hoisting

Hoisting é o comportamento padrão do interpretador JavaScript de "elevar" (ou içar) as declarações para o topo.
É quando a variável pode ser declarada **depois** que ela é usada, ou seja, ela pode ser usada **antes** de ser declarada, contanto que a variável esteja no mesmo escopo (mesma função).

***Nas Variáveis:***
```js
a = "teste"; // Atribuição da variável
var a; // Declaração da variável
console.log(a);
// Saída: "teste";
```
Embora não seja obrigatório, declarar variáveis no topo, mesmo no JavaScript, é uma forma de iniciar uma boa prática de programação. Evitando confusão de membros da comunidade que acessarem o código.

***Nas Funções:***

O mesmo pode ser aplicável nas funções, mas apesar de ambos serem nomeados de Hoisting, o comportamento muda um pouco. Diferente das variáveis, a declaração de função não apenas "eleva" o nome da função. Ele também "eleva" sua definição.

```js
// Chama a função antes da declaração:
estaElevada();

// Declara a função:
function estaElevada() {
    console.log("Sim!");
}
// Saída: Sim!
```
Mas é importante notar que este comportamento ocorre somente para declaração de função...
```js
function name();
```
...não funciona para função-expressão.
```js
var name = function();
```
***Exemplo:***
```js
// Exibe: "Função Elevada!"
elevada();

// Exibe: TypeError: undefined is not a function
naoElevada();

function elevada() {
    console.log("Função Elevada!");
}

var naoElevada = function () {
    console.log("Função não Elevada!");
};
```
O que ocorreu é que a função naoElevada() tem sua declaração (var naoElevada) sob hoisting (como "undefined"), mas não a definição de sua função, daí o "Co TypeError".

## Closure

Closure são funções internas que têm acesso a variáveis de uma função exterior, ou seja, é ter uma função dentro de outra função e poder usar argumentos e variáveis da função pai.

***Exemplo 1:***
```js
function teste() {
  var a = "Oi";
  function mostrar() {
    console.log(a);
  }
  mostrar();
}
teste();
// Saída: "Oi"
```
***Exemplo 2:***
```js
function teste(arg1, arg2){
  var meio = 1.5;
  function mostrarSequencia(){
    return arg1 + ", " + meio + ", " + arg2;
  }
return mostrarSequencia();
}
teste(1,2);
// Saída: "1, 1.5, 2"
```
Podemos usar em situações que exijam reaproveitamento de uma variável, mas dentro da mesma função.
Um contador é um exemplo, onde ele usa a variável "counter" para incrementar a próxima adição, caso necessário.
```js
var add = (function () {
  var counter = 0;
  return function () {return counter += 1;}
})();

add();
add();
add();
// Retorna "3"
```

## Variável Global e Local

Variáveis locais, como o nome sugere, são as vistas somente dentro do escopo em que foi criada, não importando que possua o mesmo nome de uma variável global.

A maneira simplificada de mostrar o que é variável global é quando a definimos com "var = nomeDaVariavel" no top-level, ou seja, sem estar dentro de nenhuma função.

***Exemplo 1:***
```js
var a = 0; //Variável global
function teste(){ // >> Início do escopo da função.
  var a = 1; // Variável local
  console.log(a);
} // >> Fim do escopo da função.
teste();
// Saída: "1" (local)

console.log(a);
// Saída: "0"(global)
```
 Variáveis locais a uma função, são acessíveis dentro dela e dentro de qualquer função definida em seu corpo (Closure).

 Para alterar a variável global, basta remover o "var" da variável local, onde ele vai buscar a referência fora do escopo.

 ***Exemplo 2:***
 ```js
 var a = 0; //Variável global
 function teste(){
   a = 1; // Variável local, mas sem a partícula var"
   console.log(a);
 }
 teste();
 // Saída: "1" (local)

 console.log(a);
 // Saída: "1"(global agora modificada)
 ```

## Variável por parâmetro

Podemos passar uma variável por parâmetro para dentro de uma função, usando a lógica:
```js
function nomeDaFunção (argumento1, argumento2, argumento3, ...){
  //Codigo a ser executado.
};
```

***Exemplo:***
```js
function dizAlunos(numero){
  var numero = numero * 2;
  console.log("Incrível! Você já tem " + numero + " alunos.");
}
dizAlunos(10);
//Saída: "Incrível! Você já tem 20 alunos."

var numero = 50; // Define variável fora do escopo da "dizAlunos()".
dizAlunos(numero);
//Saída: "Incrível! Você já tem 100 alunos."

console.log(numero); // Chamada da variável "numero".
// Saída: 50.
```

Note que ao ser declarada variável global "numero", não pôde ser modificada e ainda pode ser usada como argumento para a função. Importante destacar que a variável global passada por parâmetro para uma função, não tem seu valor alterado. A única maneira de alterar conforme mostrado no Exemplo 2 do tópico anterior.

Observe no exemplo a seguir:
```js
var a = 10; // Variável global
function teste(b){
  a = 100; // Não usamos a declaração VAR.
  console.log("Número: " + b);
}
teste(a);
//Saída: "Número: 10"

console.log(a);

```

## Instanciação usando uma IIFE

IIFE (Immediately-Invoked Function Expression) é traduzido ao pé da letra como "Expressão(ou Definição) de Função Invocada Imediatamente" é uma função anônima(sem nome) em forma de expressão, que é invocada imediatamente. Bastante usada quando quer isolar variáveis e até funções num escopo "privado", sem popular fora da função.

***Exemplo 1:***
```js
;(function () {

  var digaOi = 'Oiii'
  console.log(digaOi) // Saída: "Oiii"
}())

console.log(digaOi) // Saída: Uncaught ReferenceError: digaOi is not defined
```

Para passagem de variável por parâmetro, basta colocar os parâmetros entre parênteses na última abertura. Ele vai buscar o parâmetro lá e continuar a execução da função.

***Exemplo 2:***
```js
;(function(seuNomeAqui) {
  console.log("Olá, eu sou " + seuNomeAqui);
} ('Fernando') );
//Saída: "Olá, eu sou Fernando"
```

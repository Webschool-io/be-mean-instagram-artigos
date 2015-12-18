# Artigo - Instanciação
**Autor**: Emídio de Paiva Neto - [Zowder](https://github.com/Zowder)
**Data**: 1450037907907


Uma breve explicação de como o JavaScript cria e instancia as variáveis seguindo os seguintes tópicos:

## Hoisting

Hoisting é o procedimento do JavaScript que levanta as  declarações para o topo de seus escopos atuais. Vamos pensar no seguinte trecho de código:


```js
   var nome = "João";
      function mostraNome() {
      console.log(nome);
      var nome = "João da Silva";
    }
    mostraNome();
```
 Quando chamamos a função mostraNome, ela irá retornar undefined. Isso acontece porque para que todas as declarações sejam feitas faz-se necessário que seu código seja compilado, só depois da compilação (Compile Time), é que seu código começa a ser executado (Runtime).
 > Durante a primeira etapa o seu código é checado, busca-se as declarações e todas as variáveis são criadas em seu escopo.
> Após essa análise  feita linha a linha, inicia-se a etapa de Runtime, responsável pela execução do seu código.

Com base no que já foi dito, qualquer variável que esteja em qualquer parte do código, passará pelo processo de hoisting, e irá para o topo do seu respectivo escopo.

>No trecho de código acima, temos:
  - A variável do escopo global nome vai receber a string "João"
  - Depois executa-se a função mostraNome. -> mostraNome();
  - Dentro da função temos o console.log(nome);
  - Esse nome está se referindo a variável nome do escopo mostraNome, que até o momento é undefined. ( No momento de Compile Time a variável declarada ganha o valor de undefined e a função tem seu conteúdo compilado, e é isso que chamamos de hoisting. )
  - Só depois a variável nome do escopo mostraNome vai receber algum valor.

O código correto para esse caso seria:

```js
    var nome = "João";
    function mostraNome() {
     var nome = "João da Silva";
     console.log(nome);
    }
    mostraNome();
```
Para que não tenhamos problemas com hoisting, o melhor a se fazer é declarar variáveis no início do escopo.

## Closure

Closure basicamente é, de forma simplificada, uma função que acessa variáveis de sua função parente. No exemplo abaixo temos:

```js
function start() {
    var message = "hello world";

    function display() {
        console.log(message);
    }

    display();
}
```
No exemplo, a função display tem acesso a variável message, mas a variável message foi declarada no escopo da função start. A função 'de dentro' (inner) tem acesso a 'função de fora'(outer). Logo, a função display é considerado um closure, para que isso ocorra, faz-se necessário que a função tenha não só a referência de suas variáveis locais e variáveis globais mas também as referências das variáveis não locais que não estão nem em seu escopo e nem no escopo global, mas que de alguma forma se tornaram acessível a ela.

## Variável Global

Uma variável declarada fora de uma definição de função é uma variável global, e pode ser acessível e modificável em todo o código. Um exemplo simples seria:

```js
var idade = 19;
var ano = 2015;
function dataNasc(){
  return  ano - idade;

}
dataNasc();
```
Vale ressaltar que uma variável local pode ter o mesmo nome de uma variável global, mas ambas são totalmente separadas e o valor de uma não afeta o da outra.

## Variável por parâmetro

```js
function dataNasc(ano,idade){
  return (ano - idade);
}
dataNasc(2015,19);
```
No caso acima, quando chamo a função passando os argumentos para ela, o JavaScript faz com que este argumento seja tratado como parâmetro da função e que possa ser utilizado como uma variável dentro da função.

## Instanciação usando uma IIFE

> O IIFE significa “Immediately-invoked function expression”.

Um ponto importante para ressaltar sobre IIFE é o encapsulamento. Ao criar uma função anônima com execução imediata, poderemos criar um escopo temporário para as funções e variáveis. Sendo assim, evita-se a poluição do escopo global.

Precisamos antes de tudo transformar uma function em uma Function Expression e em seguida um conjunto de parênteses que serão como uma função que poderemos passar parâmetros.

```js
(function doSomething() {
  /* codigo */
  })();
```
 Utilizando parâmetros:

```js
(function doSomething(x) {
  console.log(x);
  })("Jose"); // Vai exibir Jose
```
Variável que recebe o valor de uma IIFE, deve-se utilizar o return dentro da IIFE.

```js
  var doSomething = function() { return "done"; }(); //
```
A maior vantagem de usar IIFEs é porque as IIFEs faz com que todo o código que está em volta do seu script não tenha acesso as variáveis que você define.

Passando uma variável como parâmetro para a IIFE

```js
var nascData = function() {
    return "Olá";
}();

(function doSomething(parametro) {
  console.log(parametro);
  })(nascData); // "Olá"
```

# Considerações
Com base no que já foi dito, é sempre bom entendermos como as coisas funcionam no JavaScript, pois partindo desse princípio fica mais fácil compreender como tudo ocorre por trás dos bastidores. Os tópicos abordados nos mostram algumas features do JavaScript, que abre espaço para padrões incríveis.


## Referências


http://tutsmais.com.br/blog/javascript-2/o-que-e-iife-no-javascript/
https://brunosouza.org/functions-no-javascript.html
http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/?trace=1519021197&source=single
https://www.codecademy.com/pt-BR/courses/javascript-beginner-pt-BR
https://www.todoespacoonline.com/w/2014/04/funcoes-em-javascript/
http://pt.stackoverflow.com/questions/1859/como-funcionam-closures-em-javascript
https://www.kenneth-truyers.net/2013/04/20/javascript-hoisting-explained/

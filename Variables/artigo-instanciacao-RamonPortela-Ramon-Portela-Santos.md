# Artigo
**autor**: Ramon Portela Santos


## Hoisting

Hoisting é o comportamento padrão da linguagem JavaScript que move todas as declarações de variáveis ou funções para o topo do escopo. O hoist acontece como resultado de como é implementado o interpretador de código, onde primeiro o interpretador procura pelas declarações das variáveis e funções e só depois executa o código. Então mesmo que uma variável seja usada antes de ser declarada o interpretador vai ler a declaração dela primeiro.

**exemplo:**
```
numero = 10;
var numero;

funcaoQualquer();

function funcaoQualquer(){
   console.log("faz qualquer coisa");
}

```

## Closure

Closure é uma função que é salva em memória para ser referenciada depois mesmo após ter encerrado sua execução.

**exemplo:**
```
function gerarMensagem(nome, idade) {
  var messagem = nome + ", que tem " + idade + " anos de idade, disse olá!";

  return function exibir() {
    console.log(messagem);
  };
}

// gera o closure
var exibeMensagem = gerarMensagem("Bob", 47);

// faz uso da função salva
exibeMensagem();

```
De acordo com o que aprendi das pesquisas eu usaria para encapsular funções dentro de outras, fazendo que elas sejam privadas e não acessíveis de qualquer parte do código.

## Variável Global

Variável global é uma variável que está diponível e acessível e todo o código, podendo ser usando dentro de funções ou não.

**exemplo:**
```
var numero = 10;

mostraNumero();

function mostraNumero(){
   console.log(numero);
}

```

## Variável por parâmetro

Quando um parâmetro é passado para uma função o valor do parâmetro é passado para dentro de uma variável da função que é chamado de argumento, caso não seja passado parâmetro e a função possua argumento o valor do argumento será undefined. Caso uma variável global seja passada por parâmetro, o argumento da função receberá uma cópia do conteúdo da variável e todas as alterações feitas valerão somente dentro do escopo da função, e a variável global permace inalterado. 

**exemplo:**
```
var numero = 10;

function funcaoQualquer(num){
   num = num + 5
   console.log(num);
   //vai ser exibido 15
}

modificaNumero(numero);

console.log(num);
//vai ser exibido 10

```

## Instanciação usando uma IIFE

IIFE é sigla para “Immediately-Invoked Function Expression", ela funciona como uma função normal porém é executada de forma imediata, ela é bastante usada quando é preciso fazer algo com uma variável temporária, porém fora de uma função já declara e também sem ter que instanciar uma nova variável global.

Uma variável pode receber o valor de uma IIFE se a IIFE retornar um valor e a variável receber o resultado da função.

Para passar uma variável como parâmetro para uma IIFE basta coloca-la nos parenteses após a declaração da IIFE, ela é copiada pra dentro do argumento da IIFE e seu valor só é alterado no escopo da função mantendo seu valor fora dela.

**exemplo:**
```
var numero = (function(num) {
    return num;
}(10));
console.log(numero);

```

## Considerações

Com as pesquisas para este artigo sobre instaciações foi possível aprender mais sobre algumas particularidades do como funcionam as coisas no JavaScript 
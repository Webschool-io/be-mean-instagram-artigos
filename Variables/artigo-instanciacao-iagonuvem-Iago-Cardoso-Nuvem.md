# Instanciação de Variáveis no Javascript
autor: **Iago Nuvem**

## Resumo

Neste artigo veremos algumas funcionalidades fundamentais utilizadas em Javascript, essa que é uma linguagem com grande e crescente popularidade. Serão abordados os tópicos: Hoisting, Clousure, Variável Global, Variável por parâmetro e Instanciação usando uma IIFE, com exemplos de utilização.

## Hoisting
Para entender Hoisting, primeiro é preciso saber diferenciar : Quando Declaramos a Variável, Quando Inicializamos a Variável e Quando Atribuimos valor à uma variável declarada. Veja abaixo:

- Declarando variavel e atribuindo valor
```js
var x; // Declara uma variável x

x = 69; // Atribui um valor à variavel x
```
- Inicializando uma variável
```js
 var x = 69; // Note que a variável já foi declarada atribuindo um valor.
```

Quando uma variável é declarada no Javascript ela é por padrão movida para o topo do escopo atual, ou seja, independente de onde a variável seja declarada no código será o mesmo que declarar no topo, isso é chamado Hoisting. O Exemplo Abaixo mostra mais claramente o funcionamento do Hoisting: 

- Exemplo Hoisting
```js
x = 69; // Atribui um valor à variavel x
y = 0; // Atribui um valor à variavel y

console.log(x); // imprime 69
console.log(y); // imprime undefined

var x; // Declara a variável x
```
Mas como assim? A Variável recebe um valor antes de ser declarada? 
*YES MOTHAFUCKA!* pura psicodelia, independente de onde você declara a variável, ela sempre é movida para o Topo do escopo. No exemplo anterior, somente a variavel **x** foi impressa, pois a variável **y** não foi declarada.

## Closure

Um Closure é basicamente uma função criada dentro de outra função (ou função interior) que tem acesso aos parâmetro da função exterior. Em geral, o Closure tem acesso a variáveis do seu próprio escopo, do escopo da função exterior e também as variáveis globais, Resumindo, é uma suruba de funções. Veja o exemplo abaixo:

- Exemplo Closure:
```js
function funcaoExterior(paramFuncaoExterior) {
    var variavelFuncaoExterior = "Variavel da Função exterior.";

    function funcaoInterior() {
        var variavelFuncaoInterior = "Variavel da Função interior.";

        return paramFuncaoExterior + variavelFuncaoExterior + variavelFuncaoInterior;
    }

    return funcaoInterior();
}

console.log(funcaoExerior("Paramentro função exterior"));
// será impresso "Parametro função exterior, Variavel da Função exterior, Variavel da Função interior."
```

## Variável Global

Uma variável Global é uma variável "publica", ela é declarada fora de um escopo, e quando criada pode ser acessada e alterada de dentro de qualquer escopo do código, é tipo uma mulher da vida, qualquer um pode meter a mão. Veja o Exemplo:

- Exemplo Variável Global
```js
var variavelGlobal = 419; // Isso é uma variável global

function editarVariavelGlobal(){
	++variavelGlobal;
    var variavelnGlobal = 1; // Isso NÃO É uma variável global
}

editarVariavelGlobal();

console.log(variavelGlobal); // será impresso 420
```

## Variável por parâmetro

Quando passamos variáveis por parâmetro fazemos referência a variáveis que são passadas na assinatura de uma função. Uma variável passada por parâmetro pode ser acessada somente dentro do escopo da própria função, mesmo que a variável seja Global. Veja o Exemplo:

- Exemplo Variável por Parâmetro:
```js
function funcao(param1, param2){
	alert('Parametro 1: ' + param1 + ' / Parametro 2: ' + param2);
}

console.log(funcao('Argumento1', 'Argumento2')); // será impresso 'Parametro 1: Argumento1 / Parametro 2: Argumento2
```

- Exemplo Variável por Parâmetro + Variável Global:
```js
var variavelGlobal = 'Global';

function funcao(param1){
	param1 = 'Tentando alterar variavel Global';
}

funcao(variavelGlobal);
console.log(variavelGlobal); // será impresso 'Global', pois a função só modifica a variável recebida como parametro dentro de seu próprio escopo, tendeu meu filho?

variavelGlobal = 'Agora sim eu altero essa porra';
console.log(variavelGlobal); // será impresso 'Agora sim eu altero essa porra' , pois a variável foi modificada fora de um escopo. vlwflw
```

## Instanciação usando uma IIFE

O IIFE significa *Immediately-invoked function expression*, também conhecido como função imediata. Como o nome sugere, ela é uma função que é executada imediatamente após a sua criação. Podemos utilizar IIFE para encapsular funções e variáveis, assim evitamos poluição no nosso escopo global e possíveis conflitos de variáveis ou funções com o mesmo nome.
Instanciação usando uma IIFE pode ser utilizando passando parâmetros ou não. Veja os paranauê abaixo:

- Exemplo IIFE sem parâmetros:
```js
(function() {
	alert('be MEAN!!');
})();
```

- Exemplo IIFE como parâmetros:
```js
(function(param) {
    alert('O negócio é ' + param);
})('comer c#$ e buc#$# !');
```

## Referências Bibliográficas

- Javascript Brasil <*http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/*>
- iMasters <*http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife*>
- w3Schools <*http://www.w3schools.com/js/js_hoisting.asp*>
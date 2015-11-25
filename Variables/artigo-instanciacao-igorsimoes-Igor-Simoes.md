#Artigo

autor: Igor Simões - igorsimoes

Artigo sobre instanciação em javascript para o curso Be Mean. Neste artigo irei abordar alguns comportamentos que a linguagem de programação javascript oferece.

##Hoisting
Hoisting(em português, elevação) é o nome dado a um comportamento da linguagem de programação Javascript em que todas as declarações(variáveis ou funções) são direcionadas para o início do escopo atual [1], ou seja, o compilador eleva todas as declarações para o topo do escopo e todas as execuções vão para o final do escopo de acordo com a ordem em que foi escrito. Ainda não entendeu? Vamos ao exemplo:

```Javascript
//exemplo 1 com variável
// saida : undefined Igor
try{
    console.log(nome);
    var nome = "Igor";
    console.log(nome);
}catch(e){
    console.log("erro!");
}
```

No exemplo 1, ao inicializar o script, a declaração da variável vai para o topo do escopo (apenas a declaração da variável nome e não sua inicialização). Por isso, ao entrar no bloco try, é mostrado no console que o valor da variável nome é undefined (ou seja, ela existe porém não foi inicializada). Na próxima linha, a variável é inicializada dentro do bloco try e é mostrado no console o valor desta variável, que neste caso é a string "Igor".

```Javascript
//exemplo 2 com função
// saida : "Hello World"
function helloWorld(){
    return msg;
    function msg(){
        return "Hello World";
    }
}
var hoisting = helloWorld();
hoisting();
```

No exemplo 2, se não houvesse o hoisting ocasionaria em erro, mas o que ocorre ao executar a função helloWorld é o retorno da função msg(), que neste caso é a string "hello world". Isto ocorre devido ao hoisting que segue uma ordem do escopo:

1. Declarações de variáveis: declaração da variável hoisting
2. Declarações de funções: declaração das funções helloWorld() e msg()
3. Execuções: neste caso, return msg é executado por último

##Closure
Closures são funções internas que têm acesso as variáveis de uma função exterior, sejam elas do próprio escopo, de funções exteriores(incluindo os parâmetros da função) ou variáveis globais [2].

```javascript
//exemplo 3
//saida : 20
var valor = 10;
function soma(x, y){
    var z = 4;
    function setY(){
        y = 4;
        return valor+x+y+z;
    }
    return setY();
}
soma(2,3);
```
No exemplo 3, temos duas funções aninhadas, sendo que a função interna setY() tem acesso direto com os parâmetros da função externa soma(x,y) e com a variável z, além do acesso a variável global valor. Closure geralmente é usado para emular métodos privados[3], segue o exemplo:

```javascript
//exemplo 4
// saida : "Igor"
function meuNome(nome) {
    // variavel privada
    var _nome = nome;
    // closure
    return {
        getNome: function() {
            return _nome;
        }
    };
};
meuNome("Igor").getNome();
```
##Variável Global
Variáveis globais são as variáveis que são declaradas fora de funções, ou seja, enquanto as variáveis locais declaradas em funções são removidas ao fim da execução do bloco da função, as globais permanecem durante toda a aplicação e podem ser acessadas por todos os escopos, segue o exemplo:
```javascript
//exemplo 5
//saida : 20 10 5 ReferenceError: z is not defined
var x = 10;
y = 5;
function soma(){
    var z = 5;
    return x+y+z;
}
soma();
console.log(x);
console.log(y);
console.log(z);
```
No exemplo 5, a soma ocorreu normalmente resultando em 20 (x+y+z) e as variáveis globais (x e y) permanecem ativa no escopo, enquanto a variável local z, foi removida ao fim da execução da função.

##Variável por parâmetro
Podemos usar variáveis locais e globais nas funções em javascript. Uma variável local é passada para uma função em forma de parâmetro, essa variável só estará disponível no escopo até o fim da execução da função, exemplo:
```javascript
//exemplo 6
function soma(x,y){
    return x+y;
}
soma(1,2); //saida : 3
console.log(x); //saida: ReferenceError: x is not defined
```
Variáveis globais também podem ser utilizadas como parâmetro seguindo o mesmo conceito, porém seria como criar uma variavel local dentro do escopo da função referenciando o valor da variável global, segue o exemplo:
```javascript
//exemplo 7
//saida : 10
var z = 10; //inicialização da variável global
function soma(x){
    console.log(x);
}
soma(z); //variável global como parâmetro
```

##Instanciação usando uma IIFE
O termo IIFE significa “Immediately-invoked function expression”, mas podemos chamá-lo de função imediata [4], ou seja, a função é imediatamente executada depois de ser criada e segue este padrão:
```javascript
(function()
    {
        //bloco a ser executado imediatamente
    })();
```
As funções IIFE se diferenciam das outras funções pois são executadas apenas uma vez, enquanto as outras funções podemos invoca-las quantas vezes quisermos e em qualquer lugar do código.
Podemos passar variáveis por parâmetro para nossa IIFE, segue o exemplo:
```javascript
//exemplo 8
//saida : "Igor Simões" "Igor"
var nome = "Igor";
(function(nome){
    nome = "Igor Simões";
    console.log(nome);
})(nome);
console.log(nome);
```
No exemplo 8, temos uma variável global chamada nome que é passada como parâmetro para a nossa IIFE. Veja que ela é uma variável global e que está sendo modificada dentro do escopo IIFE, porém é mantida o mesmo valor fora da função, ou seja, ela só é alterada localmente na função IIFE.


## Referências

[1] http://wenndersantos.net/2014/12/javascript-hoisting/
[2] http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/
[3] https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures
[4]http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/

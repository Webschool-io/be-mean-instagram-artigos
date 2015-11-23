# Artigo Instanciação
**autor**: Davidson Nascimento

## Hoisting
Em Javascript temos escopos a nível de função, que são delimitadas apenas e exclusivamente por chaves (`{}`).
Em Javascript hoisting significa elevar a declaração de uma variável ou função para o topo do escopo em que a mesma foi declarada. Todo tipo de declaração é processado antes que qualquer código seja executado. O que significa que independente do local no código que houve a declaração, é o equivalente a declarar no início do escopo, sua declaração é hoisted. 

É importante frisar que apenas a declaração vai para o topo, enquanto quaisquer atribuições ou outra lógica são deixadas no local de implementação.
Esta característica do Javascript ocorre devido o motor compilar este código antes que de fato ele seja interpretado. Parte da fase de compilação é encontrar e associar todas as declarações com seus escopos apropriados, o que é o coração do escopo léxico. 
#### Variável Hoisting
Quando definirmos uma variável, sua declaração é considerada hoisted. Sua inicialização continua no local que definirmos. Isso afeta de forma negativa no aspecto de que ao tentarmos utilizar esta variável antes de inicializá-la será retornado um `undefined`.
```js
var b
console.log(b) // undefined
console.log(a) // undefined
var a = 'beMean'
console.log(a) // beMean
```
O código acima é entendido como:
```js
var b
var a
console.log(b) // undefined
console.log(a) // undefined
a = 'beMean'
console.log(a) // beMean
```
#### Função Hoisting
Não apenas o nome da função é hoisted como também todo seu corpo, ou seja, mesmo executando uma função antes mesmo de ser definida seu escopo poderá ser utilizado, isso porque tanto o nome da função como seu corpo são hoisted.
```js
falaOi()
function falaOi() {
    console.log('oi')
}
```
## Closure
Em Javascript todas funções são clousures: são objetos e tem um encadeamento de escopo associado, ou seja, uma função interna acesse argumentos e variáveis de uma função externa, além de também ter acesso a seus próprios argumentos e variáveis. 

Ao criar uma função, você automaticamente cria um closure, onde as atuais variáveis locais e externas ficam disponíveis dentro do novo escopo.

Esta característica ocorre devido a utilização do escopo léxico pelo Javascript. Isso infere que as funções são executadas usando o encadeamento de escopo que estava em vigor quando foi definida.
Veja abaixo sua utilização, onde são simuladas variáveis privadas e públicas:
```js
function clousure() { // início do escopo da função clousure
    var a // variável local 
    this.publico = 'publico' // variável pública 
    var privado = 'privado' // variável privada
    var privado2 = function(x) {
        a = x;
    }
    this.publico2 = function(x) {
        privado2(2)
        return a * x;
    }
}
var obj = new clousure();
console.log(obj.publico) // imprime publico
console.log(obj.privado) // imprime undefined
console.log(obj.privado2) // imprime undefined
console.log(obj.publico2(3)) // imprime 6
```
## Variável Global
Variável que seu valor será acessível e modificável em todos os escopos. 
Uma variável local deve ser declarada utilizando o ```var``` e pode ter o mesmo nome que uma variável global, mas a alteração do valor de uma não afeta a outra, porém se esquecer de utilizar o ```var``` a variável é incluída no contexto global e não no contexto de execução atual.
```js
    var a = 'sou global';
    b = 'sou global';

    beMean();

    function beMean(){
        b = 'sou global alterado';
        c = 'sou global';
        var x = 'sou local';
    }

    for ( var d = 10 ; d < 20 ; d++ ) {
        var e = 10;
    }

    console.log(a) // sou global
    console.log(b) // sou global alterado
    console.log(c) // sou global
    console.log(d) // 20
    console.log(e) // 10
    console.log(x) // ReferenceError: x is not defined
```
## Variável por paramêtro
No JavaScript, a função pode ser chamada com qualquer número de argumentos, não importa quantos deles estão listados.
```js
function exibir(a,b) {
    console.log(
        arguments, // imprime todos argumentos, inclusive aqueles sem nome
        "a=" + a + 
        ", b=" + b
        )
}
exibir(1)     // imprime { '0': 1 } 'a=1, b=undefined'
exibir(1,2)   // imprime { '0': 1, '1': 2 } 'a=1, b=2'
exibir(1,2,3) // imprime { '0': 1, '1': 2, '2': 3 } 'a=1, b=2'
```
O acesso a argumentos sem nome é possível devido a existência de uma especial pseudo-matriz dentro de cada função chamada ```arguments```. Ele contém todos os parâmetros pelo seu número: ```arguments[0]```, ```argumentos[1]```, etc. Segue abaixo exemplo: 
```js
function digaOi() {
    for (var i = 0; i < arguments.length; i ++) { // itera sobre a quantidade argumentos 'ocultos'
        console.log("Oi," + arguments[i])
    }
}
digaOi("Davidson", "beMean"); // imprime Oi,Davidson // Oi,beMean
```
Quando uma variável GLOBAL é enviada como parâmetro, qualquer tipo de alteração que esta venha sofrer no escopo da função será visto posteriormente a chamada da função, ou seja, qualquer modificação no valor realizada será refletido de fato no valor da GLOBAL. 

Caso contrário, a variável GLOBAL não venha a ser enviada via arqumento para a função, as alterações sofridas ficarão restritas apenas ao escopo da função. 

Segue abaixo exemplo onde possuímos duas GLOBAIS, ```var a``` e ```var b```, onde a primeira é enviada como parâmetro e sofre alteração no escopo da função o que modifica de fato seu valor "real", como foi realmente definida. Já a segunda GLOBAL sofre alteração no escopo da função e posteriormente a chamada a função mantém seu valor original.
```js
var a = 'global'
var b = 'global'

function digaOi(a) {
    
    console.log("a="+a) // imprime a=global
    console.log("b="+b) // imprime b=global
    
    a = 'local'
    b = 'local'
    
    console.log("a="+a) // imprime a=local
    console.log("b="+b) // imprime b=local
}

digaOi(a); 
console.log("a="+a) // imprime a=global
console.log("b="+b) // imprime b=local
```
## Instanciação usando  uma IIFE
A linguagem JJ fornece apenas duas maneiras de definir uma função - usando Declaração de função ou expressão de função. Recursos de linguagem como funções anônimas, IIFE não fornece nova sintaxe para definir uma nova função, mas eles fornece benefícios adicionais para o seu programa como funções de retorno, fechos, âmbito público e privado, etc.

IIFE significa “Immediately-invoked function expression”, que é normalmente referido como *expressão de função imediatamente invocado*, são funções anônimas que são executadas assim que são definidas. É um padrão de design JavaScript comum usado pela maioria das bibliotecas populares (jQuery, Backbone.js, Modernizr etc.) para colocar todo o código biblioteca dentro de um âmbito local. 

Sua utilização é mais comum quando queremos criar um escopo no JS para que as variáveis definidas dentro da função não poluam o escopo global, afim de evitar ao máximo criar variáveis globais, e criar escopos quando e onde precisarmos.

Segue abaixo como podemos realizar a assinatura deste:
```js
// Qualquer um dos dois padrões seguintes pode ser utilizado para chamar imediatamente
// uma expressão de função, utilizando o contexto de execução da função para criar "privacidade".
(função () {/ * código * /} ()); // Crockford recomenda este
(função () {/ * código * /}) (); // Mas este também funciona

// Porque a ponto de os parênteses ou operadores coagindo é disambiguate
// entre as expressões de função e declarações de funções, eles podem ser
// omitido quando o analisador já espera uma expressão
var i = função () {return 10; } ();
verdadeiro && função () {/ * código * /} ();
0, função () {/ * código * /} ();

// Se você não se preocupam com o valor de retorno, você pode salvar um byte por apenas prefixar
// a função com um operador unário.
! Função () {/ * código * /} ();
~ função () {/ * código * /} ();
- função () {/ * código * /} ();
+ função () {/ * código * /} ();

// tiliza a palavra-chave `new`.
nova função () {/ * código * /}
nova função () {/ * código * /} () // Só precisa de parens se passando argumentos
```
Como pode ser visto, a função ```IIFE``` antes de sua execução () é essencialmente o mesmo como incluimos a função ```beMean``` antes de sua execução (). Em ambos os casos, a função de referência é executada com ```()``` imediatamente.
```js
function beMean() { .. }
// 'beMean' expressão de referência da função, então  '()' executa
beMean();
// 'IIFE expressão de referência da função, então '()' executa
(function IIFE(){ .. })();
```
No exemplo abaixo somos capazes de usar o valor de retorno de IIFE para fornecer acesso a ```fazerPagamento``` e ```calculoSobreTempo``` funções sem fornecer os seus detalhes de implementação interna.
```js
var folhaDePagamento = (function() {
    var audit = function(info) {
        console.log("Auditando informação: " + info) // imprime Auditando informação: + argumento 'info'
    }
    var fazerPagamento = function () {
        console.log("Na função fazerPagamento"); // imprime Na função fazerPagamento
        audit("sim");
    };
    var calculoSobreTempo = function () {
        console.log("Na função calculoSobreTempo"); // imprime Na função calculoSobreTempo
        audit("sim");
    };
    return {
        FazerPagamento : fazerPagamento,
        CalculoSobreTempo : calculoSobreTempo
    }
}());
console.log(folhaDePagamento) // imprime { FazerPagamento: [Function], CalculoSobreTempo: [Function] }
folhaDePagamento.FazerPagamento()
folhaDePagamento.CalculoSobreTempo()
```
## Considerações
As funções são as unidades mais comum do escopo em JavaScript, sendo que são a única unidade do escopo. Variáveis e funções que são declaradas dentro de outra função são essencialmente "escondidas" do escopo global, ou seja, pertence a um bloco arbitrário (em geral, o par de chaves ```{..}```) do código, em vez de apenas para a função.

O escopo JavaScript funciona sobre o modelo de "escopo léxico". A fase de compilação lexing é essencialmente capaz de saber onde e como todos os identificadores são declarados e, portanto, prever como eles vão ser acessados durante a execução.

Compreende-se que as características do JavaScript vão muito além do conteúdo abordado neste artigo. Cada tópico aqui colocado nos demonstra o poder que o JavaScript possui no desenvolvimento, e o conhecimento de suas principais características podem tornar o desenvolvimento não só prazeroso, mas também pode nos agragar no aspecto de desempenho.

Não há como contornar o fato de que, sem as otimizações, o código é executado mais lentamente.

JavaScript é incrível, é foda! 
## Referências
1. [http://tecnologia-simples.blogspot.com.br/2012/04/variaveis-globais-e-locais-javascript.html](http://tecnologia-simples.blogspot.com.br/2012/04/variaveis-globais-e-locais-javascript.html)
2. [https://www.turbosite.com.br/blog/entenda-o-que-sao-closures-em-javascript](https://www.turbosite.com.br/blog/entenda-o-que-sao-closures-em-javascript)
3. [https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures)
4. [http://nodebr.com/cuidado-com-a-armadilha-do-escopo-de-variaveis-em-javascript/](http://nodebr.com/cuidado-com-a-armadilha-do-escopo-de-variaveis-em-javascript/)
5. [http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/](http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/) 
6. [http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/http://www.w3schools.com/js/js_hoisting.asp](http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/http://www.w3schools.com/js/js_hoisting.asp)
7. [https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/var#var_hoisting](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/var#var_hoisting)
8. [http://pt.stackoverflow.com/questions/32251/vari%C3%A1vel-global-em-javascript](http://pt.stackoverflow.com/questions/32251/vari%C3%A1vel-global-em-javascript)
9. [http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/](http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/)
10. [http://benalman.com/news/2010/11/immediately-invoked-function-expression/#iife](http://benalman.com/news/2010/11/immediately-invoked-function-expression/#iife)
11. [http://www.bryanbraun.com/2014/11/27/every-possible-way-to-define-a-javascript-function](http://www.bryanbraun.com/2014/11/27/every-possible-way-to-define-a-javascript-function)
12. [https://brunosouza.org/functions-no-javascript.html](https://brunosouza.org/functions-no-javascript.html)
13. [http://javascript.info/tutorial/arguments](http://javascript.info/tutorial/arguments)
14. [http://prasadhonrao.com/javascript-functions-part-4-immediately-invoked-function-expression-iife/](http://prasadhonrao.com/javascript-functions-part-4-immediately-invoked-function-expression-iife/)

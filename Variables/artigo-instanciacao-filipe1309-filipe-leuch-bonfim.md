# Instanciação
**autor**: Filipe Leuch Bonfim

## Hoisting

*Hoisting* é um comportamento do Javascript, em que a declaração de variáveis e funções são **elevadas** (*hoist*) ao início do escopo. E diferente de outras linguagens (C, C++, Java,...) o escopo no Javascript só existe a nível de função.

O *Hoisting* permite que as declarações, e inicializações, ocorram em qualquer parte do código, e não apenas no início, ou seja, no Javascript não existe a obrigação de se declarar uma variável, ou função, antes de usá-la.
```javascript
// Exemplo 1
alert(minhaVar); // undefined
minhaVar = 5;
alert(minhaVar); // 5
```

Se executarmos o código acima obteremos `undefined` como resultado do `alert()`, isto acontece porque a declaração da variável foi elevada. Segue abaixo o resultado do *hoisting*:
```javascript
// Exemplo 2
var minhaVar;
alert(minhaVar); // undefined
minhaVar = 5;
alert(minhaVar) // 5
```
#### Variáveis
Quando se trata de uma variável, somente a declaração é elevada (*hoisted*), o mesmo não se aplicando para sua inicialização. Por isto, no exemplo 2, a inicialização da variável `minhaVar`, só terá efeito no segundo `alert()`.

#### Funções
Para funções, o comportamento é diferente, pois tanto o nome quanto o corpo da função são elevados.

```javascript
// Exemplo 3
minhaFuncao(); // Olá mundo!
function minhaFuncao() {
    alert(‘Olá mundo!’);
}
```

No código abaixo (exemplo 4), é demonstrado o que realmente acontece no exemplo 3.
```javascript
// Exemplo 4
function minhaFuncao() {
    alert(‘Olá mundo!’);
}
minhaFuncao(); // Olá mundo!
```

No ECMAScript 6 este comportamento pode ser alterado, se definirmos as variáveis com `let` ao invés de `var`, assim o escopo da variável possa a ser a nível de bloco e o `hoisting` não ocorre.


## Closure

Uma **closure** é uma função que mantém as referências das variáveis dos escopos de níveis acima, ou seja, é uma função que “lembra” o ambiente em que foi criada. Isto acontece porque as closures acessam as variáveis das funções de nível acima por referência, e não valor.

Algo interessante, é que as variáveis dos escopos acima que a closure acessa, são ‘automaticamente privadas’, é como  acontece em orientação a objetos, quando um objeto contém uma variável privada e alguns métodos para acessa-la e/ou altera-la. A partir desta caracteristica é possível simular variáveis privadas em Javascript, criando funções específicas para que possam ser manipuladas.

Para facilitar a compreensão, serão apresentados 3 exemplos de closures:

Exemplo 1: Uma variável do escopo acima é utilizada.
```javascript
// Exemplo 1
function funcaoExterna(paramExterno) {
  var variavelLocalExterna = "Mundo";

  function funcaoClosure() {
    console.log("Tenho acesso a tanto o parâmetro: " + paramExterno + ", quando a variável local:  " + variavelLocalExterna);  
  }

  funcaoClosure(); // executando a closure
}

setLocation ("Olá");  // Tenho acesso a tanto o parâmetro: Olá, quando a variável local:  Mundo
```

Exemplo 2: Uma variável do escopo acima é utilizada, mesmo que a função do escopo acima já tenha sido executada e retornado.
```javascript
// Exemplo 2
function funcaoExterna(paramExterno) {
  var variavelLocalExterna = "Mundo";

  function funcaoClosure() {
    console.log("Olá, tenho acesso a tanto o parâmetro: " + paramExterno + ", quando a variável         local:  " + variavelLocalExterna);  
 }

  return funcaoClosure;
}

var ola = funcaoExterna("Olá");

ola();   //  Tenho acesso a tanto o parâmetro: Olá, quando a variável local:  Mundo
```

Exemplo 3: As variáveis do escopo acima são referenciadas na closure.
```javascript
// Exemplo 3
function funcaoExterna() {
  var variavelLocalExterna = "Olá";

  return {
    get: function() { console.log(variavelLocalExterna ); },  
    set: function(novaVariavel) { variavelLocalExterna = novaVariavel; }
  };
}

var minhaFuncao = funcaoExterna();

minhaFuncao.get();           // Olá
minhaFuncao.set(‘MEAN’);
minhaFuncao.get();    // MEAN
```

## Variável Global

Variáveis **globais** são variáveis que foram declaradas e/ou inicializadas fora de uma função, e podem ser utilizadas ou alteradas em qualquer lugar do código. Para se utilizar uma variável global dentro de uma função, basta utilizá-la da mesma forma que uma variável local, não havendo a necessidade de declaração. Se o valor da variável global for alterado dentro da função, esta alteração terá efeito fora do escopo da função, ou seja, o valor será realmente alterado, como podemos reparar no exemplo abaixo.

```javascript
// Exemplo 1
varGlobal  = 1; // varável global

function minhaFuncao() {
    varGlobal = 2;
    console.log(varGlobal); // 2
}

console.log(varGlobal); // 1
minhaFuncao();
console.log(varGlobal); // 2
```


## Variável por parâmetro

Um parâmetro pode ser passado de duas maneiras em uma função: **por valor**, em que, se o valor do parâmetro for alterado, isto não irá se refletir fora da função, sendo este parâmetro uma cópia do valor original; ou **por referência**, em que a alteração do valor dentro da função, irá alterar o valor original fora dela. No Javascript, os objetos e arrays são passados por referência, e os tipos numéricos e strings por valor.

Algo muito importante de notar, e que as vezes é ignorado, é que se uma variável global for passada  por parâmetro, ela terá o ***mesmo*** comportamento de uma variável local, ou seja, se for um objeto ou array será passada por referência, e ser for numérica ou string será passada por valor.

```javascript
// Exemplo 1
meuArr = [1,2]; // referência
meuObj = {'ola': 'Mundo'}; // referência
minhaStr = "Ola mundo!"; // valor
minhafuncao(meuArr, meuObj , minhaStr);
alert(meuArr + ', ' + meuObj.ola + ', ' + minhaStr); // 1,2,3, Dae, Ola mundo!

function minhafuncao(arr, obj, str) {
    arr.push(3);
    obj.ola = 'Dae';
    str= 'Foi';
    alert(arr + ', ' + obj.ola + ', ' + str); // 1,2,3, Dae, Foi
}


```

## Instanciação usando uma IIFE

**IIFE** signigica “*Immediately-Invoked Function Expression*” (em português “Expressão de Função Imediatamente Invocada”), e como o nome já diz, é uma função que é executada no mesmo momento da sua definição. Através da utilização de uma IIFE, o trecho de código contido nela, fica encapsulado, não afetando outras partes do código fora da IIFE, ou seja, tornando o código independente do local onde for utilizado.

No exemplo 1, podemos reparar como é a sintaxe de uma IIFE,  de maneira simples, basta adicionar um conjunto de parênteses adicionais na função.

```javascript
// Exemplo 1
(function(){})();
```

Segue abaixo alguns exemplos, com variações na maneira de se declarar uma IIFE.
```javascript
// Exemplo 2
var varIife = function () { return '1ª forma' }();  
var varIife = (function () { return '2ª forma' }())
var varIife = (function () { return '3ª forma' })()
```

Para que uma variável receba um valor de uma IIFE, um `return` deve ser definido dentro da IIFE.
```javascript
// Exemplo 3
var fn = function () { return 'oi' }();
console.log(fn); // oi
```

Para passar uma variável como parâmetro em uma IIFE, deve-se passar este valor no momento em que a IIFE é declarada, e também passa-lo como parâmetro da função.
```javascript
// Exemplo 4
(function ($) {
console.log($.fn.jquery)
}(jQuery))
```

E dentro da função podemos utilizar esta variável normalmente, como um parâmetro de uma função.
```javascript
// Exemplo 5
var teste = 'Olá';
var meuArr = [1,2];

(function (string, arr) {
    string += ' Mundo!';
    arr.push(3);
    console.log(string); // Olá mundo
    console.log(arr);  // Array [ 1, 2, 3 ]
}(teste, meuArr));

 console.log(teste); // Olá
 console.log(meuArr);  // Array [ 1, 2, 3 ]
```

## Bibliografia
http://www.w3schools.com/js/js_hoisting.asp

https://developer.mozilla.org/pt-BR/docs/Glossary/Hoisting

http://code.tutsplus.com/tutorials/javascript-hoisting-explained--net-15092

http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/

http://stackoverflow.com/questions/3725546/variable-hoisting

http://tableless.com.br/elevacao-ou-javascript-hoisting/

http://programadorobjetivo.co/hoistinglet-e-escopo-de-variavel-em-javascript/

http://www.sitepoint.com/javascript-closures-demystified/

http://www.sitepoint.com/demystifying-javascript-closures-callbacks-iifes/

https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures

http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/

http://stackoverflow.com/questions/111102/how-do-javascript-closures-work

http://pt.stackoverflow.com/questions/1859/como-funcionam-closures-em-javascript

http://javascriptissexy.com/understand-javascript-closures-with-ease/

http://javascript.info/tutorial/closures

https://www.youtube.com/watch?v=yiEeiMN2Khs

https://www.youtube.com/watch?v=R_ZvxMyFSCU

https://brunosouza.org/functions-no-javascript.html

http://pt.stackoverflow.com/questions/32251/vari%C3%A1vel-global-em-javascript

https://www.codigofonte.net/dicas/javascript/709_funcao-com-parametro

http://tutsmais.com.br/blog/javascript-2/o-que-e-iife-no-javascript/

http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/

http://desenvolvimentoparaweb.com/javascript/javascript-iife-conteiner-de-codigos/

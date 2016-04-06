# Artigo - Instanciação
**Autor**: Jefferson da Silva - [JetiX](https://github.com/JetiX)

**Data**: 1458438344616

Neste artigo vamos explicar o que é e como usar hoisting, closures, variáveis globais, varíavies por parâmetro e instanciação usando IIFE.


## Hoisting
Em javascript temos o que é chamado de *function scope*, que significa que as variáveis ficam no escopo da funcão em que são declaradas. Sabendo disso podemos entender ainda outra caracteristica do javascript: o hoisting, que é o fato de as variáveis declaradas em uma funcão se comportarem como se tivessem sido declaradas no topo dessa funcão. Isso significa que isto:
```js
var bumba = 'Não é igual a pumba.';

function bumbleble() {
  console.log(bumba); // Mostra Undefined, porque a variável declarada e iniciada abaixo se comporta como se tivesse sido declarada no inicio da funcão, substituindo a variável de escopo maior de mesmo nome

  var bumba = 'Meu boi';
  console.log(bumba); // Mostra 'Meu boi', já foi atribuído um valor à variável
}
```
Se comporta como se tivesse sido escrito assim:
```js
var bumba = 'Não é igual a pumba.';

function bumbleble() {
  var bumba; // Variáveis declaradas no topo da funcão
  console.log(bumba); // Mostra Undefined

  bumba = 'Meu boi';  // Valor colocado na variável
  console.log(bumba); // Mostra 'Meu boi', já foi atribuído um valor à variável
}
```

Tendo esse comportamento em mente alguns programadores javascript consideram boa prática declarar todas as variávels no topo da funcão, para tornar evidente o jeito que elas se comportam na linguagem.

## Closure
Antes de vermos o que é closure, é bom sabermos que o javascript usa *lexical scoping*(escopo léxico), ou seja: o escopo das funcões não é o escopo que está em vigor onde elas são invocadas, mas sim o escopo de onde elas foram definidas. O conjunto **funcão + o seu escopo**(escopo de onde a funcão foi definida) é chamado de closure.

Então temos que:
```js
// Este é o arquivo outro.js
var saudacao = 'Olá jovem gafanhoto!';
exports.digaOi = function() { // O exports aqui apenas declara o que vai ser importado pelo require em app.js
  console.log(saudacao);
};
```
```js
// Este é o arquivo app.js
var saudacao = 'Oi :3';
var outro = require('./outro.js'); // importando o outro.js, que possui a funcão digaOi()

outro.digaOi(); // Mostra 'Olá jovem gafanhoto!' no terminal
```
Observamos que a funcão *digaOi* mostra no console o valor da varíavel *saudacao*. Porem em nosso exemplo temos 2 variáveis saudacao: a do arquivo *outro.js* e a do nosso arquivo *app.js*. A funcão é definida em outro.js, importada em app.js com *require* e em seguida invocada.

Nessa situacão a variável mostrada pelo console é a de *outro.js*, pois foi nesse arquivo que a funcão foi definida, então o "closure" dela se limita ao escopo em que ela foi definida e não acessa as variáveis dos outros locais em que for invocada. Para ter acesso às variáveis de fora da sua 'closure', variáveis de onde a funcão é invocada ou seus valores, é necessário que elas sejam passadas para a funcão como argumentos.


## Variável Global
Uma variável global é a variável definida no fora de qualquer funcão ou uma variavel definida sem o uso da palavra chave *var*. Ela é acessível a todas as funcões do arquivo. Exemplos:
```js
var variavelGlobal = "Sou Global";

function printGlobal() {
    console.log(variavelGlobal);
}

printGlobal(); // Mostra "Sou Global"
```

```js
function declaraGlobal() {
  globalSemQuerer = "Sou global pois fui declarada sem a palavra chave var.";
}

function printGlobal() {
    console.log(globalSemQuerer);
}

declaraGlobal(); // Declara a variável global
printGlobal(); // Acessa a variável global, mesmo ela tendo sido criada em outra funcão.
```

## Variável por parâmetro
Quando variáveis primitivas são passadas como parâmetros para uma funcão, é criada uma copia do valor da variável no escopo da funcão, ou seja, modificacões das variáveis dentro da funcao não afetarão, fora da funcão, o valor das variáveis usadas como parâmetro. Já objetos quando são passados como parâmetros para uma funcão, não é criada uma copia do objeto, ao invés disso se passa uma copia do "endereco" de onde o objeto está para a funcão, ou seja: ao contrário das váriaveis primitivas, as modificacões feitas dentro da funcão irão afetar o objeto fora da funcão, como se observa no código:
```js
// Variáveis primitivas
var a = "Sou uma string.";
var b = 1;
var c = true;

// Objeto
var d = { valor: "Objeto do mal" };

// Funcão que modifica os parâmetros passados
var variosArgumentos = function(a, b, c, d) {
  a = undefined;
  b = undefined;
  c = undefined;
  d.valor = undefined;
};

// Funcão que apenas mostra, no terminal, o valor das variáveis passadas para ela
var printAll = function() {
  for(var i = 0; i < arguments.length; i++) {
    console.log(arguments[i]);
  }
}


variosArgumentos(a, b, c, d);
printAll(a, b, c, d);
/* Resultado mostrado na tela:

Sou uma string.
1
true
{ valor: undefined }

Ou seja, apenas o objeto teve seu valor afetado fora da funcão, as variáveis primitivas tiveram apenas as suas copias dentro da funcão afetadas.

*/
```

## Instanciação usando uma IIFE
IIFE(Immediately-Invoked Function expression) é uma funcão que é declarada e executada imediatamente. Existem outras formas de fazer IIFEs, mas por convencão elas são feitas de duas maneiras:
```js
(function(){ /* código */ })();
(function(){ /* código */ }());
```
IIFEs são úteis para criar seguranca e privacidade no código, instaciando os valores apenas dentro da funcão. Exemplo:

```js
var valorMaisDois = (function() {
  var valorPrivado = 0;
  return function() {
    console.log(++valorPrivado);
  };
})();

valorMaisDois(); // 1
valorMaisDois(); // 2
valorMaisDois(); // 3
console.log(typeof(valorPrivado)); // Mostra undefined, não é possível acessar valorPrivado, apenas a funcão tem acesso a variável
```

Além do uso citado, IIFEs também podem ser usadas para a funcão não poluir ou ser influenciada pelo o escopo global e para não precisar definir uma variável toda vez que a funcão for executada.

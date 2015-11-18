# IIFE

O IIFE ou Immediately-invoked function expression, é umas das formas de se declarar uma função no javascript, para entender melhor `como uma variável pode receber um valor de uma IIFE` temos que entender antes alguns conceitos.
O primeiro deles é como funciona a `IIFE`, que será a nossa função que retorna um valor, após isso temos que entender como funciona o `Variable assignment` que é a forma de atribuir uma função a nossa variável, e por fim, podemos juntar esses dois conceitos e criar nossa função auto executável que vai retornar um valor para nossa variável.

### IFFE (função auto executável)

A função no javascript na sua sintax mais básica como: `function functionName(){ /* code */ }`, precisa obrigatoriamente ser chamada para que o bloco de código contida dentro dela seja executado. Uma alternativa para isso é o IIFE que ao mesmo tempo cria a função e já a executa na sequência.

```
  (function (){
    /* code */
  })();
```

Algumas formas de criar uma IIFE:

```
  (function(){ /* code */ }());
  (function(){ /* code */ })();
  new function(){ /* code */ }();
  var fn = function(){ /* code */ }();
```

### Variable Assignment

É a forma de criar uma função retornando um valor a uma variável, dessa forma a menos que essa função seja um IIFE ainda precisamos executar a função para que a variável receba o valor retornado dela.

```
  // Função anônima atribuída a uma variável
  var soma = function( num1, num2 ){
    return num1 + num2;
  };

  var operacao = function adicao( num1, num2 ){
    return num1 + num2;
  };

  soma( 5, 5 ); // 10
  adicao( 10, 10 ); // adicao is not defined
```

Legal, mas porque eu deveria aprender isso?

### Escopo
As variáveis têm como escopo a função onde elas foram criadas, e como não queremos poluir o escopo global as variáveis criadas dentro da nossa IIFE só existem dentro dela.

```
  (function(){
    var element = document.getElementById('element');
    console.log( element ); // <div id="element">...</div>
  })();

  console.log( element );  // element is not defined
```

### Conflitos de libs
Como muitas libs usam o `$` é comum haver conflitos, apesar de ter outras alternativas como o `noConflict()` na IIFE você pode passar a referencia ao jQuery, ou qualquer outra lib como parâmetro:

```
  (function( $j ){
    function facaAlgo() { /* code */ }
    $j('.element').click( facaAlgo );
  })( jQuery );
```

### Performance
Se você usa muitas vezes variáveis globais em seu código uma boa dica para performace é passar essas variáveis como parâmetro para que o interpretador não precise sair do escopo da sua função toda vez para ir buscar o valor daquela variável global.

```
  (function( doc ){
      var btn = doc.getElementById('btn')
        , element = doc.getElementById('element');

      btn.addEventListener('click', function(){
        element.style.display = 'none';
      }, false);
  })( document );
```

Com a junção desses dois conceitos, o `Variable Assignment` e o `IIFE` podemos criar algo como:

```
  var operacao = (function(){
    return {
      soma: function( num1, num2 ) {
        return num1 + num2;
      },

      subtracao: function( num1, num2 ) {
        return num1 - num2;
      }
    }
  })();
```

Nesse exemplo acima temos uma função auto executável anônima atribuída a variável conta, nessa IIFE retornamos um objeto com duas funções atribuídas as chaves do objeto. Como nós estamos retornando um objeto podemos acessar essas funções com `conta.soma( 5, 5 )` por exemplo.
É como se estivéssemos exportando essas funções que primeiramente estavam privadas dentro da IIFE para ser acessada através da variável `conta`. Sendo assim podemos escolher o que queremos que permaneça privado e o que queremos tornar acessível.
Para deixar mais organizado podemos criar as funções separadas e retornar o objeto apenas com o nome da função desejada.

```
  var operacao = (function(){

    // função privada da IIFE
    function soma( num1, num2 ) {
      return num1 + num2;
    }

    // função privada da IIFE
    function subtracao( num1, num2 ) {
      return num1 - num2;
    }

    return {
      soma, // retornando função soma a variável operacao
    }

  })();

  operacao.soma(5,5) // 10
  operacao.subtracao(5,5) // operacao.subtracao is not a function
```

### Quer saber mais sobre IIFE ?
- [IIFE - benalman ](http://benalman.com/news/2010/11/immediately-invoked-function-expression/)
- [Sobre funções imediatas JavaScript](http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/)
- [Every Possible Way to Define a Javascript Function](http://www.bryanbraun.com/2014/11/27/every-possible-way-to-define-a-javascript-function)
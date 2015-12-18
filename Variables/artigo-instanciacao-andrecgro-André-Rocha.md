# Entendendo a criação e instanciação de variáveis no Javascript
**Autor**: [André Rocha](https://github.com/andrecgro)

# Definição
  O conceito de variável é carregado para todas as linguagens de programação, funcionando basicamente como uma referência à um espaço de memória. De forma mais simples, trata-se de uma forma de armazenar e acessar valores em seu programa de forma fácil.
  Como o nome sugere, as variáveis podem alterar seus valores durante a execução do programa, diferente das chamadas ```constantes```, que vamos falar adiante.

### Tipos de Variáveis

  Em algumas linguagens de programação, chamadas de ```fortemente tipadas```, você é obrigado a declarar qual tipo de dado será armazenado por aquela variável para o programa ou [```compilador```](https://pt.wikipedia.org/wiki/Compilador) guardar o devido espaço na memória.
  No JavaScript, uma linguagem de `tipagem fraca`, declaramos nossas variáveis sem mencionar qual o tipo de dado que estamos tratando, isso nos permite maior flexibilidade de programação já que, não apenas o valor das variáveis mas também o seu tipo de dado pode ser alterado durante a execução do Programa.

  Temos 6 tipos de dados e variáveis em Javascript:

  * String - valor entre aspas simples ou duplas, geralmente frases, nomes e textos.(```'Este seria o exemplo de 1 string'```)
  * Numérico - qualquer tipo número.( números reais, inteiros, negativos, etc.)
  * Booleano - valor duplo representado por (verdadeiro/true) ou por (falso/false) - nome baseado na [Álgebra de Boole](https://pt.wikipedia.org/wiki/%C3%81lgebra_booliana)
  * Array - vetores ou conjunto de valores usando a seguinte declaração: ```var meuArray = {1,2,3,4,5}```
  * Objeto - coleção de propriedades(ou atributos)
  * Função - utilizada para dar nome à nossas funções(veremos adiante) com a seguinte sintaxe: ```var myFunction = function(){...}```


## Hoisting

  Hoisting(do inglês: içar, elevar ou guindar) é uma tarefa realizada Javascript que, na interpretação dos programas, envia todas as declarações de variáveis para o começo de seu escopo, garantindo assim que todas estas tenham sido declaradas antes de serem utilizadas (mesmo quando o programador não o tenha feito nessa ordem).

  Graças ao Hoisting podemos ter códigos no seguinte formato em Javascript:

  ```
    x=92;
    y=38;
    alert(x+y); //130
    var x;
    var y;
  ```
  Repare que a instanciação, ou atribuição, das variáveis ocorrem antes da soma, caso contrário teríamos como resposta ```undefined``` pois a operação Hoisting serve apenas para a declaração das variáveis e não para sua instanciação.

## Closures

### Escopo

  Antes de falarmos sobre Closures, precisamos entender o que é Escopo no Javascript.

  Pela definição de escopo podemos entender que trata-se dos locais de atuação de uma variável.
  Variáveis no Javascript podem ter dois tipos diferentes de escopo, local(são acessíveis apenas dentro da função em que foram declaradas) e global(podem ser acessadas de qualquer parte do arquivo do projeto).

  No exemplo a seguir, podemos entender melhor sobre escopo:

  ```

    // Exemplo de escopo local

    var myFunction = function(){
        var x = 10;//variável de escopo local, pode ser acessada apenas de dentro da função myFunction
        alert(x);
        }
    };

    // Exemplo de escopo global

    exemplo = ""; //variável de escopo global, falaremos mais sobre variáveis globais adiante

    var myFunction = function(){
        exemplo="teste";//atribuição da variável global
        alert(exemplo);
    }


  ```

### Closures

  Agora que já falamos sobre o escopo, podemos entender melhor o funcionamento dos Closures no Javascript.

  Closure, basicamente, é o aninhamento de funções no Javascript. Colocando uma função dentro de outra podemos obter algumas funcionalidades interessantes em nosso código.

  ```
    function funcaoPai() {
      var name = "Javascript";
      function funcaoAninhada() {
        alert(name);
      }
      funcaoAninhada();
    }
    funcaoPai();
  ```

  O mais interessante nos Closures são sua capacidade de acessar variáveis fora de seu escopo. As Closures podem acessar variáveis em três escopos diferentes, variáveis próprias(escopo local), variáveis globais(escopo global) e variáveis pertencentes ao escopo de sua "função pai".
  Além das variáveis criadas e instanciadas na função pai, as Closures também tem acesso aos parâmetros passados para esta.
  Por último, uma caracterísca bem interessante deste tipo de programação é que o acesso às variáveis da função pai são feitos pelas Closures mesmo após estes terem sido retornados anteriormente, é como se as variáveis guardasse seu escopo na hora de sua criação e os fornecesse quando acessadas, não importando se já foi retornada ou não.

  Para entender melhor este trecho acima temos o seguinte exemplo:

  ```
    function funcaoPai() {
      var name = "Javascript";
        function funcaoAninhada() {
          alert(name);
        }
      retorna funcaoAninhada();
    }

    var closures = funcaoPai();
    closures();
  ```

  Podemos observar acima - o mais interessante das closures - a capacidade de permitir que as funções interiores continuem acessando as variáveis da função exterior mesmo após a primeira já ter sido retornada, isto acontece pois, como dito acima, nas closures as variáveis armazenam seu escopo inicial.
  Além disso, as closures trabalham com referência de variáveis das funções exteriores, ou seja, o valor das variáveis na função filho se mantém atualizado com o valor das mesmas na função pai.

## Variáveis globais

  A declaração de variáveis globais pode ser de três formas:

   1. Basta não colocar a palavra chave ```var``` na hora de declarar a variável.<br/>
   2. Declarar a variável com a palavra chave ```var``` mas no começo do script e fora de qualquer escopo de função.<br/>
   3. Declarar a variável como uma propriedade do objeto global (que no browser é window) ```window.fill="";```<br/>

   Isto faz com que você deixe a variável suscetível à acessos por todo o script - código javascript - implicitamente.

   Obs.: Para um código mais legível, é convenção darmos preferência ao segundo tipo de declaração, pois possui basicamente o mesmo efeito e deixamos claro a intenção de criarmos uma variável.

  ```
    fill = "";//variável global

    function myFunction(){
      alert(fill);//acessando variável global
    }
  ```

  Entretanto, a utilização de variáveis globais é algo um pouco perigoso pois precisamos entender e controlar a atribuição de valores destas variáveis de forma correta e sistemática, para que não passemos a utilizar valores indesejados atribuídos por outras partes do script.

## Variáveis por parâmetro

  Variáveis por parâmetro são variáveis criadas na implementação de uma função e usadas em um escopo local.
  Utilizando parâmetros em nossas funções, estamos criando uma forma de tratar e trabalhar com os valores recebidos por essa função.

  ```
    function myFunction(parametro){
      alert(parametro); // exibe uma caixa de diálogo com o valor da variavel parametro;
    }

    myFunction("Javascript"); //utiliza myFunction passando a string "Javascript" como parâmetro.
  ```

  No exemplo acima, está exemplificado como trabalhamos com variáveis por parâmetros.
  De forma sucinta, o que estamos fazendo é "criar" uma variável chamada parâmetro e depois atribuir o valor "Javascript" à ela. É importante reforçar que o escopo destas variáveis por parâmetro é local, sendo acessadas somente dentro da função. Também podemos ressaltar que, no caso do exemplo acima, a variável por parâmetro é ```parametro``` e não ```"Javascript"```.

## Instanciação usando uma IIFE

  (IIFE) “Immediately-invoked function expression” - função imediata - é uma forma de executar a função imediatamente depois de criada.

  Sintaxe de uma IIFE:
  ```
   (function(){
    ...
   })();
  ```

 A ideia principal para a utilização do ```design pattern``` *IIFE* é o encapsulamento. Ao criarmos uma função anônima com execução imediata (IIFE), estamos criando um escopo temporário para nossas funções e variáveis e, ao mesmo tempo, isolando partes do código de alterações em variáveis que podem ser indesejadas.

 ```
  var foo = "faculdade"; //variável global

  (function(){
   var foo = "biblioteca"; //variável local
   alert("Eu estudo em uma " + foo); //alerta da variável local
  })();

  alert(foo); // alerta da variável global
 ```

  Executando este trecho de código você poderá ter uma noção mais clara do que estamos falando, observando que o primeiro ``` alert ``` irá imprimir a sentença com a variável local e seu valor sobreposto à variável global, diferente do segundo ```alert``` que imprimirá o valor da variável global por estar fora do escopo da IIFE em questão.


### Passando parâmetros para uma IIFE

   A passagem de parâmetros para uma IIFE reserva uma sintaxe um pouco diferente, mas também muito simples, como quase todo o Javascript.
   Primeiramente você deve referenciar os parâmetros que serão utilizados dentro dos parênteses da IIFE, como faria com qualquer outra função.

   ```
   (function(a,b){
    ...
   })();
   ```
  Esta referência está acontecendo em ```a,b``` no exemplo acima.

  Em seguida, basta adicionar os valores reais que serão utilizados nestes parâmetros referenciados, resultando no código final:

  ```
  var valorA = Javascript;
  var valorB = lindo;
   (function(a,b){
     alert(a+" é "+b);
   })(valorA,valorB);
  ```

## Referências

  1. [Compilador](https://pt.wikipedia.org/wiki/Compilador) - Acesso em 22 de Novembro de 2015;<br/>
  2. [Tipos de dados e variáveis em Javascript](https://www.todoespacoonline.com/w/2014/04/variaveis-em-javascript/) - Por Luiz Otávio - Acesso em 22 de Novembro de 2015;<br/>
  3. [Álgebra Booliana](https://pt.wikipedia.org/wiki/%C3%81lgebra_booliana) - Acesso em 22 de Novembro de 2015;<br/>
  4. [Javascript Hoisting](http://www.w3schools.com/js/js_hoisting.asp) - Acesso em 22 de Novembro de 2015;<br/>
  5. [Entenda Closures no Javascript com facilidade](http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/) - por Eric Oliveira - Acesso em 22 de Novembro de 2015;<br/>
  6. [Closures](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures) - Acesso em 22 de Novembro de 2015;<br/>
  7. [Escopo variável](https://msdn.microsoft.com/pt-br/library/bzt2dkta(v=vs.94).aspx) - Acesso em 22 de Novembro de 2015;<br/>
  8. [Sobre funções imediatas JavaScript(IIFE)](http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/) - por Pedro Araujo - Acesso em 22 de Novembro de 2015;<br />
  9. [Immediately-invoked function expression](https://en.wikipedia.org/wiki/Immediately-invoked_function_expression) - Acesso em 22 de Novembro de 2015;<br/>
  10. [Variáveis e Funções no JavaScript: criação e instanciação](https://github.com/Webschool-io/be-mean-instagram-artigos/blob/master/Variables/artigo-instanciacao-ajanini-Alexandre-Reiff-Janini.md) - Por Alexandre Reiff Janini - Acesso em 22 de Novembro de 2015;<br/>
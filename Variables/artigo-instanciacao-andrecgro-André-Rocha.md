# Variáveis e Funções no JavaScript: criação e instanciação
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
    alert(x+y);
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

    exemplo = "";//variável de escopo global, falaremos mais sobre variáveis globais adiante

    var myFunction = function(){
        exemplo="teste";//atribuição da variável global
        alert(exemplo);
    }


  ```

## Variáveis globais
## Variáveis por parâmetro
## Instanciação usando uma IIFE
## Considerações

## Referências
[Compilador](https://pt.wikipedia.org/wiki/Compilador) - Acesso em 22 de Novembro de 2015;
[Tipos de dados e variáveis em Javascript](https://www.todoespacoonline.com/w/2014/04/variaveis-em-javascript/) - Por Luiz Otávio - Acesso em 22 de Novembro de 2015;
[Álgebra Booliana](https://pt.wikipedia.org/wiki/%C3%81lgebra_booliana) - Acesso em 22 de Novembro de 2015;
[Javascript Hoisting](http://www.w3schools.com/js/js_hoisting.asp) - Acesso em 22 de Novembro de 2015;
[Entenda Closures no Javascript com facilidade](http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/) - por Eric Oliveira - Acessado em 22 de Novembro de 2015;
[Closures](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures) - Acessado em 22 de Novembro de 2015;
[Escopo variável](https://msdn.microsoft.com/pt-br/library/bzt2dkta(v=vs.94).aspx) - Acessado em 22 de Novembro de 2015;
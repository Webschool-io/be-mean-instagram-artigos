#Instanciação de variáveis em JS

A instanciação de variáveis em **JS** é algo comum, porém sofre influência ou influncia em vários aspectos da linguagem, por exemplo:

A instanciação de uma variável comum no Javascript acontece da seguinte forma:

```js
    var a; //undefined
    var b = 5; //5
```

No exemplo acima, ocorreu uma instanciação simples utilizando a palavra reservada **var**, porém não foi especificado o escopo em que ela se encontra, entre outras coisas, veremos agora alguns comportamentos característicos do JS:

-----------------------------------------

#Hoisting

O Hoisting, ou "içamento", acontece em tempo de execução, e o que ele faz é içar todas as declarações tanto de variáveis quanto funções para o começo do arquivo para que sejam executadas com prioridade, exemplo:

```js
    console.log(a);
    var a = 5;
```

A primeira vista o que pensamos é que o console.log() irá gerar um erro porque a variável **a** ainda não foi definida, porém é aí que o efeito de içamento(Hoisting) acontece, ele iça a declaração da variável **a** para o topo, ficando assim:

```js
    var a;
    console.log(a); //undefined
    a = 5; //a atribuição não sofre o içamento
```

Como visto acima, somente a declaração da variável é içada, enquanto a atribuição da mesma permanece no mesmo lugar.


####Com funções
O efeito de hoisting com funções pode causar ações indesejadas, por exemplo:

```js
    //hoisting em function declarations
    soma(); //2 com efeito de hoisting
    
    function soma() {
      return 1 + 1;
    }
```

No exemplo acima, o efeito de içamento ocorrerá sem nenhum imprevisto, ou seja, a declaração da função será içada acima da chamada da mesma e seu retorno será 2.


Porém o mesmo não acontece quando se usa **function expressions**, ou seja, quando se passa uma função anônima para uma variável, veja:

```js
   //hoisting em function expressions
   soma();

   var soma = function () {
     return 1 + 1;      
   };

```

No exemplo acima, acontecerá algo inesperado, a declaração da **variável soma** será içada para o topo, porém a atribuição da função ocorrerá após a chamada da mesma, ou seja, neste ponto a função **soma()** ainda não existe, o que existe é a **variável soma**, o código em tempo de execução ficará da seguinte forma:

```js
   //hoisting em function expressions
   var soma;
   soma(); //gera typeError

   soma = function () {
     return 1 + 1;      
   };

```



--------------------------------------------

#Closure

Antes de falar sobre closure, vale primeiro explicar que, em JS em cada função é criado um **novo escopo** para as variáveis definidas dentro dela, exemplo:

```js

    var a = function () {
        var a = 5;
        console.log(a);
        
        return function() {
          var a = 6;  
          console.log(a);   
        }
    };

    a()(); // 5, 6

```

Como ilustrado acima, as variáveis **a** são diferentes dentro de cada função, porém na função mais interna, se **a** fosse definida sem a palavra chave **var**, o interpretador do JS buscaria a definição de **a** no escopo mais externo caso não o encontrasse no escopo local, e por fim caso ainda não encontrasse ele buscaria no escopo GLOBAL.

####Como funciona a Closure

O mecanismo de Closure permite acesso a uma veriável de escopo local externamente à função. Em JS assim que uma função é executada, suas variáveis locais são destruídas, a Closure permite manter uma referência a essas variáveis mesmo após a execução da função, exemplo:

```js
    
    var a = function () {
        var teste = "chulapa";

        return function () {
            return teste;
        }
    };

    console.log(a()()); //chulapa
```

Como pode ser visto acima, conseguimos acesso externo ao valor da variável local **teste** através da função mais interna, a qual retorna a variável em questão, este é o mecanismo de Closure.
A Closure acontece porque uma variável retornada de função mantém a referência do escopo da qual ela foi criada.
Um ótimo uso para as Closures é simular o encapsulamento de propriedades, utilizando as propriedades(variáveis e funções) locais como privadas e retornando as que seriam públicas. 


----------------------------------

#Variável Global

Em JS **qualquer variável que não seja definida dentro de uma função, será encarada como variável GLOBAL**, e isso pode acarretar vários problemas como:

1. Incompatibilidade com outras bibliotecas por colisões de nomes

1. Problemas de performance

1. Fragilidade de Código


####Global Implícita

Por vezes devido a algum erro de declaração, uma variável que deveria ser de escopo local acaba sendo encarada implícitamente como GLOBAL, exemplo:

```js
    function () {        
      var chubaka = "BUAAAA"; //erro, aqui era somente vírgula
          chibanca = "PAAAA"; //variável encarada como global 
    }
```

No exemplo acima a variável **chibanca** será encarada como Global implicitamente, pois não possui em sua definiçãoa palavra reservada **var**.

**OBS:**
>Sempre que uma variável é referenciada, o interpretador buscará em qual escopo ela foi definida, caso não encontre no escopo atual, ele vai subindo a hierarquia até chegar ao escopo GLOBAL.

------------------------------------------

#Variável por Parâmetro

Quando uma variável é passada por parâmetro, ela fica disponível dentro da função pelo nome que foi definido na lista de argumentos, através deste nome é possível utilizar os valores desta variável que foi passada por parâmetro, exemplo:

```js
        
    var top = 5;
    function exibeQuadrado(a) {
        console.log(a * a);
    }

    exibeQuadrado(top);//passa top como parâmetro e exibe 25
```

Como visto acima, a variável **top** foi passada como parâmetro para a função **exibeQuadrado()** que internamente a referencia como **a**.

Quando uma variável GLOBAL é passada por parâmetro, o comportamento da função é o mesmo, ela criar uma referência interna para a variável, e assim sendo nada é alterado no valor da variável GLOBAL.

------------------------------------------- 

#Instanciação usando uma IIFE

####IIFE(Immediately Invoked Function Expression)
Como o próprio nome diz, uma IIFE é uma função que é auto-executada no momento em que é lida, então caso uma variável receba um valor de uma IIFE, esta receberá somente o retorno da função e não a definição da mesma, exemplo:

```js
    var vraw = (function () {
            var vraw = "vrawwwwwww";
            return vraw;
        })();
    console.log(vraw);//vrawwwwwww
```

Como podemos verificar na sintaxe **(function(){})()**, a função chama a execução de si mesma.

As IIFE's permitem criar códigos modulares, ou seja, ela encapsula o código de forma a não permitir intervenção externa ao seu funcionamento, porém é possível passar parâmetros pra ela assim como qualquer outra função e tratar tais parâmetros com outros nomes internamente, evitando conflitos com outras bibliotecas, um exemplo disso é o JQuery, que tem seu objeto JQuery, porém internamente o trata como $, exemplo:

```js
    (function($) {
      ...
    })(Jquery);
```

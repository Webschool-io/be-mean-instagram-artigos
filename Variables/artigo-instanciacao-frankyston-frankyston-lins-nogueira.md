# Artigo
**autor**: Frankyston Lins
**Tema**: Instanciação

Este artigo explica com teoria e código como o JavaScript cria e instancia as variáveis.

## Hoisting
Hoisted é apenas a declaração de uma função ou variável mas não inicializada.

**VARIÁVEL**
	
    ```
    // Irá imprimir o erro dentro do `catch` 
    try {
      console.log(a)
    } catch (e) {
      console.error('A variável `a` não foi definida.')
    }
    
    // Irá imprimir `undefined`
    try {
      console.log(a)
      var a = 2
    } catch (e) {
      console.error('A variável `a` não foi definida.')
    }
    ``` 
    
**FUNÇÕES**
	O hosting com funções acontece de maneira diferente. Aqui, não só o nome da função é hoisted como também seu corpo.
    
    ``` 
    foo()
    function foo() {
      console.log('bar')
    }
    ```

**OBSERVAÇÃO**
Uma função pode ser declarada também como uma expressão, e quando declarada desta forma, ela obedece a regra de hoisting de variável. Apenas seu nome será hoisted.
    
    ```
    foo() //=> TypeError
    var foo = function() {}
    ``` 

**Fonte: http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/**

## Closure

**CONCEITO:**
Um closure é uma função interior que tem acesso a variáveis de uma função exterior – cadeia de escopo. O closure tem três cadeias de escopo: ele tem acesso ao seu próprio escopo (variáveis definidas entre suas chaves), ele tem acesso as variáveis da função exterior e tem acesso as variáveis globais.

    ```
    var nameIntro = "Your name is ";
    //esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
    function makeFullName () {
        return nameIntro + firstName + " " + lastName;
    }
        return makeFullName ();
    }
    
    showName ("Michael", "Jackson"); //Your name is Michael Jackson
    ```

### Regras do Closure

**1. Closures tem acesso a variável das funções exteriores mesmo após o retorno da função exterior**

    ```
    function celebrityName (firstName) {
    var nameIntro = "This celebrity is ";
 
    //essa função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
    function lastName (theLastName) {
        return nameIntro + firstName + " " + theLastName;
    }
    return lastName;
    }
     
    var mjName = celebrityName ("Michael"); 
    //Nesta conjuntura, a função exterior celebrityName foi retornada
     
    //O closure (lastName) é chamado aqui depois da função exterior ter retornado acima
    //Sim, o closure continua tendo acesso as variáveis da função exterior e parâmetros
    mjName ("Jackson"); //This celebrity is Michael Jackson
    ```

**2. Closures armazenam referências para as variáveis da função exterior**
    
    ```
    function celebrityID () {
        var celebrityID = 999;
        //Nós retornamos um objeto com algumas funções interiores
        //Todas as funções interiores têm acesso as variáveis'da função exterior
        return {
            getID: function () {
                //Esta função interior vai retornar a variável celebrityID ATUALIZADA
                return celebrityID;
            },
            setID: function (theNewId) {
                //Esta função interior vai mudar a variável da função exterior a qualquer hora
                celebrityID = theNewId;
            }
        }
    }
    var mjID = celebrityID (); //Nesta atual conjuntura, a função exterior celebrityID já retornou
    mjID.getID(); //999
    mjID.setID(567); //Muda a variável da função exterior
    mjID.getID(); //567 - retorna o valor atualizado da variável celebrityID
    ```

**Fonte: http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/**

## Variável Global

Uma variável global é uma que é visivel em qualquer escopo. Elas podem ser convenientes em programas pequenos, mas rapidamente se tornam difíceis de lidar conforme os softwares crescem. Porque uma dessas variáveis pode ser alterada em qualquer parte do código e em qualquer momento, isso pode complicar muito o comportamento de um programa. O uso dessa variáveis degrada a confiabilidade dos softwares em que são usadas.

### Modos de usar variáveis globais

1. Colocar um comando **var** fora de qualquer função:

    ```
    var foo = value;
    ```
2. Adicionar uma propriedade diretamente no objeto global. Como por exemplo nos navegadores que tem o objeto global window:

    ```
    window.foo = value;
    ```
3. Utilizando uma variável sem declaração (variável global implícita): 

    ```
    foo = value;
    ```

### Variável global dentro de função
    ```
    foo = value;
    // Irá imprimir `2`
    try {
      var a = 2
      console.log(a)
    } catch (e) {
      console.error('A variável `a` não foi definida.')
    }
    ```

Para variável dentro de função é preciso ser instaciado e iniciado com um valor juntamente o comando var.

## Variável por parâmetro

### Quando um parâmentro é passado.
Os parâmetros são conhecido como variáveis da função. Diferentemente de variáveis comuns, em vez de ser inicializado para **undefined**, eles são inicializados para os argumentos fornecidos quando a função é invocada. Os argumentos de uma função não estão limitados para strings e número. Você pode passar objetos para uma função. 

PS.: Um parâmentro extra que está disponível para funções quando elas são invocadas é a matriz **arguments**. ELa dá acesso à função a todos os argumentos que foram fornecidos com a invocação.

### Quando uma GLOBAL é passada por parâmetro.
As variáveis definida no interior de uma função não podem ser acessadas de qualquer lugar fora da função, porque a variável está definida apenas no escopo da função. No entanto, uma função pode acesar todas variáveis e funções definida fora do escopo onde ela está definida. Em outras palavras, a função definida no escopo global pode acessar todas as variáveis definidas no escopo global. A função definida dentro de outra função também pode acessar todas as variáveis definidas na função hospedeira e outras variáveis ao qual a função hospedeira tem acesso.


## Instanciação usando uma IIFE

Em 2010, um cara chamado Ben Alman, publicou um artigo chamado “Immediately-Invoked Function Expression (IIFE)”, no artigo ele conta do porque não concordava com o termo “self-executing anonymous function”, que significava isso aqui:

    ```
    (function () {
      console.log('Olá')
    }())
    ```

Esta função é executada no mesmo momento em que é definida.

### Pra que IIFE são úteis?
Há vários locais que podemos usar uma IIFE, porém o mais comum é quando queremos criar um escopo no JavaScript para que as variáveis definidas dentro da função não poluam o escopo global (variáveis definidas no window).

Isso é tudo a respeito de evitar ao máximo criar variáveis globais no JavaScript, e criar escopos quando e onde precisarmos.

    ```
    (function () {
      'use strict'
     
      var sayHi = 'oi'
      console.log(sayHi) // oi
    }())
     
    console.log(sayHi) // ReferenceError: sayHi is not defined
    ```

Assim mantemos as variáveis todas dentro do nosso código, evitando que o JavaScript de outras pessoas alterem nossas variáveis. Como libs de terceiros ou curiosos com o DevTools aberto.

### Passando valor em uma função IIFE
A passagem do parâmetro é no segundo parênteses, onde invocamos a function, podemos passar qualquer parâmetro como se fosse qualquer outra função.

    ```
    (function (string) {
      console.log(string)
    }('oi'))
     
    // Log 'oi'
    ```

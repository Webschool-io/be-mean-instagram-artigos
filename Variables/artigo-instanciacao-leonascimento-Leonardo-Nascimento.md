# Variáveis e Funções no JavaScript: criação e instanciação

**Autor**: [Leonardo Nascimento](https://github.com/leonascimento)

# Declaração de variáveis na JavaScript

# Hoisting 

![](https://cdn.pbrd.co/images/2nqDT0Bv.jpg)

> Se você acha que conhece bem a linguagem JavaScript e não sabe o que é **(Elevação ou Hoisting)**
você pode até saber algumas coisas sobre a linguagem, mas está deixando de lado um dos principais assuntos que faz parte do background que a JavaScript possui.

**Hoisting** é um assunto que pode em alguns casos confundir boa partes dos estudantes da JavaScript.

### Significado

**Hoisting** em inglês significa (içar, levantar, hastear).

### Conceito 

**Hoisting** é um comportamento padrão da JavaScript que eleva todas as **declarações** ao topo do escopo atual, ou seja , as declarações são elevadas ao topo do escopo onde estão sendo declaradas. Se essa declaração está dentro de uma função será elevado ao topo do escopo dessa função, se estiver fora de uma função ela será elevada ao topo do escopo global.

### Hoisting com Variáveis

**Exemplo 01:**
```JS
console.log(someVar); // undefined 

var someVar = "someValue";
```
1. `console.log(someVar); // undefined` - Analisando esse simples exemplo porque o valor é `undefined`? Pelo fato da engine do JavaScript não elevar atribuição ou inicialização de variáveis. 

2. `var someVar = "someValue";` - Podemos ver que foi feito uma atribuição então o JS leva em consideração apenas a declaração e ver internamente da seguinte forma:

```JS
var someVar;           // Declaração (içada, elevada)
console.log(someVar);  // undefined
someVar = "someValue"; // Atribuição
```
1. Primeiro temos a declaração sendo içada.
2. Vemos a tentativa de imprimir uma variável que não tem nenhum valor atribuído antes da tentativa de a imprimir.
3. Note que a atribuição não foi içada ao topo do escopo e apenas a declaração `var someVar;`  foi elevada. Por isso a causa do undefined.

### Como evitar o Hoisting

* A melhor forma de evitar alguns bugs é sempre **declarar/definir** as variáveis no topo de cada escopo,

It's Good
```JS
// Declarando
(function(){
   // Declarando ao topo
   var product, price, discount;
  
  // Atribuindo valores
  product = "Paper";
  price = "25.00";
  discount = 0.2;
})();

```

* Podemos também além de declará-las inicializar todas no topo de cada escopo.

```JS
function someFunc() {
  var firstName = "",
      lastName = "",
      price = 0,
      discount = 0,
      fullPrice = 0,
      myArray = [],
      myObject = {};
 
   //scripts ...  
};
```

> Por final podemos usar um _linter_ ou _hinter_ em nosso código e nunca esquecer que **mesmo se você declarar variáveis ou expressões de funções após a execução lembre-se que a atribuição sempre deve ser estar antes da chamada dessa variável ou da expressão de função** isso evitará o valor undefined quando a acontecer o hoisting.

### Hoisting com Funções

**Exemplo 01:** 

```JS
someFunction(); // "Some value";

function someFunction() {
    return "Some value";
}
```
1.  Simplesmente estamos invocando uma função `someFunction()` antes de declará-la.

2.  Na hora que o JS interpretar códigos, ele pega todas as definições de funções e variáveis que foram definidas e as joga para o topo escopo onde elas se encontram.

3. O JS internamente ler isso dessa forma

```JS

// Declaração levantada ao topo
function someFunction() {
    return "Some value";
}

someFunction(); // "Some value";
```

**Exemplo 02:**

> Na linguagem JavaScript podemos definir uma **FUNÇÃO DECLARATIVA** ou podemos criar uma **EXPRESSÃO DE FUNÇÃO**. 

**FUNÇÃO DECLARATIVA: É a forma que vimos no exemplo anterior.** 
```JS
function nameFunction(){
   // scripts
}
```

**EXPRESSÃO DE FUNÇÃO:  Geralmente a função não é nomeada e sempre é atribuída a uma variável como no exemplo abaixo.** 

```JS
var bar = function( a,b ) {
  return a*b;
};
bar(2,2); // 4
```

Devemos ter cuidado com a _Expressão de Função_ pois se invocarmos antes da expressão acontece o hasteamento da declaração.

```JS
foo(2,2); // undefined

var foo = function( a,b ) {
  return a*b;
};
```

Desta forma a engine do JavaScript levanta a declaração da expressão de função ao topo do escopo.

```JS
var foo;   // Declaração levantada ao topo 
 
foo(2,2);  // undefined

foo = function( a,b ) {
  return a*b;
};
```
1.  A declaração é içada
2.  `foo()` o momento que a função que não existe é executada, será disparado um erro do tipo TypeError, nos avisando que undefined não pode ser usado como uma função.
3. Expressão de função que não é executada pelo fato do erro ter acontecido antes

### Dicas
**Dica: 01**

> Saiba que uma se uma **Declaração de Função** possuir o mesmo nome de uma declaração de variável ela tem precedência sobre uma **Declaração de variável** quando hasteadas.

```JS
var someName;

function someName () {
    console.log("Leo");
}

console.log (typeof someName); //function
```
1. `var someName;` - Declaramos uma variável
2. Declaramos uma função nomeada com o mesmo nome da variável
3. Se verificarmos o `typeof` vamos constatar que `someName` é do tipo function

**Dica: 02**
> Neste outro caso se a **Inicialização** de uma variável possuir o mesmo nome de uma função ela sobrescreve a **Declaração de Função**

```JS
var someName = "Oliver"; 

function someName () {
  console.log("Rich");
}

console.log(typeof someName); //string
```
1. Inicializamos uma variável
2. Declaramos uma função com mesmo nome da variável
3. Testamos o `type` de `someName` não sobrescreveu a variável que foi inicializada no topo do escopo

**Dica: 03** 

Se você usa [(ECMAScript 2015) ES6](https://leanpub.com/understandinges6/read) você pode fazer o uso do `let`, pois tudo que é declarado com ele não é hasteado antes da execução do código.

---------

# Closures

![](https://cdn.pbrd.co/images/2oTFsNr8.jpg)

### Significado 
Em inglês **Closure** significa (encerramento, fechamento, oclusão)

### Conceito
Esse conceito de **Closures** foi inicialmente implementado numa linguagem de programação de paradigma funcional chamada Scheme.

> Em ciência da computação e na programação um **Closure** é uma função que referencia variáveis livres no contexto léxico. Um **Closure** geralmente ocorre quando uma função é declarada dentro do corpo de outra função. A função interior pode referenciar variáveis locais pertencentes ao escopo da função exterior. 

Uma **Closure** surge quando a função onde ela encontra-se é executada. Podemos ver que ela depende de uma função para existir, pois no momento que a função exterior é **executada** e o **Closure** é criado passa a ter referências a quaisquer variáveis no escopo da função que a contém.

Entenda que os **Closures** possuem apenas referências das variáveis do escopo da função exterior. Assim que elas não podem acessar o objeto **_arguments_** da função exterior.

As **Closures** possuem acesso a 3 cadeias de escopo: 
* **Primeiro escopo**: Acessa todas as variáveis no seu escopo
* **Segundo escopo**: Acessa todas as variáveis da função exterior
* **Terceiro escopo**: Acessa todas a variáveis do escopo **Global**

![](http://codechutney.in/blog/wp-content/uploads/2014/09/closure.png)

### Como usar

Exemplo Trivial

```JS
// 1 Função exterior
function welcomeMsg(firstName, lastName) {
    var msgIntro = "You're welcome ";
    // 2 closure que vai ser criada
    function makeMsg () {
        return msgIntro + firstName + "  " + lastName;
    }
   // 3 executando o closure e retornando o resultado da sua execução
   return makeMsg(); 
}

welcomeMsg ("Leonardo", "Nascimento");  // "You're welcome Leonardo  Nascimento"
```
1. `welcomeMsg` é a função exterior que recebe dois parâmetros
2. Função interior que acessar dois parmetros e uma variável da função exterior
3. Retornamos a chamada da **Closure**

## Contextos de Uso

### **Closures** para criar Funções **Factories:**

```JS
function geraDescricao(title) {
    return function(nivel) {
        return title + ' ' + nivel;
    };
}

var vendedor= geraDescricao('Vendedor');
var gerente = geraDescricao('Gerente');

alert(vendedor('Trainee'));
alert(gerente('Executivo Regional de Contas')); 
alert(gerente('Nacional de Produção'));  
``` 

Sempre que precisarmos criar funções similares mas com valores únicos precisamos de Factories. Assim vamos manter o nosso JavaScript DRY. Perceba que com apenas 5 linhas de códigos podemos criar N funções com valores únicos.

###  Quando precisamos **simular variáveis ou métodos privados**.

Muitas linguagens fornecem a capacidade de declaramos variáveis e métodos privados, mas na JavaScrip podemos
contornar isso simulando essa funcionalidade utilizando **Closures**. Podemos emular utilizando um padrão conhecido como Module Pattern.

```JS
var calculeCart = (function() {
  var total = 0;
  function calcule(price) {
    total += price;
  }
  return {
    addPreco: function(value) {
      calcule(value);
    },
    removePreco: function(value) {
      calcule(-value);
    },
    precoTotal: function() {
      return total;
    }
  }; 
})();
calculeCart.addPreco(20); 
calculeCart.addPreco(10);
alert(calculeCart.precoTotal());  // 30
calculeCart.addPreco(10); 
calculeCart.removePreco(10);
calculeCart.removePreco(20);
alert(calculeCart.precoTotal()); // 10

console.log(calculeCart.calcule(30)); //Uncaught TypeError: calculeCart.calcule is not a function
```
1. Temos um module pattern tudo que é criado dentro da IIFE apenas pode ser acessado apenas internamente.
2. Criamos a variável `total` e a função `calcule` onde ambas não são acessadas diretamente
3. Definimos quais funções devem ser publicas, então retornamos `addPreco`,  `removePreco`, `precoTotal`; Desta forma não podemos acessar `total` e `calcule` diretamente. Agora elas estão privadas
4. Adicionamos alguns preços
5. imprimimos o total de 30
6. removemos alguns produtos
7. tentamos acessar `calculeCart.calcule(30)` passando o valor de 30 mas veja que um erro é lançado pois ela não está acessível publicamente.

Usando **Closures**  para simular variáveis e métodos privados mantemos os namespaces mais limpos, prevenindo conflitos no escopo global. 

### Usando Closures para Criar uma função proxy/bind

Um problema comum que acontece em alguns momentos é quando perdemos a referência de um objeto. Para resolver esse problema de escopo precisamos criar uma função proxy que recebe o nome de uma função e um contexto, ela retorna uma nova função com o contexto que foi recebido e deve ser aplicado. Então usamos um **Closure** para vincular uma referência de escopo externo ao `this` dentro do escopo interno recém criado.

```JS
var Car = function() {

  this.start = function() {
    return console.log("car started");
  };

  this.turnKey = function() {
    // Access element
    var carKey;
    carKey = document.getElementById("carKey");

    // Use proxy
    carKey.onclick = proxy(function(event) {
      this.start();
    }, this);
  };

  return this;
};

// Utilizando um Closure para gerar o bind de uma referência de um escopo dentro de um escopo recém-criando.
var proxy = function(callback, self) {
  return function() {
    return callback.apply(self, arguments);
  };
};

var tesla = new Car();

// Apenas um click de uma usuário no elemento #carKey  e veremos "car started"
tesla.turnKey();
```
1. Nesse contexto criamos uma classe Car
2. Definimos o método `start` que faz um simples log 
3. Definimos o método `turnKey` que acessa um elemento com um ID carKey
4. Em seguida dentro de `turnKey` anexamos um evento de click, agora perceba que fizemos a chamada da função proxy e passamos o callback como parâmetro, e logo após passamos o `this` que contém o escopo que queremos aplicar internamente no callback.

### Closures para Estender a Linguagem

Neste ponto isto é fácil de ver que **Closures** são essenciais para escrever um JavaScript de alta qualidade. Vamos aplicar o que nós sabemos sobre **Closures** para aumentar tipos nativo. Com nosso foco em objetos de função, vamos aumentar um tipo de função nativa.

```JS
Function.prototype.cached = function() {
    var self = this, //"this" referencia a função original
        cache = {};  //nosso local, escopo lexico de armazenamento em cache
    return function(args) {
        if(args in cache) return cache[args];
        return cache[args] = self(args);
    };
};
```

Esse pequeno trecho de código nos permite quando criarmos uma pequena função, será criado outra função idêntica que fica em cache.

```JS
Math.sin = Math.sin.cached();
Math.sin(1) // => 0.8414709848078965
Math.sin(1) // => 0.8414709848078965 chamando a partir do cache
```

Temos uma variável local `cache` que fica privado escopo exterior. Isso deve evitar qualquer adulteração que possa invalidar o nosso cache. O Closure que está retornando tem acesso ao binding da função exterior, isso significa que somos capazes de retornar uma função com acesso completo para o cache no interior bem como a função original. Isso pode ser muito bom por questão de desempenho. Esse simples exemplo gerencia apenas um argumento, mas seria melhor em uma função Factory com argumentos de cache múltiplos. 

## Alguns Comportamentos de Closures

### Erro em Loops

Um problema bastante clássico é quando usamos Closures em internamente em um  loop. Isso acontece pelo fato dos closures possuírem apenas acesso ao valor atualizado das variáveis das funções exteriores, eles podem também apresentar erros quando a variável da função exterior muda com um loop for.

Exemplo do erro:

```JS

function celebrityIDCreator (theCelebrities) {
    var i;
    var uniqueID = 100;
    for (i = 0; i < theCelebrities.length; i += 1) {
        theCelebrities[i]["id"] = function () {
            return uniqueID + i;
        }
    }

    return theCelebrities;
}

var actionCelebs = [{name:"Stallone", id:0}, {name:"Cruise", id:0}, {name:"Willis", id:0}];

var createIdForActionCelebs = celebrityIDCreator(actionCelebs);

var stalloneID = createIdForActionCelebs[0];

console.log(stalloneID.id()); //103
```

1. Note que o objetivo seria gerar um ID para cada objeto celebridade.

2. Quando a função anônima é executada o valor de `i` é 3. O número 3 foi adiciona ao `uniqueID` para gera 103 para todos os celbrityID. Então cada posição no array retornado objteve ID = 103, ao invés de 100, 101, e 102. 

3. Isso **acontece pelo fato do closure (função anônima) ter acesso direto a variável da função por referência e não por valor**.

4. Nesse exemplo acessamos a variável `i` quando ela foi alterada, depois que a função roda o loop `for` inteiro e retorna o último  103.

**SOLUÇÃO 01: Podemos usar Closure para criar um ESCOPO LOCAL NA MEMORIA**

Podemos usar uma **Expressão de Função Imediatamente Invocada** (IIFE).

```JS
function celebrityIDCreator (theCelebrities) {
    var i;
    var uniqueID = 100;
    for (i = 0; i < theCelebrities.length; i += 1) {
           // 1. IIFE              
           theCelebrities[i]["id"] = function (j) { 
               return function () {
                // 2.
                return uniqueID + j; 
            } () // 4
        } (i); //3. 
    }
    return theCelebrities;
}

var actionCelebs = [{name: "Stallone", id:0}, {name: "Cruise", id:0}, {name: "Willis", id:0}];

var createIdForActionCelebs = celebrityIDCreator (actionCelebs);

var stalloneID = createIdForActionCelebs [0];
console.log(stalloneID.id); //100

var cruiseID = createIdForActionCelebs [1];
console.log(cruiseID.id); //101
```

1. IIFE a variável paramétrica j é o i passado na invocação da IIFE

2. Cada iteração do loop `for` passa no valor atual de `i` dentro desta IIFE e salva o valor correto no array

3. Invocando imediatamente a função passando a variável i como um parâmetro

4. Adicionando `()` no fim da função, nós estamos executando-a imediatamente e retornando somente o valor de uniqueID + j, ao invés de retornar uma função

**Solução: 02 Se você utilizar [ES6 - ECMAScript 6](https://leanpub.com/understandinges6/read) `let`**

`let` - é uma palavra reservada no lugar de `var` para criar um novo de escopo para a variável, se ela estiver em um `if`, loop `for`, `while` a palavra `let` aproveita os `{ }` delimitadores onde essa variável está contida e cria um escopo naquele bloco. Basta substituir var por `let` e tudo funciona magicamente.

```JS
for(var i = 0; i < 3; i++) {
    let j = i;
    setTimeout(function() {
        console.log(j);
    });
}
```

### Closures tem acesso a variável das funções exteriores mesmo que retornemos a função exterior.

Mesmo que a função exterior seja retorna os closures ainda guardam a referência da variável da função exterior mesmo ela sendo retornada. Então você pode chamar a função interior depois em seu programa.

```JS
function celebrityName (firstName) {
    var nameIntro = "This celebrity is ";

    // Essa função interior tem acesso as variáveis da função exterior além dos parâmetros
    function lastName (theLastName) {
        return nameIntro + firstName + " " + theLastName;
    }
    return lastName;
}

var barryName = celebrityName ("Barry"); 
//Neste momento a função exterior celebrityName foi retornada

//O closure (lastName) é chamado aqui depois da função exterior ter retornado acima
//Sim, o closure continua tendo acesso as variáveis da função exterior e parâmetros
barryName("Allen"); //This celebrity is Barry Allen
```

-----

# Variáveis por Parâmetro

![](https://mgoodme.files.wordpress.com/2014/09/school-bus.jpg)

Os parâmetros de uma função comportam-se como variáveis regulares. O valor inicial da variável é dado
por quem invocou a função e não pelo código da função em si. Variáveis definidas dentro do escopo da função
incluindo seus parâmetros passam a ser locais à própria função, isto é, no exemplo abaixo temos a variável `result` dentro da nossa função chamada de `myFunction`, então a nossa variável `result` será criada novamente 
toda vez que a função for invocada.

Exemplo:

```JS
var myFunction = function(base, exponent) {
  var result = 1;
  for (var count = 0; count < exponent; count++)
    result *= base;
  return result;
};

console.log(myFunction(3, 9)); //19683
```

Esses tipos de variáveis apresentadas acima só **funciona** com os parâmetros e às variáveis que forem declaradas utilizando a palavra-chave: `var` dentro do escopo da função.

As variáveis declaradas globalmente não são locais à própria função como vimos no exemplo acima
como sua declaração é global, então **podemos acessar variáveis globais dentro de qualquer função**,
a menos que você tenha declarado uma variável local que tenha o mesmo nome.

-------

# Variável Global

![](https://cdn.pbrd.co/images/2rVcBQ5y.jpg)

De forma bem simples podemos dizer que: variáveis globais são variáveis declaradas fora de uma função, então
significa que elas estão no escopo global(é o objeto window, ou o documento HTML inteiro), então toda vez que
definimos uma variável global ela será acessível em qualquer lugar da nossa aplicação, 
então podemos sobrescrever essa variável, o que não seria um a boa ideia.Exemplo:

```JS
var res = 5; // variável global
função myFunction ()
   alert (res); // 5
}

console.log(window.myName); //leo;
console.log("myName" in window); //true
```
OBS: Todas as variáveis globais são anexadas no objeto window:
 
### Como elas tornam-se automaticamente globais ?

Quando atribuímos um valor a uma variável sem declarar  o "var" antes , mesmo que ela esteja dentro do escopo 
de uma função ela será uma  variável global.

```JS
function escopo(){
  localVar = true;
}

escopo();

console.log(localVar); // true
```

Quando o código está sendo compilado será lido todas as atribuições das variáveis
caso você não declare o var , então a variável não será
atribuida somente a esse escopo, mas sim ao escopo global da aplicação.

### variáveis setTimeout: 

As funções `setTimeout` são executadas no escopo global, exemplo:

Atenção : O objeto `this` dentro da função `setTimeout` se refere ao objeto`Window`, e não ao `myObj`.

```JS
var highValue = 200;
var constantVal = 2;
var myObj = {
    highValue: 20,
    constantVal: 5,
    calculateIt: function () {
        setTimeout (function () {
            console.log(this.constantVal * this.highValue);
        }, 2000);
    }
} 

myObj.calculateIt(); //400
```

### Alguns motivos para não utilizar variáveis globais:

* Polui o namespace global 
* Sem controle de acesso: poderá ser definida em qualquer parte do programa
* Códigos que utilizam variáveis globais são um pouco mais difíceis de Testar
* Problemas de alocação de memória
* Acoplamento implícito

### Verificando se existe variáveis globais

Podemos verificar se existe uma variável global por meio do window:

```JS
if (window.someVariable) { ... }

Nunca verifique assim , pois lançará um execeção que a variável não foi declarada:
    if (someVariable) { ... }

A verificação poderá ser feita também utilizando typeof:

if (typeof someVariable !== 'undefined') { ... }
```

### Futuro:

Com módulos em ECMAScript6, as variáveis globais se tornarão menos comum, pois variáveis
criadas no top âmbito de um  módulo são apenas global para esse módulo.

----------

# IIFE

![](https://cdn.pbrd.co/images/2rVL8ui1.jpg)

As funções IIFE(Expressão de Função Imediatamente Invocada ou Immediately-Invoked Function Expression)
em JavaScript , são expressões de função imediatamente invocada , ou seja, elas são imediatamente chamadas em tempo de execução e não podemos chamá-las novamente, elas executam apenas uma vez, esse tipo de função é também comumente chamada função anônima. aqui vemos alguns exemplos:

```JS
// IIFE basicamente é isso
(function () {
 
})();
```

```JS
// Aaqui atribuimos a função a uma variável
var logMyName = (function (name) {
    console.log(name); // leo
})('leo');
```

Lembre-se que sem os parênteses ela não funciona é obrigatório usá-los, há algumas **técnicas** para não utilizar os parênteses, porém não é aconselhável a sua utilização, mas vale a pena saber que existe! são as seguintes:

```JS
!function () {
 
}();

+function () {
 
}();

-function () {
 
}();
 
~function () {
 
}();
```

### Escopo Privado em uma IIFE

O exemplo abaixo veremos que a IIFE cria um âmbito privado, isto é, um escopo privado:

```JS
(function (window, document, undefined) {
    var name = 'Foo';
})(window, document);

console.log(name); // undefined
```

Note que  não temos acesso a variável, `var name = 'Foo'` tentamos imprimi-la
e o resultado é `undefined`, pois a IIFE cria um escopo isolado, tornado a nossa
variável inacessível.

### Argumentos da IIFE:

A IIFE tem a capacidade de passar objetos globais(window, document, jQuery, etc) a função anônima de uma IIFE, e em seguida, referenciar estes objetos globais na IIFE através de um escopo local.

```JS
function(window, document, $) {
   //....
}(window, document, window.jQuery);

```
OBS: O JavaScript primeiro procura por uma propriedade no âmbito local, 
e em seguida, ele percorri todo caminho até a cadeia, última parada é no escopo global. 
Essa capacidade das IIFE de colocar objetos globais no âmbito local proporciona velocidades mais 
rápidas de pesquisa interna e desempenho.

### Minificando

Um dos melhores benefícios do padrão IIFE é a minificar as variáveis locais, então
é possível nomeá-las com o nome que você desejar. Vamos a um exemplo:

```JS
// sem minificar as variáveis locais
(function (window, document, undefined) {
    console.log(window); // Object window
})(window, document);

// variáveis minificadas :
(function (a, b, c) {
    console.log(a); // Object window
})(window, document);

```

### Benefícios da IIFE:

* Usado pela maioria das bibliotecas populares (jQuery, Backbone.js, Modernizr, etc)
* Reduz pesquisas de escopo
* Minifica e Otimiza variáveis
* Temos muito mais Legibilidade em nosso código

## Fontes
* **Hoisting**
  * [Variable statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
  * [Escopo de Variável JavaScript e Hoisting Explicado](https://github.com/ericdouglas/traduz-ai/blob/master/javascript/003-escopo-de-variavel-js-e-hoisting-explicado.md)
  * [Hoisting my Labs](https://github.com/leonascimento/labs/blob/master/JavaScript/Vars/Hoisting.js)
  * [JavaScript Hoisting
](http://www.w3schools.com/js/js_hoisting.asp)
  * [Hoisting e escopo em JavaScript](http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/)
  * [Escopo de Variável JavaScript e Hoisting Explicado](https://github.com/ericdouglas/traduz-ai/blob/master/javascript/003-escopo-de-variavel-js-e-hoisting-explicado.md)
  * [Explaining function and variable hoisting in JavaScript](http://jamesallardice.com/explaining-function-and-variable-hoisting-in-javascript/)
  * [JavaScript Hoisting Explained](http://code.tutsplus.com/tutorials/javascript-hoisting-explained--net-15092)
  * [Entendendo escopo e hoisting no JavaScript](https://www.hugobessa.com.br/posts/entendendo-escopo-e-hoisting-no-javascript/) 
  * [JavaScript Hoisting Explained](http://code.tutsplus.com/tutorials/javascript-hoisting-explained--net-15092)
  * [Demystifying JavaScript Variable Scope and Hoisting](http://www.sitepoint.com/demystifying-javascript-variable-scope-hoisting/)
* **Closures**
  * [JavaScript: The Definitive Guide, 6th Edition](http://shop.oreilly.com/product/9780596805531.do)
  * [Practical Uses for Closures](https://medium.com/written-in-code/practical-uses-for-closures-c65640ae7304#.pgin5hz10)
  * [Why use "closure"?](http://howtonode.org/why-use-closure)
  * [Understand JavaScript Closures With Ease](http://javascriptissexy.com/understand-javascript-closures-with-ease/)
  * [Demystifying JavaScript Closures, Callbacks and IIFEs](http://www.sitepoint.com/demystifying-javascript-closures-callbacks-iifes/)
  * [A Random Exploration Of Closure Use Cases In Javascript](http://www.bennadel.com/blog/2134-a-random-exploration-of-closure-use-cases-in-javascript.htm)
  * [What can be done with Closures?](http://www.jibbering.com/faq/faq_notes/closures.html#clClDo)
  * [Why use "closure"?](http://howtonode.org/why-use-closure)
  * [Closures: Front to Back](http://code.tutsplus.com/tutorials/closures-front-to-back--net-24869)
  * [Getting Closure](http://markdaggett.com/blog/2013/02/25/getting-closure/)
  * [Closure em Javascript](https://javiani.wordpress.com/2009/11/07/closure-em-javascript/)
  * [Practical Uses for Closures](https://medium.com/written-in-code/practical-uses-for-closures-c65640ae7304#.709by66cl)
  * [Closures](http://javascript.info/tutorial/closures)
  * [JavaScript Closures Demystified](http://www.sitepoint.com/javascript-closures-demystified/)
  * [JavaScript Closures Explained](https://lostechies.com/derekgreer/2012/02/17/javascript-closures-explained/)
  * [Clausura ciência da computação](https://pt.wikipedia.org/wiki/Clausura_ci%C3%AAncia_da_computa%C3%A7%C3%A3o)
  * [Use Cases for JavaScript Closures](https://msdn.microsoft.com/en-us/magazine/ff696765.aspx)
  * [Example of a Javascript Closure: setTimeout Inside a For Loop](http://brackets.clementng.me/post/24150213014/example-of-a-javascript-closure-settimeout-inside)
* **Variáveis por Parâmetros**
  * [Function parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#Function_parameters)
  * [Parâmetros e Escopos](https://github.com/braziljs/eloquente-javascript/blob/master/chapters/03-funcoes.md)
* **Variável Global**
  * [Tips for using window in JavaScript](http://www.2ality.com/2013/09/window.html)
  * [Global Variables Are Bad](http://c2.com/cgi/wiki?GlobalVariablesAreBad)
* **IIFE**
  * [JavaScript IIFE como contêiner de códigos](http://desenvolvimentoparaweb.com/javascript/javascript-iife-conteiner-de-codigos/)
  * [I Love My IIFE](http://gregfranko.com/blog/i-love-my-iife/)
#Hoisting

Hoisting é o nome dado por Ben Cherry para a resolução de nomes e inicialização 
de valores do JavaScript. Seria algo como a elevação de uma variável ou função 
para o topo do escopo. A linguagem tem um funcionamento diferente de linguagens 
como C, C++ e Java no que diz respeito a declaração e inicialização de variáveis 
e funções. E sem uma explicação realmente fica difícil de entender como o JS 
lida com esse problema.

##Variable Hoisting

Acompanhe os dois exemplos a seguir:

Exemplo 1
```
//Vai gerar o erro: Uncaught ReferenceError: bar is not defined
function foo() {
  console.log(bar);
}
foo();
```

Exemplo 2
```
//Exibe undefined no console e não dá erro! 
function foo() {
  console.log(bar);
  var bar = 1;
}
foo();
```

Mesmo que uma variável seja definida e inicializada, como acontece no Exmeplo 2
o JavaScript só faz o hoisting da sua declaração. Isso é conhecido como Variable
Hoisting, o JS não inicializa a variável dentro do escopo, somente a declara.
Por isso, no Exemplo 2 não ocorre erro, pois ela foi declarada no topo do escopo 
da função. O Exemplo 3 se comporta da mesma forma que o Exemplo 2:

Exemplo 3
```
//Exibe undefined no console 
function foo() {
  var bar;
  console.log(bar);
  bar = 1;
}
foo();
```

##Function Hoisting

Já com Funções o comportamento é um pouco diferente, pois, não só a declaração 
é elevada para o topo, como também o corpo da função, ou seja, sua 
"inicialização" também é "hoisteada". No caso do Exemplo 1, é como se 
a função bar() fosse declarada logo no início do escopo.

Exemplo 1
```
//Exibe 'Hello BeMean' no console 
function foo() {
  bar();
  function bar() {
    console.log("Hello BeMean!");
  }
}
foo();
```

###Exceção a regra

Se uma função for declarada como uma variável (expressão), ela vai seguir o 
hoisting de variáveis. Ex:
```
//Vai gerar o erro: Uncaught TypeError: bar is not a function
function foo() {
  bar();
  var bar = function() {
    console.log("Hello BeMean!");
  };
}
foo();
```

Fonte: http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/


#Closure

Uma closure é uma função interna declarada dentro de outra função, na qual, 
a função interna tem acesso ao escopo da função externa. Closures podem ser
usados para ocultar um estado, criar uma fabrica de objetos, 
para eventos e callbacks.

Exemplo 1 - Ocultando estado de um objeto
```
function Conta(nome, saldo) {
  var message = "Olá " + nome + ", você tem " + saldo + " em saldo.";

  return function saldoAtual() {
    console.log(message);
  };
}

//O cliente acessa a closure da seguinte maneira:
var bemean = Conta("BeMean", 500);
bemean();

```
Exemplo 2 - Fabrica de objetos
```
var ClienteFactory = function() {
    var id = 0;
    var createCliente = function(nome) {
        id++;
        return {
            id: id,
            nome: nome
        };
    };
    return createCliente;
};

var factory = ClienteFactory();
var c1 = factory("Cliente 1");
var c2 = factory("Cliente 2");

alert(c1.id);    // 1
alert(c2.id);    // 2
```
Exemplo 3 - Usando como closure em eventos e callbacks
```
function setAlarm(message, timeout) {

  // Define handle in the closure
  function handle() {
    console.log(message);
  }

  setTimeout(handle, timeout);
}

setAlarm("Wake UP!", 100);
```


#Variável Global

Uma variável global como o nome já diz, pode ser acessada por qualquer 
função dentro do mesmo escopo.

```
function foo() {
  var g = "BeMean"; //var global
  
  function bar() {
    console.log("bar() -> " + g);
    
    function baz() {
      console.log("baz() -> " + g);
    }
    
    baz();
  }
 
  console.log(g); 
  bar();
}

```

#Variável por parâmetro






#IIFE

Melhor exemplo é o module pattern


Fonte: 
https://javiani.wordpress.com/2009/11/07/closure-em-javascript
http://klauslaube.com.br/2011/05/29/afinal-o-que-sao-closures.html







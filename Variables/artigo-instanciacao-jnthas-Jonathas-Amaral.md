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
a função interna tem acesso ao escopo da função externa.

```

function PokeFactory() {
    var id = 0;
    function newPokemon() {
      return {
        id: id,
        name: '',
        description: '',
        height: 0
      };
    }
}

var pikachu = PokeFactory()();


```



#IIFE

Melhor exemplo é o module pattern


Fonte: 
https://javiani.wordpress.com/2009/11/07/closure-em-javascript
http://klauslaube.com.br/2011/05/29/afinal-o-que-sao-closures.html







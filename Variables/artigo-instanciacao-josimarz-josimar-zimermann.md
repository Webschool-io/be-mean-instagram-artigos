# Artigo - Instanciação
**Autor**: Josimar Zimermann - [josimarz](https://github.com/josimarz)
**Data**: 1456063085549

## Hoisting

O termo *hoisting* pode ser traduzido para o português como "içado" e foi cunhado por **Ben Cherry**.

O **hoisting** é um comportamento da linguagem JavaScript que consiste em içar, ou levantar, a declaração de uma variável ou função para o topo. Isto significa que é possível referenciar uma variável JavaScript sem antes declarar essa variável. Observe o exemplo:

```js
try {
    console.log(a);
} catch (e) {
    console.error(e);
    }
```

O exemplo acima irá imprimir no console `ReferenceError: a is not defined`, indicando que a variável `a` não existe. Observe o próximo exemplo:

```js
try {
    console.log(a);
} catch (e) {
    console.error(e);
}
var a;
```

O exemplo acima irá imprimir no console `undefined`. Isto significa que a variável `a`, referenciada na instrução `console.log(a)` existe, mas não foi inicializada. Isso ocorre graças a habilidade de *hoisting* do JavaScript.

No momento de execução do código, quando uma variável é referenciada, o JavaScript conhece essa variável caso a sua declaração tenha sido feita no mesmo escopo na qual ela é referenciada. No entanto, somente a declaração da variável é içada e não a sua inicialização. Veja o exemplo a seguir:

```js
try {
    console.log(a);
} catch (e) {
    console.error(e);
}
var a = 1;
```

Embora a variável `a` seja inicializada, será impresso no console o termo `undefined`, pois a inicialização não é içada, apenas a declaração.

Para compreender cabalmente o funcionamento do *hoisting* em JavaScript, é necessário conhecer como funciona um escopo. Diferente de linguagens como C, C++ ou Java, um escopo em JavaScript não é definido por um bloco de código entre chaves. Existem quatro formas de criar um escopo em JavaScript:

1. Declarando uma função.
2. Usando a função `eval()`.
3. Criando um bloco `catch` dentro de um bloco `try`.
4. Criando um bloco `with`.

No exemplo abaixo temos dois escopos que fazem referência à variáveis homônimas.

```js
var a = 1

function foo() {
    console.log(a);
    var a = 2;
    console.log(a);
}

foo();
console.log(a);
```

O exemplo acima exibirá como resultado:

	undefined
	2
	1

A primeira saída no console é realizada pela primeira instrução `console.log(a)` dentro da função `foo`, acionada a partir do escopo global. Visto que a declaração de uma função resulta num novo escopo, todas as variáveis declaradas dentro desse escopo são visíveis apenas dentro dele.

No exemplo, a variável `a` declarada dentro do escopo da função `foo` irá sobreescrever a variável `a` do escpoo global, mas apenas para o escopo dessa função. Portanto, quando a primeira instrução `console.log(a)` é executada dentro do escopo da função `foo`, acontece o *hoisting* que encontra a declaração da variável `a`.

Depois da execução da primeira instrução `console.log(a)` dentro do escopo da função `foo`, ocorre a inicialização da variável `a`: `var a = 2`. Portanto, na próxima instrução `console.log(a)` dentro da função `foo` o resultado será "2".

Por fim, no escopo global, após chamar a função `foo`, é executada a instrução `console.log(a)`, que apresentará no console o valor "1", inicializado no ropo do código Afinal, o escopo global não é capaz de conhecer as variáveis dentro de escopos por ele contidos.

O *hoisting* aplica-se também à função. Nesse contexto, o cabeçalho da função e seu corpo são içados. Isso significa que é possível chamar uma função que ainda não foi declarada, como exemplifica o código a seguir:

```js
foo();
function foo() {
    console.log('bar');
}
```

O código acima imprime "bar" no console do navegador, embora a função tenha sido chamada antes de sua declaração. Graças ao *hoisting*.

Se duas funções são declaradas dentro do mesmo escopo e com o mesmo nome, então o a última função declarada irá sobreescrever as anteriores, como exemplifica o código a seguir:

```js
function foo() {
    function bar() {
        return 3;
    }

    return bar();

    function bar() {
        return 8;
    }
}

console.log(foo());
```

A execução do código acima irá resultar na impressão do valor "8" no console JavaScript.

No entanto, quando uma função, anônima ou não, é atribuída à uma variável, então aplica-se o *hoisting* de variável. Veja no exemplo:

```js
foo();
var foo = function bar() { return false; };
```

O código acima resultará na mensagem de erro `Uncaught TypeError: foo is not a function(...)`. Isso ocorre porque apenas a declaração da variável foi içada e não sua inicialização.

### Fontes
* [Hoisting e escopo em JavaScript](http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/)
* [Entendendo escopo e hoisting no JavaScript](https://www.hugobessa.com.br/posts/entendendo-escopo-e-hoisting-no-javascript/)

## Closure

Em JavaScript *closures* são funções que referenciam variáveis livres no contexto léxico. Um *closure* também pode ser compreendido como uma função aninhada, como exemplificado no código a seguir:

```js
function init() {
    var name = "Mozilla";
    function displayName() {
        console.log(name);
    }
    displayName();
}
init();
```

O exemplo acima produzirá o termo "Mozilla" no console JavaScript. A função `displayName` é um *closure*, isto é, uma função aninhada dentro da função `init`. Portanto, a função `displayName` é capaz de acessar quaisquer variáveis no escopo da função `init`.

Observe o próximo exemplo:

```js
function makeFunc() {
    var name = "Mozilla";
    function displayName() {
        console.log(name);
    }
    return displayName;
}

var myFunc = makeFunc();
myFunc();
```

Novamente o resultado será a impressão da palavra "Mozilla" no console JavaScript.

A variável `myFunc` guarda o retorno da função `makeFunc`; uma referência à *closure* `displayName`. Depois, `myFunc` é executada como uma função, executando a instrução `console.log(name)` e exibindo como resultado a palavra "Mozilla".

É sabido que variáveis locais a uma função apenas existem durante a execução da mesma. Uma vez que `makeFunc` terminou de executar, é razoável concluir que a variável `name` não existirá mais. Porém, não é isto que acontece.

A função `myFunc` é uma *closure*, isto é, um tipo de objeto que combina a função e o ambiente onde foi criada. Este ambiente é composto por quaisquer variáveis que estavam no escopo no momento em que a função foi criada.

Sempre que uma nova closure é criada, um novo ambiente é criado. Para melhor compreensão, observe o código a seguir:

```js
function makeAdder(x) {
    return function(y) {
        return x + y;
    };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2));
console.log(add(2));
```

A execução do código acima irá produzir:

    7
    12

Quando `var add5 = makeAdder(5)` é executado, um novo ambiente é criado. Nesse ambiente, o "5" é atribuído à variável `x` e uma função anônima, neste caso uma *closure*, é atribuída à variável `add5`.

O mesmo ocorre com a instrução `var add10 = makeAdder(10)`, que atribuí o parâmetro "10" à variável `x`.

Portanto, temos dois ambientes ou contexto. No ambiente de `add5` a variável `x` vale "5", enquanto que no ambiente de `add10` a variável `x` vale "10".

No entanto, as duas funções, `add5` e `add10`, compartilham o mesmo corpo ou definição:

```js
function(y) {
    return x + y;
}
```

Porém, quando executadas, cada uma irá olhar para o seu próprio ambiente, considerando os valores das suas variáveis.

Quando `console.log(add5(2))` é executado, o variável `x` vale "5" no ambiente de execução. Portanto, o resultado será a expressão `5 + 2`.

No contexto da função `add10`, o valor da variável `x` é "10". Quando a instrução `add10(2)` é executada, a expressão `10 + 2` é avaliada, produzindo como resultado o valor "12".

Existem diversas situações nas quais a aplicação das *closure* pode ser uma ótima alternativa.

### Criação de variáveis privadas

A linguagem JavaScript não fornece o conceito nativo de atributos privados. Portanto, todas as propriedades de um objeto JavaScript são públicas. Mas é possível simular atributos privados usando closures, como mostra a definição da classe `Girl`.

```js
// Contrutor da classe Girl
function Girl(name, weight) {

    // Variáveis privadas à classe

    var name = name;
    var weight = weight;

    // Cria uma instância da classe Girl

    var girl = {

        // Método acessor para o atributo name
        getName: function() {
            return name;
        },

        // Método acessor para o atributo weight
        getWeight: function() {
            return weight;
        }
    };

    // Retorna a nova instância
    return girl;
}

// Cria uma nova instância da classe Girl
var katie = new Girl("Katie", 125);

// Verifica a existência da propriedade pública weight no objeto katie
console.log("Weight in? ", ("weight" in katie));

// Obtém o valor da propriedade weight usando o método acessor
console.log("Weight is: ", katie.getWeight());
```

Executando o código acima, será apresentado como resultado no console:

    Weight in? false
    Weight is: 125

As propriedades `name` e `weight` declaradas dentro do escopo da classe `Girl` são visíveis somente dentro do contexto da classe e das suas *closures*. Portanto, as *closures* `getName` e `getWeight`, definidas dentro do objeto `girl`, possuem acesso aos atributos `name` e `weight`.

### Aplicação do *Singleton Design Pattern*

O *Singleton Design Pattern* é um padrão de desenvolvimento que consiste em garantir que uma classe seja instanciada apenas uma única vez no contexto da aplicação.

Podemos usar o recurso *closure* da JavaScript para aplicar o *Singleton*, como mostra o exemplo a seguir:

```js
var Singleton = (function () {
    var instance;

    function createInstance() {
        var obj = new Object('I am the instance');
        return obj;
    };

    return {
        getInstance: function () {
            if (!instance) {
                instance = createInstance();
            }
            return instance;
        }
    };
})();

var instance1 = Singleton.getInstance();
var instance2 = Singleton.getInstance();

console.log("Same instance? " + (instance1 === instance2));
```

O objeto `Singleton` é um módulo que contém uma variável local denominada `instance`. A função `createInstance`, dentro do módulo `Singleton`, é uma *closure* e, portanto, tem acesso à variável `instance`.

Quando o método `getInstance` é executado, é verificado se a variável `instance` foi inicializada. Caso não tenha sido inicializada, a função `createInstance`
é executada, criando um novo objeto e armazenando na variável `instance`. Essa mesma variável e retornada.

Este método garante que apenas um objeto será criado. Quando a instrução `console.log("Same instance? " + (instance1 === instance2))` é executada, o resultado `Same instance? true` é produzido, visto que as duas variáveis, `instance1` e `instance2`, apontam para o mesmo objeto.

#### Fontes

* [Closures](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures)
* [Getting Closure](http://markdaggett.com/blog/2013/02/25/getting-closure/)
* [A Random Exploration Of Closure Use Cases In Javascript](http://www.bennadel.com/blog/2134-a-random-exploration-of-closure-use-cases-in-javascript.htm)
* [Singleton](http://www.dofactory.com/javascript/singleton-design-pattern)

## Variável global

De forma básica, na linguagem JavaScript, uma variável global é uma variável declarada fora do escopo de uma função. Uma variável global é acessível em qualquer parte do código JavaScript.

```js
var foo = 'Global escope';
function bar() {
    console.log(foo);
};
bar();
```

Ao executar o código acima será exibe no console "Global escope". Isto significa que a função `bar` pode ver a variável `foo` que está no escopo global.

No entanto, considere o próximo exemplo:

```js
var foo = 'Global escope';
function bar() {
    var foo = 'Local escope';
    console.log(foo);
};
bar();
console.log(foo);
```

O código acima produzirá o seguinte resultado:

    Local escope
    Global escope

Quando uma variável local denominada `foo` dentro da função `bar` é declarada, ela sobreescreve a variável global homônima. Portanto, quando a instrução `console.log(foo)` é executada dentro do escopo da função `bar`, será exibido o valor da variável local.

No entanto, podem have situações em que deseja-se mesmo acessar a variável global. Para tanto, deve-se utilizar a palavra chave `window`. Esta palavra é um objeto que faz referência ao escopo global.

```js
var foo = 'Global escope';
function bar() {
    var foo = 'Local escope';
    console.log(window.foo);
};
bar();
console.log(foo);
```

o código anterior produzirá o seguinte resultado:

    Global escope
    Global escope

Note que dentro da função `bar` a instrução `console.log(window.foo)` usa a palavra chave `window` para fazer uma referência explícita ao escopo global.

### Fontes

* [Passing local variable with name of a global variable isn't possible in JS?](http://stackoverflow.com/questions/7186171/passing-local-variable-with-name-of-a-global-variable-isnt-possible-in-js)
* [Tips for using window in JavaScript](http://www.2ality.com/2013/09/window.html)
* [GLOBAL VARIABLES IN JAVASCRIPT](http://snook.ca/archives/javascript/global_variable)

## Variável por parâmetro

A linguagem JavaScript possui a capacidade de passar parâmetros para funções de duas formas distintas; por valor e por referência.

A passagem de parâmetros por valor é assumida quando parâmetro é uma variável de um tipo primitivo ou um valor literal. Observe o exemplo:

```js
function foo(x) {
    console.log(x);
    x = 5;
    console.log(x);
}

var x = 4;
console.log(x);
foo(x);
console.log(x);
```

Após executar o código acima o resultado apresentado será:

    4
    4
    5
    4

Visto que a variável `x` é de um tipo primitivo, quando ela é passada para a função `foo`, uma cópia do valor é criado e existente apenas no escopo da função `foo`. Portanto, qualquer operação realizada dentro da função `foo` na variável `x` não afetará a variável `x` no escopo global.

Diferente dos tipos primitivos, os objetos, quando passados como parâmetro de uma função, são passados como referência e, portanto, quaisquer modificações realizadas sobre o objeto dentro da função refletirão no objeto original.

```js
function Obj() {
    this.value = 5;
}

var obj = new Obj();
console.log(obj.value);

function foo(o) {
    o.value = 6;
}

foo(obj);
console.log(obj.value);
```

O resultado da execução do código acima é apresentado a seguir:

    5
    6

Quando o objeto `obj` é instanciado, seu atributo `value` é inicializado com o valor "6". Depois, o objeto `obj` é passado como parâmetro à função `foo` sob o nome `o`. Como a função `foo` recebeu uma referência para um objeto, quando a instrução `o.value = 6` é executada, a operação é realizadao sobre o objeto original.

Considere a necessidade de criar uma função JavaScript que seja capaz de incrementar em 1 qualquer variável global do tipo inteiro. Usando o objeto global `window` é possível acessar as variáveis globais a partir de qualquer contexto. Se o código possui uma variável global denominada `foo` ela pode ser acessada no objeto `window` de duas formas:

```js
console.log(window.foo);
console.log(window["foo"]);
```

Portanto, observe a função abaixo, que recebe o nome de uma variável global e incrementa o seu valor:

```js
function incGlobalVar(varName) {
    window[varName]++;
}

var foo = 0;
incGlobalVar("foo");
console.log(foo);
```

O código acima produzirá como resultado o valor `1`.

### Fontes

* [JAVASCRIPT: PASSING BY VALUE OR BY REFERENCE](http://snook.ca/archives/javascript/javascript_pass)
* [Passing a global variable to a function](http://stackoverflow.com/questions/18178728/passing-a-global-variable-to-a-function)

## Instanciação usando uma IIFE

**IIFE** é um acrônimo para *Immediately-Invoked Function Expression*, que pode ser traduzido como Expressão de Função Invocada Imediatamente. Isso significa que uma IIFE é invocada, ou executada, imediatamente no momento que é declarada.

Existem várias sintaxes para declarar uma IIFE, mas a mais comum dentre elas é esta:

    (
        function [name]([parameters]) {
            // function body
        } ([values to parameters])
    );

Uma IIFE pode ser anônima e também pode receber parâmetros. Observe alguns exemplos de IIFE:

```js
(function () { var x = 10; } ()); // IIFE anônima e sem parâmetros
(function foo() { var x = 10; } ()); // IIFE com nome e sem parâmetros
(function (y) { var x = 10; } (8)); // IIFE anônima e com parâmetro
(function foo(y) { var x = 10; } (8)); // IIFE com nome e com parâmetro
```

Assim como qualquer outra função, **IIFE** pode receber parâmetros por valor ou referência. Variáveis de tipos primitivos e valores literais são passados como valor, ao passo que objetos são passados como referência. Observe o exemplo a seguir:

```js
var a = 0;
(function (x) {
    x++;
    console.log(x);
} (a));
console.log(a);
```

Este código produzirá o seguinte resultado:

    1
    0

Como a variável `a` é de um tipo primitivo, apenas o seu valor é passado para a IIFE. Portanto, a IIFE não altera o valor da variável global `a`.

No próximo exemplo uma IIFE recebe como parâmetro um objeto:

```js
var obj = {
    name: 'Pedro Álvares'
};
(function (o) {
    o.name = o.name + ' Cabral';
} (obj));
console.log(obj.name);
```

A execução do código acima produzirá como resultado "Pedro Álvares Cabral". Como a variável `obj` é um objeto, ela é passada como referência à IIFE. Portanto, qualquer operação realizada sobre esse objeto dentro da IIFE irá refletir no objeto original.

Uma IIFE também pode retornar um valor que pode ser atribuído à uma variável. Tal qual uma função, deve-se utilizar a palavra chave `return`. Observe o próximo exemplo:

```js
var p = (function () { return "Pedro Álvares Cabral"} ());
```

A IIFE executará imediatamente, retornando o valor "Pedro Álvares Cabral", que é atribuído à variavel `p`.

### Fontes

* [Immediately-invoked function expression](https://en.wikipedia.org/wiki/Immediately-invoked_function_expression)
* [Sobre funções imediatas JavaScript (IIFE)](http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/?trace=1519021197&source=single)
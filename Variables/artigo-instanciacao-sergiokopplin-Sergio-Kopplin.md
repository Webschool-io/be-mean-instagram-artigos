# Entendendo a criação e instanciação de variáveis no Javascript
**Autor**: [Sérgio Kopplin](https://github.com/sergiokopplin)
**Link Blog**: [Instanciação no JS](http://koppl.in/instanciacao-no-js/)
**Data**: TUESDAY. JANUARY 26, 2016

O JS é uma linguagem extremamente interessante e pode deixar os programadores caçando erros e bugs por conta de suas minúncias. Dentre elas, a declaração de variáveis.

Segundo o [MDN](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/var), "Declarações de variáveis, onde quer que elas ocorram, são processadas antes que qualquer outro código seja executado.", porém, isso não significa que as variáveis terão valores definidos antes da execução de outros código. Vamos entender esse processo melhor, através do Hoisting, Closure, Escopo Global, Escopo por parâmetro e IIFE.

---

## Hoisting

No Javascript as variáveis são declaradas em escopo [Hoisting](http://www.wordreference.com/enpt/hoist). Hoisting, na tradução literal, significa elevar, subir ou içar. Logo, no escopo em questão, a variável é 'levantada' e sua declaração ocorre antes da execução do restante do código, esse processo então recebe esse nome. Vamos ao exemplo:

{% highlight js %}
// exemplo a
// imprimi o erro dentro do `catch`
try {
  console.log(a)
} catch (e) {
  console.error('A variável `a` não foi definida.')
}

// exemplo b
// imprimi `undefined`
try {
  console.log(a)
  var a = 2
} catch (e) {
  console.error('A variável `a` não foi definida.')
}
{% endhighlight %}

No **exemplo a**, a variável não havia sido declarada em nenhum momento, logo o erro foi acusado pelo console. No **exemplo b** nós temos um erro diferente, pois a declaração da variável a foi elevada pelo *hoisting* ao topo da execução, porém, a atribuição de valor ocorre depois da chamada do console.log, logo, nós temos a sua declaração válida mas sua atribuição inexistente.

---

## Closure

Closure é um conceito em linguagem de programação que, de forma simplificada, permite que uma função acesse variáveis de sua função parente. Essa técnica é oriunda de linguagens funcionais, mas acabou sendo difundido e implementado também para outras linguagens como JavaScript e C#.

Considere o seguinte:

{% highlight js %}
function init() {
    var name = "Mozilla"; // variável local criada pelo método init()
    function displayName() { // displayName() é uma função interna, uma closure
        alert(name); // usa uma variável declaada na função pai
    }
    displayName();
}
init();

// resultado
// alert: Mozilla
{% endhighlight %}

Nesse caso, a função **init** possui variáveis locais, porém a **displayName** não declara nenhuma internamente, mesmo assim consegue usar a variável name pois se encontra no mesmo escopo.

#### Diferenças das Closures:

Considere o seguinte exemplo, retirado do MDN:

{% highlight js %}
function makeFunc() {
  var name = "Mozilla";
  function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc();
myFunc();
{% endhighlight %}

Nesse exemplo teremos o mesmo resultado do anterior, porém uma closure foi criada. A função **myFunc** agora é uma função derivada de **makeFunc** e recebe todo o ambiente contido, incluíndo a variável **name** e a função **displayName**. E isso meus amigos, é a definição simples para closure.

> A closure is a special kind of object that combines two things: a function, and the environment in which that function was created. The environment consists of any local variables that were in-scope at the time that the closure was created. (MDN)

---

## Variável Global

Variáveis globais podem ser criadas no JS com o auxílio da reservada **var**. Podemos então utilizar o seguinte exemplo:

{% highlight js %}
var silvio = "maoe";
function foo() {
    alert(silvio);
}

foo();

// resultado
// alert: maoe
{% endhighlight %}

Nossa variável **silvio** não pertence a um escopo de função, portanto pode ser acessada em qualquer lugar de nossa aplicação. Mas podemos melhorar ainda mais essa declaração declarando a variável dentro da nossa **window** da seguinte maneira:

{% highlight js %}
window.silvio = "maoe";
function foo() {
    alert(silvio);
}

foo();

// resultado
// alert: maoe
{% endhighlight %}

Com isso podemos ter a certeza de que nossa variável não é *local* em nenhum aspecto. O curioso é perceber que qualquer variável vai para a window e isso se torna redundante, pois no primeiro exemplo nossa variável poderia ter sido acessada pela window, com a chamada **window.silvio**, ou ainda melhor, apenas **silvio**.

Vale ressaltar que se críarmos uma variável dentro de um escopo, ela não irá para nossa **window**, por exemplo:

{% highlight js %}
function sbt() {
    var silvio = "maoe"
}

window.maoe
// resultado
// undefined
{% endhighlight %}

---

## Variável por parâmetro

Variáveis passadas por parâmetro são passadas a funções para que possam ser utilizadas internamente nas regras de negócio, logo se tornam parte do escopo local da função. Por exemplo:

{% highlight js %}
function soma(val1, val2) {
    return val1 + val2;
}

soma(3, 5);

// resultado
// 8
{% endhighlight %}

No nosso exemplo a variável passada por parâmetor para a função foi usada em no escopo local de **soma**. Sem mistérios nesse processo.
Se passarmos variáveis **globais** definidas anteriormente, podemos obter resultados diferentes, confira:

{% highlight js %}
var glob1 = 10
    , glob2 = 20;

function sub(v1, v2){
    glob1 = glob1 + 10; // glob1 muda para 20
    return glob1 - glob2; // 20 - 20
}

sub(glob1, glob2)
// resultado
// 0
{% endhighlight %}

No nosso exemplo, a variável global adquiriu um escopo local dentro da função, transformando-se em outra variável, que apesar de ter outro nome, passou a ter um espaço alocado na memória diferente, ou seja, temos uma variável **global** chamada **glob1** com o valor *10* e uma outra **local** da função **sub** que tem o valor de *20*.

---

## Instanciação usando uma IIFE

Uma IIFE pode ser descrita como **Immediately-invoked function expression**, o que pode também ser traduzido como **Função/Expressão Imediatamente Invocada** (risos). O seu uso mais recorrente é visto quando queremos definir variáveis em um escolo local e queremos que elas não passem para o escopo global e sejam definidos na window, logo, podemos criar escopos onde precisamos e terceiros não poderão alterá-las em um escopo global. Vejamos no exemplo:

{% highlight js %}
(function () {
    'use strict';

    var sayHi = 'oi';
    console.log(sayHi);
}());

console.log(sayHi) // ReferenceError: sayHi is not defined
{% endhighlight %}

Esse exemplo é bem simples, mas explica o todo da nossa IIFE. Dentro dela temos a abertura do primeiro parênteses, o que significa que estamos criando um escolo local para a nossa função.
Sendo assim, logo após o fechamento das nossas chaves, temos em seguida os **()** que expressam ao JS o nosso desejo de invocarmos essa função imediatamente.

> Você deve usar uma IIFE sempre que surgir a necessidade de isolar as variáveis que você precisa do escopo global. Isso é uma boa prática e é uma forma de fazer com que seu código funcione bem independente de onde ele for usado, são práticas como essa, que libs como jQuery, AngularJS, React e várias outras libs e frameworks usam. (@felquis)

---

Referências:

- [LoopInfinito - hoisting-e-escopo-em-javascript](http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/)
- [stackoverflow.com](http://stackoverflow.com/questions/15395347/does-a-browser-truly-read-javascript-line-by-line-or-does-it-make-multiple-passe)
- [developer.mozilla.org - JavaScript/Guide/Closures](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures)
- [pt.stackoverflow.com](http://pt.stackoverflow.com/questions/1859/como-funcionam-closures-em-javascript)
- [javascriptissexy.com](http://javascriptissexy.com/understand-javascript-closures-with-ease/)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
- [http://benalman.com/news/2010/11/immediately-invoked-function-expression/](http://benalman.com/news/2010/11/immediately-invoked-function-expression/)
- [http://tutsmais.com.br/blog/javascript-2/o-que-e-iife-no-javascript/](http://tutsmais.com.br/blog/javascript-2/o-que-e-iife-no-javascript/)

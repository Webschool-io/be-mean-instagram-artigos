# Como o Javascript cria e instancia variáveis
autor: Carlan Calazans (carlancalazans at gmail.com)

## Hoisting

Hoisting é um termo que possui o significado de elevar ou puxar para cima. Basicamente, em Javascript, significa que uma variável ou função não estão declarados no local onde você pensa. Além disso, variáveis e funções são tratados de forma diferenciada.

Para entender o hoisting mais facilmente, primeiro entenderemos como funciona o escopo de variáveis e funções em Javascript.

### Escopo de variáveis

O escopo de uma variável é simplesmente o contexto no qual a variável existe. Esse escopo permite especificar de onde você pode acessar uma variável e se você possui acesso à variável em um determinado contexto.

Em Javascript, temos dois tipos de escopo para variáveis: local ou global.

#### Local

Variáveis declaradas dentro de uma função são variáveis locais, ou seja, elas existem somente dentro das funções onde foram declaradas. É obrigatório o uso da palavra-chave `var` antes das declarações de variáveis locais. Caso contrário, elas serão globais.

```javascript
var age = 36;

function getAge() {
	var age = 26; // variavel local declarada com a palavra-chave var
	console.log(age); // imprime 26
}

console.log(age); // variavel global e imprime 36
```
[Rode o código acima no JS Bin](https://jsbin.com/xurupucemo/edit?js,console)

Nesta explicação também se enquadra as variáveis passadas por parâmetros ou variáveis de parâmetros. Estas variáveis são tratadas como variáveis locais.

```javascript
var age = 36;

function getAge(any) {
	console.log(any);
}

getAge(age); // imprime 36

console.log(age); // imprime 36
```
[Rode o código acima no JS Bin](https://jsbin.com/suxuzusaqo/edit?js,console)

As variáveis locais também estão disponíveis para funções declaradas dentro de uma função (inner function). Por exemplo:

```javascript
function getAge() {
	var age = 26;

	function printAge() {
		console.log(age);
	}

	printAge();
}

getAge(); // imprime 26
```
[Rode o código acima no JS Bin](https://jsbin.com/paqomecoji/edit?js,console)

Leia mais sobre o assunto no tópico sobre [Closure](#closure).

Para finalizar, mais um exemplo de declaração de variável local que pode confundir quem programa também em outras linguagens de programação.

```javascript
function podeBeber(anyAge) {
	if (anyAge > 20) {
		var text = "Pode beber ate cair";
		console.log(text);
	}

	console.log(text);
}

podeBeber(21);
```
[Rode o código acima no JS Bin](https://jsbin.com/wuciweweve/edit?js,console)

A declaração e inicialização da variável `text` está sendo realizado dentro de um bloco `if`, mas o escopo desta variável é dentro da função `podeBeber`.

Em um futuro bem próximo (EcmaScript 6), a declaração das variáveis precedidas com a palavra-chave `let` existirão ou serão visíveis somente dentro de um bloco de código.

#### Global

Qualquer variável declarada fora de uma função é uma variável global. O uso da palavra-chave `var` não é obrigatório.

```javascript
// Todas as declaracoes abaixo sao validas

var myAge = 36;

myAge = 36;

var myAge;

myAge;
```

**Cuidado!** Sem utilizar a palavra-chave `var` na declaração de uma variável dentro de uma função, ao invés de escopo local, você estará utilizando uma variável no escopo global.

```javascript
var age = 36;

function getAge() {
	age = 26; // variavel global
	console.log(age); // imprime 26
}

getAge();
console.log(age); // imprime 26
```
[Rode o código acima no JS Bin](https://jsbin.com/curosuviri/edit?js,console)

### Hoisting de variáveis

Toda a declaração de uma variável é elevada (hoisted) para o topo de uma função na qual está inserida.

```javascript
var age = 36;

function getAge() {
	console.log(age);
	var age = 26;
	console.log(age);
}

getAge();

//output:
//undefined
//26
```
[Rode o código acima no JS Bin](https://jsbin.com/yovubozoxu/edit?js,console)

A razão do output apresentado acima é por que a declaração da variável local `age` foi levantada para o topo da função.

A primeira coisa que o interpretador do Javascript procura dentro de uma função é a declaração de variáveis. E ao encontrar, as declarações são levadas para o início da função. Neste caso, não importa se temos uma variável global `age`, pois a prioridade é da variável local.

O código da função `getAge()` é processado da forma apresentada abaixo:

```javascript
function getAge() {
	var age;
	console.log(age); // age sem valor e undefined
	age = 26; // inicializacao ou atribuicao de valor
	console.log(age); // imprime 26
}

getAge();
```
[Rode o código acima no JS Bin](https://jsbin.com/pumubapeqi/edit?js,console)

### Hoisting de funções

O que diferencia o hoisting de funções e variáveis é a predecência. Veja bem, uma função possui um peso maior sobre uma declaração de uma variável.

```javascript
var getAge;

function getAge() {
	console.log("getAge");
}

console.log(typeof getAge); // imprime function
```
[Rode o código acima no JS Bin](https://jsbin.com/radevuliga/edit?js,console)

No entanto, no código abaixo a variável global prevalece por causa da inicialização da variável.

```javascript
var getAge = 26; // inicializacao

function getAge() {
	console.log("getAge");
}

console.log(typeof getAge); // imprime number
```
[Rode o código acima no JS Bin](https://jsbin.com/jutomoceja/edit?js,console)

## Closure

De acordo com a [Wikipedia](https://pt.wikipedia.org/wiki/Clausura_(ci%C3%AAncia_da_computa%C3%A7%C3%A3o)):

> Uma clausura ocorre normalmente quando uma função é declarada dentro do corpo de outra, e a função interior referencia variáveis locais da função exterior. Em tempo de execução, quando a função exterior é executada, então uma clausura é formada, que consiste do código da função interior e referências para quaisquer variáveis no âmbito da função exterior que a clausura necessita.

Em resumo, uma closure é qualquer função que mantém referência a variáveis do escopo do pai mesmo após o pai ter sido retornado.

O pulo do gato para entender closures é entender como funções dentro de funções funcionam. Ficou esquisito esta última frase, não é? Mas, é isso mesmo.

### Função dentro de função

1) Uma função filha (ou inner function) pode referenciar o escopo da função pai (outer function)

```javascript
function digaOi(name) { // outer function

  function oi() { // inner function
    console.log("Oi, " + name + "!");
  }

  oi();

}
digaOi("Carlan"); // imprime Oi, Carlan!
```
[Rode o código acima no JS Bin](https://jsbin.com/miposoweji/edit?js,console)

![](https://www.dropbox.com/s/63cwx2j2ndh64nz/closure-1.svg?raw=1)

No exemplo acima, a função `oi()` possui acesso a variável `name` da função `digaOi(name)`. Ao chamar a função `digaOi(name)` passando como parâmetro o meu primeiro nome, a função `oi()` pode acessar as variáveis e parâmetros da função `digaOi(name)` imprimindo no console o resultado "Oi, Carlan!".

2) Uma função filha (ou inner function) pode referenciar uma variável definida na função pai (ou outer function) mesmo depois da função pai ter sido retornada

```javascript
function digaOi(name) { // outer function

  function oi() { // inner function
    console.log("Oi, " + name + "!");
  }

  return oi;
}

var fnDigaOi = digaOi("Carlan");
fnDigaOi();

console.log(fnDigaOi);
// a linha acima imprime o corpo da funcao oi():
//
// function oi() { // inner function
//  console.log("Oi, " + name + "!");
// }
```
[Rode o código acima no JS Bin](https://jsbin.com/sofefurinu/edit?js,console)

![](https://www.dropbox.com/s/bcvnlo0t18jxaax/closure-2.svg?raw=1)

A única diferença entre o exemplo anterior é que a função filha (ou inner function) está sendo retornada dentro da função pai (ou outer function), ao invés de ser chamada. O valor da variável `fnDigaOi` é a função `oi()`.

Como pode ser observado, aparentemente a função `digaOi(name)` já não existe, mas a função `oi()` ainda pode acessar o seu escopo, a variável `name`.

3) Uma função filha (ou inner function) armazena o valor de variáveis do escopo da função pai (ou outer function) por referência e não valor

```javascript
function digaMeuNome() { // outer function
  var name = "Carlan";

  return {
    getName: function() { console.log(name); },
    setName: function(newName) { name = newName; }
  };

}

var fnDigaMeuNome = digaMeuNome();
fnDigaMeuNome.getName(); // imprime Carlan
fnDigaMeuNome.setName("Calazans");
fnDigaMeuNome.getName(); // imprime Calazans
```
[Rode o código acima no JS Bin](https://jsbin.com/danocaroju/edit?js,console)

A função pai (ou outer function) `digaMeuNome()` retorna duas closures. Uma para obter o meu nome chamada `getName()` e outra para alterar o meu nome chamada `setName(newName)`. Observe que as duas fazem referência a variável `name`, definida no escopo da função pai. No decorrer do exemplo, é possível observar que ao chamar a função `getName()` o meu primeiro nome é impresso e ao chamar a função `setName(newName)` a variável `name` passa a ser o meu sobrenome.

Com esse exemplo é possível perceber que as closures compartilham as variáveis definidas no escopo da função pai e armazenam referências a estas variáveis.

### Onde usar

1) "Variáveis de instância" e fábrica de objetos ou pattern module

```javascript
function Pokemon(pname, pattack, pdefense, ptype) {

  var name = pname;
  var attack = pattack;
  var defense = pdefense;
  var type = ptype;

  return {
    getName: function() {
      console.log('[LOG] returning name');
      return name;
    },
    setName: function(newName) {
      console.log('[LOG] setting name to ' + newName);
      name = newName;
    },
    getAttack: function() {
      console.log('[LOG] returning attack');
      return attack;
    },
    setAttack: function(newAttack) {
      console.log('[LOG] setting attack to ' + newAttack);
      attack = newAttack;
    }
    // ...
  };

}

pikachu = Pokemon('Pikachu', 0.5, 22, ['electric']);

// console.log(pikachu.name); // undefined

console.log(pikachu.getName()); // imprime Pikachu

console.log(pikachu.getAttack()); // imprime 0.5

pikachu.setAttack(0.8); // seta o novo valor de attack

console.log(pikachu.getAttack()); // imprime 0.8

console.log(typeof pikachu); // imprime object

// no entanto, ainda podemos fazer isso
pikachu.name = 'UUUUUU';
console.log(pikachu.name);
```
[Rode o código acima no JS Bin](https://jsbin.com/rubujohipa/edit?js,console)

O exemplo acima está sendo utilizado para mostrar dois casos de uma vez. O primeiro deles é referente a "variáveis de instância" (com aspas). A manipulação das variáveis informadas pela função Pokemon só acontecerá através das closures definidas dentro do objeto retornado. O segundo caso é uma fábrica de objetos, duh.

2) Funções de callback

```javascript
// imprime a receita
function printRecipe(recipeName, ingredients, steps) {
  console.log('Receita de ' + recipeName);
  console.log('Ingredientes:');
  for (var ingredient in ingredients) {
    if (ingredients.hasOwnProperty(ingredient)) {
      console.log("  - " + ingredient + " (" + ingredients[ingredient] + ")");
    }
  }
  console.log('Passo a passo:');
  for (var i = 1; i <= steps.length; i++) {
    console.log(i + ". " + steps[i-1]);
  }

}

// recebe o input do usuario
function getInput(recipeName, ingredients, steps, callback) {
  if(typeof callback === 'function') {
	callback(recipeName, ingredients, steps);
  }
}

// entrada de dados e inicializacao

var r = "Cup Cake";

var i = {
  'Baunilha': '12ml',
  'Fermento em pó': '12g',
  'Leite': '220ml',
  'Ovos': '4',
  'Açúcar': '350g',
  'Margarina': '220g',
  'Farinha de trigo': '350g',
  'Sal': '2g'};

var s = [
  'Na tigela da batedeira coloque açúcar refinado...',
  'Adicione os ovos um a um com a batedeira...',
  'Em outra tigela, adicione a baunilha no leite e reserve.',
  '...',
  'Leve ao forno pré-aquecido por 25m'];

// chamada da funcao que obtem o input do usuario com os dados acima
getInput(r, i, s, printRecipe);
```
[Rode o código acima no JS Bin](https://jsbin.com/xipajopaxu/edit?js,console)

Uma função de callback é uma função que é passada como argumento para outra função e em um dado momento a função de callback é executada dentro da outra função. Deu pra perceber que funções de callback são closures?

Veja bem, quando passamos uma função de callback para outra função, a função de callback é executada dentro do corpo da outra função como se tivesse sido escrita lá dentro.

## Instanciação usando uma IIFE

>Uma função imediata é um padrão que produz um escopo léxico unado a função de escopo do JavaScript. Funções imediatas podem ser usadas para evitar a elevação de variáveis (hoisting) dentro de blocos, protegendo o ambiente global de ser poluído e simultanamente permite o acesso público a métodos enquanto retem a privacidade para as variáveis declaradas dentro da função.

De forma mais simples, uma IIFE é uma função que é executada logo após a sua criação.

Há mais de uma maneira de se escrever uma IIFE, mas a versão mais comum utilizada é conforme apresentado abaixo:

```javascript
(function() {
  // corpo
  // escopo proprio
})();
```

Observe que trata-se de uma função envolvida com parênteses, depois temos um abre e fecha parênteses e, por fim um ponto e vírgula.

Para transformar uma função em uma IFFE, basta fazer conforme foi explicado no parâgrafo anterior. No entanto, como ficam as funções que possuem parâmetros?

Para enviar parâmetros para esta função basta:

```javascript
(function(name, type) {
  // a variavel name contem "Pikachu"
  // a variavel type contem ['electric']
})("Pikachu", ['electric']);
```

É possível atribuir uma IFFE a uma variável da seguinte maneira:

```javascript
var digaOi = function() {
  console.log('oi');
}();
```

Uma IFFE geralmente é utilizada para criar um escopo próprio. Dentro da IFFE o escopo é privado e protegido de modificações acidentais.

## Referências

* [JavaScript Variable Scope and Hoisting Explained](http://javascriptissexy.com/javascript-variable-scope-and-hoisting-explained/)
* [Understand JavaScript Closures With Ease](http://javascriptissexy.com/understand-javascript-closures-with-ease/)
* [Wikipedia sobre closure](https://pt.wikipedia.org/wiki/Clausura_(ci%C3%AAncia_da_computa%C3%A7%C3%A3o))
* [JavaScript Closures 101- they're not magic](http://www.javascriptkit.com/javatutors/closures.shtml)
* [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
* [Variable Scope Basics](http://www.kirupa.com/html5/variable_scope_js.htm)
* [Closures in JavaScript](http://www.kirupa.com/html5/closures_in_javascript.htm)
* [JavaScript Variables](http://www.w3schools.com/js/js_variables.asp)
* [JavaScript Closures](http://www.w3schools.com/js/js_function_closures.asp)
* [Why use "closure"?](http://howtonode.org/why-use-closure)
* [Wikipedia sobre IIFE](https://en.wikipedia.org/wiki/Immediately-invoked_function_expression)
* [JavaScript: The Right Way](http://jstherightway.org/pt-br/)
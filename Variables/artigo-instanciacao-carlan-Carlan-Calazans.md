# Como o Javascript cria e instancia variáveis
autor: Carlan Calazans <carlancalazans at gmail.com>

## Hoisting

Hoisting é um termo que possui o significado de elevar ou puxar para cima. Basicamente, em Javascript, significa que uma variável ou função não estão declarados no local onde você pensa. Além disso, variáveis e funções são tratados de forma diferenciada.

Para entender o hoisting mais facilmente, primeiro entenderemos como funciona o escopo de variáveis e funções em Javascript.

### Escopo de variáveis

O escopo de uma variável é simplesmente o contexto no qual a variável existe. Esse escopo permite especificar de onde você pode acessar uma variável e se você possui acesso à variável em um determinado contexto.

Em Javascript, temos dois tipos de escopo para variáveis: local ou global.

#### Local

Variáveis declaradas dentro de uma função são variáveis locais, ou seja, elas existem somente dentro das funções onde foram declaradas. É obrigatório o uso da palavra-chave `var` antes das declarações de variáveis locais.

```
var age = 36;

function getAge() {
	var age = 26; // variavel local declarada com a palavra-chave var
	console.log(age); // imprime 26
}

console.log(age); // variavel global e imprime 36
```
[Rode o código acima no jsbin](https://jsbin.com/xurupucemo/edit?js,console)

#### Global

Qualquer variável declarada fora de uma função é uma variável global. O uso da palavra-chave `var` não é obrigatório.

```
// Todas as declaracoes abaixo sao validas

var myAge = 36;

myAge = 36;

var myAge;

myAge;
```

### Hoisting de variáveis

Toda a declaração de uma variável é elevada (hoisted) para o topo de uma função na qual está inserida.

```
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
[Rode o código acima no jsbin](https://jsbin.com/cedamobeju/edit?js,console)

A razão do output apresentado acima é por que a declaração da variável local `age` foi levantada para o topo da função. O código da função `getAge()` é processado da forma apresentada abaixo:

```
function getAge() {
	var age;
	console.log(age); // age sem valor e undefined
	age = 26; // inicializacao ou atribuicao de valor
	console.log(age); // imprime 26
}

getAge();
```
[Rode o código acima no jsbin](https://jsbin.com/pumubapeqi/edit?js,console)

### Hoisting de funções

O que diferencia o hoisting de funções e variáveis é a predecência. Veja bem, uma função possui um peso maior sobre uma declaração de uma variável.

```
var getAge;

function getAge() {
	console.log("getAge");
}

console.log(typeof getAge); // imprime function
```
[Rode o código acima no jsbin](https://jsbin.com/radevuliga/edit?js,console)

No entanto, no código abaixo a variável global prevalece.

```
var getAge = 26; // inicializacao

function getAge() {
	console.log("getAge");
}

console.log(typeof getAge); // imprime number
```
[Rode o código acima no jsbin](https://jsbin.com/jutomoceja/edit?js,console)

## Closure

> Uma clausura ocorre normalmente quando uma função é declarada dentro do corpo de outra, e a função interior referencia variáveis locais da função exterior. Em tempo de execução, quando a função exterior é executada, então uma clausura é formada, que consiste do código da função interior e referências para quaisquer variáveis no âmbito da função exterior que a clausura necessita.

Abaixo está um exemplo que será transformado em uma closure, mas se encontra em um formato mais simples de ser lido e entendido. Veja o exemplo:

```
function init() {
  var name = "Mozilla"; // name é uma variável local criada pela function init
  function displayName() { // displayName() é uma inner function
    console.log(name); // usando uma variavel declarada na funcao pai
  }
  displayName();
}
init();
```
[Rode o código acima no jsbin](https://jsbin.com/vovilayeho/edit?js,console)

1. A função `init()` cria uma variável local chamada `name`.
2. A seguir é criada uma função chamada `displayName()`. Ela pode ser chamada de inner function por ter sido definida dentro da função `init()` e está disponível somente dentro desta função.
3. A função `displayName()` não possui nenhuma variável local. No entanto, possui acesso a variáveis da função pai `init()`.

Agora veja esse outro exemplo:

```
function init() {
  var name = "Mozilla"; // name é uma variável local criada pela function init
  function displayName() { // displayName() é uma inner function
    console.log(name);
  }
  return displayName; // pulo do gato
}

var myFunc = init();
myFunc();
```
[Rode o código acima no jsbin](https://jsbin.com/vuzawitelo/edit?js,console)

Os dois códigos acima possuem o mesmo resultado, ou seja, a string Mozilla é impressa no console. O que realmente muda entre os dois exemplos é que em um deles a função `displayName()` é executada e em outra a função `displayName()` é retornada antes de ser executada.

Com isso é possível definir uma closure como um tipo especial de objeto que combina duas coisas:

1. Uma função
2. Um ambiente no qual a função foi criada.

O ambiente consiste em qualquer variável local que estava no escopo na hora que a closure foi criada.

De forma mais simples, uma closure é uma função filha que possui acesso aos membros da função pai. Uma função filha possui acesso aos seguintes escopos: ao seu escopo, ao escopo do pai e ao escopo global.

Um exemplo simples para finalizar:

```
function some(from) {
	return function(to) {
		return from + to;
	};
}

var soma1 = some(1);
var soma5 = some(5);

console.log(soma1(1)); // imprime 2
console.log(soma5(5)); // imprime 10
```
[Rode o código acima no jsbin](https://jsbin.com/sijerefugi/edit?js,console)

Mais um para ficar bem claro:

```
var PokemonFactory = function() {
    var id = 0;
    var createPokemon = function(name, attack, defense, type) {
        id++;
        return {
            id: id,
            name: name,
            attack: attack,
            defense: defense,
            type: type
        };
    };

    return createPokemon;
};

var newPokemon = PokemonFactory();
var pikachu = newPokemon( "Pikachu", 0.5, 22, ['electric']);
var charmander = newPokemon("Charmander", 0.2, 34, ['fire']);
var bulbasaur = newPokemon("Bulbasaur", 0.3, 47, ['grass', 'poison']);

console.log(pikachu);
console.log(charmander);
console.log(bulbasaur);
```
[Rode o código acima no jsbin](https://jsbin.com/picicitato/edit?js,console)

## Variável por parâmetro

## Instanciação usando uma IIFE

## Referências

* [JavaScript Variable Scope and Hoisting Explained](http://javascriptissexy.com/javascript-variable-scope-and-hoisting-explained/)
* [Understand JavaScript Closures With Ease](http://javascriptissexy.com/understand-javascript-closures-with-ease/)
* [Wikipedia](https://pt.wikipedia.org/wiki/Clausura_(ci%C3%AAncia_da_computa%C3%A7%C3%A3o))
* [JavaScript Closures 101- they're not magic](http://www.javascriptkit.com/javatutors/closures.shtml)
* [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

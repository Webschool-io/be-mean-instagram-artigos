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

## Variável por parâmetro

## Instanciação usando uma IIFE


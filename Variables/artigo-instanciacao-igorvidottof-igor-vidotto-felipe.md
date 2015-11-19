# Artigo - Instanciação de Variáveis no Javascript

**autor:** Igor Vidotto Felipe

## 1 - Introdução
Para abstrair melhor os conceitos que serão abordados nesse artigo faz-se necessário ter um conhecimento prévio sobre **variáveis** e **funções** no JavaScript. Essa introdução conterá o básico sobre ambos os temas para orientação.

Os conceitos principais abordados serão **Hoisting, Closure, Variável Global, Variável por Parâmetro e Instanciação usando uma IIFE**.

### 1.1 - Variáveis
Variáveis são comuns em todas as linguagens de programação, são uma forma de guardar valores que serão usados posteriormente. No JavaScript há uma pequena diferença ao se declarar uma variável, posto que não é uma linguagem dinâmica e não "altamente tipada".

#### 1.1.1 - Forma de Declaração
```javascript
var a; //undefined
var num = 5; //number
var nome = "Igor"; //string
var teste = true; //boolean
```
> Também é possível definir uma variável sem a palavra chave `var`, será exemplificado mais adiante neste artigo suas aplicações.
> Além desses exemplos de tipos primitivos do JavaScript, ainda existe o `null`, que, juntamente com o `undefined` também são considerados tipos especiais.

#### 1.1.2 - Outros tipos de dados
Além dos citados na seção 1.1.1, existem outros tipos de dados. Os dados compostos, que comportam o `Object`, `Array` e `Function`.

### 1.2 - Funções
Em JavaScript, as funções também são consideradas como valores, isto significa que elas podem fazer tudo o que outros valores e objetos podem fazer. É possível até definir uma função dentro de outra, enviá-la como parâmetro, ser atribuida a uma variável ou ainda guardá-la num array. Em termos técnicos, isso significa que as funções são consideradas Objetos de Primeira Classe.

#### Formas de Declaração
Há três formas de declarar uma função:

##### 1.2.1 - Function Declaration
A mais habitual e utilizada em muitas outras linguagens de programação.
```javascript
function hello(){
	console.log("Hello World");
};
```
> Tomando como o exemplo acima, essa função também recebe o título de **named function**, onde `hello` é seu nome.

##### 1.2.2 - Function Expression
Outra forma de criar uma função é utilizando a notação da Function Expression. Esta, por sua vez, atribui uma função anônima a uma variável.
```javascript
var goodbye = function(){
	console.log("Goodbye World");
};
```

Alternativamente, é possível criar uma **named function expression**, para isso basta atribuir um nome a função.
```javascript
var goodbye = function bye(){
	console.log("Goodbye World");
};
```
> Neste exemplo o nome da função passa a ser **bye**, no entanto ela não perde seu vínculo com a variável goodbye.

##### 1.2.3 - Function() Constructors
Uma função também pode ser declarada usando o construtor Function(). O corpo da função é passando dentro de uma string.
```javascript
var hi = new Function("return 'Hi World';");
```
> Percebe-se que a sintaxe é muito mais complicada e sua criação é mais lenta que as outras formas.

## 2 - Palavras-chave
*JavaScript*, *Function*, *Hoisting*, *Closure*, Variáveis.

## 3 - Conceitos Principais

### 3.1 - Hoisting
`Hoisting` é o processo de mover um valor (variável ou função) ao topo do bloco de código onde ele é utilizado, independentemente de onde ele foi definido. 
Possibilita a declaração de variáveis e funções em qualquer local do escopo em que elas serão utilizadas, evitando assim interrupções por exceção. 

#### 3.1.1 - Variable Hoisting
Todas as variáveis declaradas são, automaticamente, movidas para o topo do escopo da função à qual ela está inserida, como se elas fossem defininidas no começo da função. Todavia seus valores não são movidos juntamente com elas. Isto significa uma variável que recebeu um valor no meio ou final da função terá seu valor `undefined` no topo, até que a atribuição ocorra de fato.
```javascript
function exemplo(){
	// como a variável a existe nesse escopo ela foi movida aqui para o topo 
	// var a; (efeito do hoisting)
	console.log(a); //aqui a está undefined
	// 
	// imagine um monte de códigos aqui
	// 
	var a = "Hoist me!"; //atribuição real da variável
	console.log(a); //aqui o valor já foi atribuído
}

exemplo();
```
> O console retornará undefined no primeiro `console.log` e a string no segundo, porque o valor de `a` foi atribuído.

#### 3.1.2 - Function Hoisting
No caso de funções, as que são definidas com a notação **Function Declaration** recebem o efeito do `hoisting`, que leva ao topo todo seu conteúdo, e, com isso, elas podem ser invocadas mesmo antes de sua declaração. Já com relação àquelas que são definidas a partir da **Function Expression** terão um comportamento semelhante ao das variáveis, não sendo possível invocá-las antes de serem declaradas.

**Function Declaration**
```javascript
/*
(efeito do hoisting)
function soma(a,b){ 
	console.log(a+b);	
}
*/

soma(2,3); //a função está sendo chamada mesmo antes de sua declaração pois sua declaração foi levada ao topo (hoisting)

function soma(a,b){ //declaração real da função
	console.log(a+b);
}
```

**Function Expression**

```javascript
subtracao(8,3);

subtracao = function(a,b){
	console.log(a-b);
}
```
> A saída do console será `subtracao is not defined`. O escopo não consegue reconhecer a função pois seu código não foi levado ao topo, como na Function Declaration.


### 3.2 - Closure
#### Conceito, Importância e Utilização

`Closure` se referece a uma variável, função ou objeto que é retornado do escopo de uma função, mas que mantém a referência deste escopo, mesmo sendo utilizada em outro lugar do código.
As `closures` são importantes para tornar elementos de `escopo local` de funções acessíveis e/ou modificáveis no `escopo global`.
Podemos usar uma `closure` quando temos parâmetros de algum cálculo que são constantes (não podem ser alterados). Então protegemos esses dados por meio do escopo local de uma função e retornamos a parte dos parâmetros que podem ser alterados, neste caso, por meio de uma função também.
```javascript
function fromCelsius(){
	var a = 1.8; // variável local, protegida
	var b = 32; // variável local, protegida
	return function(celsius){ // celsius é a variável que pode ser alterada
		return celsius * a + b;
	};
};

var toFahrenheit = fromCelsius(); // a variável toFahrenheit está recebendo a função que está sendo retornada da função fromCelsius()
toFahrenheit(30); // esse valor se refere ao parâmetro celsius
```
**Saída do console**
```
86 
```
> Então `toFahrenheit` agora é exatamente igual a função retornada de `fromCelsius()`, a Closure garante que as variáveis `a` e `b` mantenham a referência da função principal de onde foram criadas, no caso `fromCelsius()`.

 Sendo assim:
 * celsius = 30; 
 * a = 1.8;
 * b = 32;

> 30*1.8+32 = 86

#### Situação Usual
As funções são elementos essenciais utilizados para proteção de variáveis e rotinas, pois o JavaScript não possui modificadores de visibilidade como private, public e protected. Unir as características das funções com a utilização de closures é uma ótima forma de realizar o encapsulamento.
O exemplo abaixo utiliza outro conceito importante de JavaScript, os [Design Patterns](http://addyosmani.com/resources/essentialjsdesignpatterns/book/), mais especificamente o *Revealing Module Pattern*.
```javascript
var createPessoa = function(nome, idade){
	var nome = nome;
	var idade = idade;
	var getNome = function(){
		console.log(nome);
	};
	var getIdade = function(){
		console.log(idade);
	};
	return { // retornando um objeto
		getNome: getNome, // closure garantindo a referência
		getIdade: getIdade // closure garantindo a referência
	};

};

var p = createPessoa("Igor", 19);
```

```
p;
```
> saída: { getNome: [Function], getIdade: [Function] } // p consegue visualizar apenas os atributos retornados, mas a closure mantém a referência com as variáveis definidas localmente na função `createPessoa`

`p.nome;`
`p.idade;`
> saída: undefined // isto é, o valor "não existe", mas na verdade ele está protegido
 
```
p.getNome();
```
> saída: Igor // por meio da closure a função busca o valor da variável nome que está definida localmente na função `createPessoa`

```
p.getIdade();
```
> saída: 19 // por meio da closure a função busca o valor da variável idade que está definida localmente na função `createPessoa`
 
Com base no exemplo, a `closure` garante que o objeto que foi retornado mantenha um vínculo com as variáveis e funções locais, que estão "protegidas" no escopo local da função `createPessoa`, podendo assim acessá-las pelos métodos, mas não diretamente pelos atributos



### 3.3 - Variável Global
Primeiramente é necessário saber os conceitos de escopo global e local para obter uma melhor compreensão de como utilizar uma variável global dentro de uma função.
### Escopo Global
Qualquer variável que seja criada **fora** de qualquer função é considerada uma **variável de escopo global**. Isso quer dizer que ela é visível e acessível para o programa completo.

#### Escopo Local
Qualquer variável que seja criada **dentro** de uma função com a palavra chave `var` é considerada uma **variável de escopo local**. Isso quer dizer que ela é apenas visível e acessível dentro desta função.
> No entanto, se a palavra chave `var` não for utilizada, ela também será considerada de escopo global, consequentemente tornando-se disponível fora da função também.

#### Variável Local
```javascript
var x = 5; // variável global

function exibeX(){
	var x = 10; // variável local
	console.log("x dentro da função: " + x);
}

exibeX(); 
console.log("x fora da função: " + x);
```
**Saída do console**
```
x dentro da função: 10
x fora da função: 5
```

> Portanto a variável `x` definida fora da função possui escopo global e de dentro da função possui escopo local, simplificando, são duas variável diferentes.

#### Utilizando Variável Global dentro de uma função
Para utilizar uma variável global dentro de uma função é muito simples, basta fazer uma alteração no código acima. Tirando a palavra chave `var` da variável dentro da função ela deixa de ser local e passa a ser global.

```javascript
var x = 5; // variável global

function exibeX(){
	x = 10; // agora também é global e se refere a mesma que foi declarada acima da função
	console.log("x dentro da função: " + x);
}

exibeX(); 
console.log("x fora da função: " + x);
```
**Saída do Console**
```
x dentro da função: 10
x fora da função: 10
```
> Logo, a atribuição de dentro da função alterou a variável global, visto que fez referência a mesma.

### 3.4 - Variável por Parâmetro
Funções em JavaScript podem ou não receber parâmetros, sendo que não há limite para a quantidade parâmetros que podem ser passados para uma função.

Quando variáveis globais são passadas como parâmetros para um função, suas referências não são enviadas juntamentente, tão somente seus valores são enviados como **argumentos**, isso significa que, mesmo se o parâmetro for alterado no interior do escopo da função, as variáveis globais continuarão intactas, com seus valores originais.

Outra forma de se explicar essa questão se dá no fato de que esses valores obtidos por meio das variáveis globais que foram passadas como parâmetros são atribuídos a variáveis locais cujo nome é o nome dos próprios parâmetros definidos na função, o que reafirma a explicação dada acima.

```javascript
var num1 = 10;
var num2 = 20;
var testeParametros = function(a,b){ // entre os parênteses ficam os parâmetros, os valores obtidos das variáveis globais neste caso foram salvos nas variáveis locais 'a' e 'b'
	console.log("Variável local 'a' criada a partir do valor da variável global 'num1' = " + a);
	console.log("Variável local 'b' criada a partir do valor da variável global 'num2' = " + b);
	console.log("Variável global 'num1' = " + num1);
	console.log("Variável global 'num2' = " + num2);
	// alterando o valor das variáveis locais
	a = 55;
	b = 88;
	console.log("Agora o valor da variável local 'a' = " + a);
	console.log("Agora o valor da variável local 'b' = " + b);
};


testeParametros(num1,num2);
console.log("Todavia a variável global 'num1' continua valendo: " + num1);
console.log("E a variável global 'num2' continua valendo: " + num2);
```

**Saída no console**
>```
Variável local 'a' criada a partir do valor da variável global 'num1' = 10
Variável local 'b' criada a partir do valor da variável global 'num2' = 20
Variável global 'num1' = 10
Variável global 'num2' = 20
Agora o valor da variável local 'a' = 55
Agora o valor da variável local 'b' = 88
Todavia a variável global 'num1' continua valendo: 10
E a variável global 'num2' continua valendo: 20
```

#### Particuliaridades do JavaScript
* Pela natureza da linguagem ser dinâmica, funções no Javascript não realizam nenhum tipo validação nos valores de seus parâmetros (argumentos), o que pode gerar resultados indesejados
```javascript
var soma = function(a,b){
	console.log(a+b);
};

var x = "era pra ser ";
var y = "número :/";
soma(x,y);
```
> Saída: era pra ser número :/

* As funções no Javascript não verificam a quantidade de parâmetros passados, sendo que:
	* se a quantidade de parâmetros for **menor** do que a exigida pela função, os outros parâmetros são definidos, automaticamente, como `undefined`
	```javascript
	var x = 5;
	var soma = function(a,b){
		console.log(a+b);
	};

	soma(x);
	```
	> Saída: NaN // pois não há como somar um undefined

	* se a quantidade de parâmetros for **maior** do que a exigida pela função, os parâmetros sobressalentes são, automaticamente, ignorados.
	```javascript
	var x = 5;
	var y = 7;
	var z = 2;
	var soma = function(a,b){
		console.log(a+b);
	};

	soma(x,y,z);
	```
	> Saída: 12 // o último parâmetro foi ignorado

* As funções no Javascript possuem um objeto chamado `arguments` por padrão. Ele contém um array que armazena todos os argumentos que foram passados como parâmetro quando uma função é invocada.
```javascript
var x = 5;
var y = 7;
var z = 2;
var soma = function(a,b){
	console.log(a+b);
	console.log(arguments); 
};

soma(x,y,z);
```
> Saída: 
```
12
```
```
{ '0': 5, '1': 7, '2': 2 } // array de argumentos onde o primeiro número representa o índice
```

 **Observação:** Argumentos são passados por valor enquanto que objetos são passados por referência. Ou seja, mudanças no argumentos só serão visualizadas dentro do escopo da função enquanto que mudanças realizadas nos objetos serão visualizadas fora do escopo da função.


### 3.5 - Instanciação usando uma IIFE
Uma `IIFE` (Immediately Invoked Function Expressions) é uma função que, como o próprio nome sugere, é invocada assim que é definida. Não há muitas diferenças na declaração de uma IIFE se comparada à uma função normal, entretanto quem não está habituado com a sintaxe, à primeira vista achará uma coisa totalmente estranha e doida. 

Para declarar uma IIFE o **primeiro passo** é colocar a função dentro de uma expressão, ou seja, entre parênteses.
#### Função normal
```javascript
function(){
	// código
};
```

#### Função dentro de uma expressão
```javascript
(function(){
	// código
});
```

Ok, agora basta invocar a função que está dentro da expressão, como se faz a invocação de qualquer outra função, com `()`, esses, por sua vez, vão logo após o fechamento da função `}`, então assim que a função é definida, é automaticamente invocada.
```javascript
(function(){
	// código
}());
```

Um recurso muito poderoso do JavaScript é poder atribuir uma IIFE à uma variável, esta então carregará esse bloco de código autoexecutável.
É muito simples fazer isso, basta usar a notação para declaração de função *Function Expression*.
```javascript
var autoExec = (function(){
	// código
}());
```

A utilização de IIFEs é uma ótima forma de executar uma tarefa, cuja possui variáveis que só serão utilizadas para ela.Isso acontece porque as variáveis que são definidas dentro de uma IIFE ficam definidas localmente e não poluem o escopo global, são variáveis temporárias que apenas são utilizadas quando a IIFE é invocada.

No exemplo abaixo uma IIFE é utilizada para trocar o valor das variáveis globais a e b, o que requere uma variável temporária - representada por `temp` no exemplo - que só existe quando a IIFE é invocada.

```javascript
a = 1;
b = 99;

var autoExec = (function(){
	var temp = a;
	a = b;
	b = temp;
}());

autoExec; // variável que contém a IIFE, somente chamando-a a IIFE já é executada
console.log("Valor de a = " + a);
console.log("Valor de b = " + b);
console.log(temp); 
```
> Saída: 
```
Valor de a = 99
```
```
Valor de b = 1
```
```
temp is not defined
```

#### Passando uma variável por parâmetro para uma IIFE
Quando passamos uma variável por parâmetro para uma **IIFE** o comportamento é exatamente igual como quando passamos um variável como parâmetro para uma **função normal**.

O parâmetro pega o **valor** e não a referência da variável que foi passada e atribui o mesmo à uma variável **local**, sendo assim, não altera o valor da variável que foi passada.

A única diferença se dá na forma de passagem de parâmetros, como a IIFE é invocada assim que sua declaração é finalizada temos de passar os parâmetros já em sua declaração. Como o parênteses que vem depois do fechamento da função `}()` é o responsável por realizar essa invocação imediata, os parâmetros são passados por meio deles.
```javascript
(function(){
	//
	// code
	//
}(parametroA, parametroB));
```

#### Exemplo
```javascript
var n = 0; // Variável global n

var counter = (function(n){ // este n se refere a var local que receberá o valor obtido de n global
	return function(){ // a IIFE está retornando uma função
		n += 1; // alterando o valor de n
		console.log("Valor da variável 'n' local da IIFE = " + n);
	};
}(n)); // passando a var n global como parâmetro
```
> Como a IIFE está retornando uma função, a variável 'counter', que está recebendo o retorno dessa IIFE passa a ser uma função também, e, consequentemente, deverá usar () para invocar essa função retornada.

```
console.log(counter); // Confirmando que agora counter é uma função
counter();
console.log("Valor da variável global 'n' = " + n);
counter();
console.log("Valor da variável global 'n' = " + n);
```
> Saída: 
```
[Function]
Valor da variável 'n' local da IIFE = 1
Valor da variável global 'n' = 0
Valor da variável 'n' local da IIFE = 2
Valor da variável global 'n' = 0
```

## 4 - Conclusão
Os temas que foram tratados neste artigo são de suma importância e devem ser dominados para trabalhar da maneira correta e ter mais produtividade utilizando JavaScript.

## 5 - Referências
Jones, Darren, **E-book JavaScript: Novice to Ninja**. Publicado em jan.2015. Disponível para aquisição em [https://www.sitepoint.com/premium/books/javascript-novice-to-ninja](https://www.sitepoint.com/premium/books/javascript-novice-to-ninja).

Freeman, Eric & Robson, Elisabeth, **E-book Head First JavaScript Programming**. Publicado em mar.2014. O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472. Disponível para aquisição em [http://shop.oreilly.com/product/0636920027065.do](http://shop.oreilly.com/product/0636920027065.do)

MDN(Mozilla Developer Network), **JavaScript Guide**. Última atualização em 21 out.2015. Disponível em [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide). Último acesso em: 17 nov.2015.

MSDN (Microsoft Developer Network), **JavaScript Language Reference**. Disponível em [https://msdn.microsoft.com/en-us/library/d1et7k7c(v=vs.94).aspx](https://msdn.microsoft.com/en-us/library/d1et7k7c(v=vs.94).aspx). Último acesso em: 17 nov.2015.

w3schools.com, **JavaScript Tutorial**. Disponível em [http://www.w3schools.com/js/default.asp](http://www.w3schools.com/js/default.asp). Último acesso em: 17 nov.2015.











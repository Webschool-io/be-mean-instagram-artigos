# Artigos - Herança na JavaScript

**Autor**: Leonardo Nascimento de Oliveira- [leonascimento](https://github.com/leonascimento)

**Data**: 1450918820838


> Aprender sem pensar é esforço em vão; pensar sem nada aprender é nocivo - **(Confúcio)**

Antes de ler esse artigo é aconselhável que você leia sobre objetos, classes e o uso do this em JavaScript. Tentarei explicar rapido antes de entrar em detalhes sobre herança.

### Entendendo o conceito de Classes

![](http://mulheretc.com.br/wp-content/uploads/2015/11/F%C3%A1brica-de-Bolo-c_6723.jpg)

Pensa numa forma de bolo, você é dono de uma fábrica de bolos e cupcakes. E cada forma tem um tamanho específico, tem um profundidade ou diâmetro padrão. 

Essa forma é a base para você criar "trocentos" bolos; Cada **bolo é um objeto** que foi criado/modelado com as especificações da forma em si, e a **forma é classe** pois ela serviu como base. Com cada forma podemos criar vários bolos ela serve como um molde.

![](http://www.permanentstyle.co.uk/wp-content/uploads/2014/07/A-Caraceni-.jpg)

Um outro exemplo fácil, é pensar em um Alfaiate, quando você vai encomendar por exemplo um terno, ele tira medidas do cliente para a partir delas traçar os moldes da peça que você encomendou. Logo em seguida ele faz o corte dos tecidos baseado no molde. O molde é a classe, e cada peça de tecido cortada é o objeto.

> Lembre-se que em Orientação a Objetos podemos criar (instanciar) vários objetos a partir de uma classe base.

## Estrutura de Uma classe

* Classe
  * Nome da Classe
  * Atributos
  * Métodos

**Atributo:** Também conhecido como propriedade de uma classe, é composto pelo nome do atributo e o valor dele.

**Método:** Um objeto criado a partir de uma (instanciado) por uma Super Classe pode possuir comportamentos, esses comportamentos são edscritos através de métodos. Um método nada mais é o equivalete a um procedimento ou função. Ele pode manipular variáveis locais, atributos da classe.

### Representação de Classes

![](http://www.dca.fee.unicamp.br/cursos/PooJava/classes/uml_class.gif)

![](http://www.programiz.com/sites/tutorial2program/files/C%2B%2B-class-containing-data-function.jpg)

![](http://www.bennadel.com/resources/uploads/prototype_shared_by_all_sub_class_instances.gif)

## Visibilidade de Atributos

Em linguagems clássicas que são Orientadas a objetos ou possuem esse paradrigma como PHP ou Java dentre outras, você define a **visibilidade** dos seus atributos e métodos, que são `private`, `public`, quando definimos como private apenas estamos restrigindo de qualquer pessoa quando instanciar um objeto tenha acesso direto a esse atributo ou método. 

Outra coisa que vimos na imagem anterior de represetação de classes foi o tipo de dado que vai ser passado. Em java quando definimos um atributos precisamos também definir o tipo dele. Usando JavaScript sentimos uma maior liberdade, pois não precisamos especificar esses detalhes, isso pelo fato da JavaScript ser uma linguagem de programação com tipagem fraca, ou seja, sem tanta frescura e aquele porrilhão de regras. 

## Classes em JavaScript

Antes da Versão da ECMA6 "emulamos" uma classe atráves do uso de uma função. Então criamos uma função construtora que é utilizada com o objetivo de inicializar (criar) novos bojetos, e você faz o uso da palavra `new` para chamar o construtor.

```JS
// 1. Função construtora
function Person (name, age) {
  
  // 2. Propriedades da Função construtora Person
  this._name = name;
  this._age  = age;

  // 3. Método da Função construtora Person
  this.hello = function(){
    console.log("My name is " + this._name + "and I'm "+ this._age +" years old");
  };

};

// Utilizando new e chamado o construtor Person para criar um objeto chaves.
var chaves = new Person('Chaves','65');

// Invoca o método ola
chaves.hello(); // My name is Chavesand I'm 66 years old
```

Note que ao definir as propriedades da classe Person e o métodos, nós estamos usando a palavra reservada `this` essa palavra referencia o objeto que está sendo instanciado. Desta forma o objeto foi criado a partir de uma classe construtora, use ele com cuidado pois devemos fazer o uso consciente pois ele possui algumas armadilhas, em alguns casos se usado de forma errada ele pode referencia o objeto global window.

> **OBSERVAÇÃO 1**: Na JavaScript existe outras forma de definir classes como usando ES6, abordaremos apenas em outro artigo.

> **OBSERVAÇÃO 2**: Note que criamos o méto do `this.hello` internamente dentro da classe, isso é um má prática pois se tivermos 1000 objetos diferentes instanciadas vai exisitr 1000 métodos this.hello na memória. A forma correta seria anexar esse método em um `prototype` o que vamos ver logo em seguida.

## Como funciona a Herança na JavaScript

Pessoas que estam bem acostumadas com herança em outras linguagens quando caem de paraquedas na JavaScript desistem ou ficam confusos pelo fato das versões anteriores a ES6 não possuir herança clássica. 

Então a única forma de implementar a herança na JavaScript é "emular" classes fazendo o uso herança baseada em cadeia de `prototypes` ou de `outras técnicas` que vamos ver adiante.

## Definições de Herança

> Herança em linguagens de programação pode se compreendida como um mecanismo que permite que características de uma **classe base** (também conhecida como superclasse) possam ser herdadas por outras classes, que são criadas a partir da **classe base**. Uma classe criada a partir da classe base pode ser chamada de **classe derivada** ou subclasse, ela apresenta as características que são (estrutura e métodos) da classe base, podendo acrescentar novos métodos e propriedades particulares dela.

> Herança em OOP é a capacidade de classes compartilharem atributos e métodos entre si. Geralmente a herança é usada para compartilhar comportamentos generalizados e comuns entres as classes filhas.

### Prototype

Cada função possui uma propriedade `prototype` (protótipo) vazia por default que pode ser modificada ou reescrita.

Pensa como se cada função tem uma area alocada para ela. Então nessa area podemos anexar propriedades e métodos ao prototype daquela função, principalmente quando nosso objetivo é implementar a herança.

* Podemos pensar que o prototype é uma características do objeto

* Um atributo prototype aponta para o objeto pai, o objeto do qual foram herdadas as propriedades.

**PRINCIPAL USO**
O principal uso do `prototype` é para herança: adicionamos métodos e propriedades dentro desse `prototype` pertencente a tal função, então num momento quando criamos um objeto (instanciamos) a partir desta função, essas propriedades e métodos que estam no `prototype` da função ficarão disponíveis para o objeto utilizá-lo.

**É IMPORTANTE LEMBRAR**

* O atributo prototype é referenciado como objeto prototype
* Ele é configurado automaticamente quando criamos um novo objeto.

> OBS: Todos objeto têm atributos como propriedades de objetos. E os atributos dos objetos são: prototype, constructor etc.

### Exemplo de herança usando `prototype`

```JS
// 1. Função construtora Foo
function Foo(someValue) {
  
  // 2. Propriedade anexada diretamente na função Foo, ela não está no prototype mas os métodos do prototype
  //    podem acessar
  this.value = someValue;

};

// 3. Método printValue anexado no prototype da função construtora Foo
Foo.prototype.printValue = function(){
  console.log(this.value);
};

// 4. Intanciando (criando objeto a partir da função construtora Foo);
var someObject = new Foo("Eu sou um valor");

// 5. Invocando o método printValue a partir do objeto somObjet. Isso foi possível pq o objeto tem 
//    acesso a tudo que é anexado no prototype. 
someObject.printValue(); // Eu sou um valor
```
### Implementando a Herança de Protótipos (prototypes)

Até agora perceba que não vimos nada sobre herança, agora sim que sabemos criar um classe (função construtora), criar um objeto a partir dela, e sabemos também o que é o prototype podemos pôr a mão na massa e implementar a herança.
É a forma mais comum de implementar herança.

```JS

// 1. Criando uma classe carro
function Car () {

  this.qtdRodas = 4;

};

// 2. Método que imprime todas as informações sobre o carro
Car.prototype.showInforCar = function () {

  console.log("Model: "    + this.model    ); 
  console.log("Color: "    + this.color    );
  console.log("Price: "    + this.price    );
  console.log("QtdRodas: " + this.qtdRodas );

};

// 3 Adicionando o método amIComplete a propriedade prototype de Car
Car.prototype.amIComplete = function () {

  if (this.isOrganic) {
    console.log("I am a complete Car!");
  }

};

// 4. Agora queremos criar uma classe construtora generalMotors
function GeneralMotors (model, color, price) {

  this.model     = model;
  this.color     = color;
  this.price     = price;
  this.montadora = "General Motors";

}

// Até aqui temos duas funções construtoras, veja que [ Car ] tem dois métodos anexados no prototype
// E a função construtora [ generalMotors ] não consegue acessar as propriedades de [Car] ou métodos

// 5. Devemos configurar o protótipo da função [ generalMotors ] para o construtor [ Car ]
// Agora ele passa a herdar todos os métodos e propriedades de [ Car.prototype ]
GeneralMotors.prototype = new Car();

// 6. Agora podemos criar vários carros da marca General Motors
// Cria um novo objeto, aBanana, com o construtor Fruit
var corsa = new GeneralMotors("Corsa Sedan", "Gray", 25.000 );

// 7. Imprime a cor
console.log(corsa.color); // Gray

// 8. Aqui, o objeto (corsa) usa o método showInforCar do prototype da classe Car 
console.log(corsa.showInforCar()); 
// GrayModel: Corsa Sedan
// Color: Gray
// Price: 25
// QtdRodas: 4

console.dir(corsa);
// GeneralMotors
//  color: "Gray"
//  model: "Corsa Sedan"
//  montadora: "General Motors"
//  price: 25
//    __proto__: Car
//      qtdRodas: 4
//      __proto__: Car
//        amIComplete: ()
//        constructor: Car()
//        showInforCar: ()
//        __proto__: Object
```

### CONSTRUCTOR STEALING / CLASSICAL INHERITANCE

Essa é uma outra técnica utilizada por muitos devs que na tentativa de resolver o problema de herança com 
os valores de referência em protótipos, os desenvolvedores 
começaram a usar uma técnica chamada de Constructor Stealing (também chamado às vezes object masquerading ou 
Classical Inheritance).

A idéia básica é bastante simples: Chamar o construtor **Pessoa** a partir do construtor **PessoaFisica**. 
Tendo em mente que as funções são simplesmente objetos que  executam código em um contexto particular, 
apply() e call() são métodos podem ser usados ​​para executar um construtor no objeto 
recém-criado, como neste exemplo:

```JS
// 1. Função construtora Pessoa
function Person (name, phone) {

  // 2. Definindo propriedades 
  this.data  = ["Cor: branca", "idade:22", "Função: Programador(a)"];
  this.name  = name;
  this.phone = phone;

}

// 3. Função construtora Pessoa física,
function FisicPerson( name, phone, cpf ) {

  // 4. Precisamos herdar todas as propriedades e métodos da função construtora Pessoa
  // pra que isso possa acontecer temos que usar call que vai fazer com que cada instância 
  // note que passamos o this que nada menos tem o papel de ir lá em pessoa e fazer com que todos
  // os this em pessoas apontem para PessoaFisica, ou seja o call passando o this rouba tudo que tem lá
  // e redefine para PessoaFisica fazendo com que tenha sua própria cópia da propriedade dados
  // DICA: Sempre passe o this como primeiro parametro
  Person.call( this, name, phone);
  this.cpf = cpf;

}

// 5. Uma simples função que imprime o cpf 
FisicPerson.prototype.showCPF = function(){
  console.log(this.cpf);
};

// 6. Instanciando uma Pessoa Fisica
var keka = new FisicPerson("Jéssica","848888-9999","065.332.452-32");

// 7. Inserindo dados na propriedade data
keka.data.push("Nível: Júnior");
console.log(keka.data); //"Nivel: Júnior","Cor: branca", "idade:22", "Programadora";
keka.showCPF();         // 065.332.452-32

// 8. Instancia de outra Pesso Fśica
var leo = new FisicPerson("leo","843322-5569", "062.458.625-51"); 
console.log(leo.data); //["Cor: branca", "idade:22", "Programador"]
console.log(leo.phone); // 843322-5569
```

Note que a sacada neste caso foi fazer o uso do call passamos o this e neste caso, todas as
propriedades definidas em Pessoa agora cada Instância de pessoa física tem uma cópia própria delas.
O nosso problema é que Person possui algumas que FisicPerson necessita mas precisa implementar mais
outros então roubamos as propriedades da outra.

### Parasitic Combination Inheritanc 

Combinação de Herança as vezes também chamada de PSEUDO CLASSICAL INHERITANCE. Combina encadeamento de 
protótipos e roubo de construtor para obter o melhor de cada padrão. A idéia básica é usar o encadeamento de 
protótipo e herdar propriedades e métodos no protótipo, e usar construtor Stealing herdar propriedades da 
instância. Isso permite que a reutilização da função através da definição de métodos no protótipo e permite 
que cada instância de ter suas próprias propriedades. Considere o seguinte:

```JS
// 1. Defina classe superType
function SuperType(name) {

  // 2. Defina propriedades
  this.name   = name;
  this.colors = ['red', 'blue', 'green'];

}

// 3. Defina um método no prototype de SuperTyp, essa classe que queremos que queremos esse método
SuperType.prototype.sayName = function() {

  console.log(this.name);

};

// 4. Definindo outra classe que herda os métodos e propriedades da clase SuperType
function SubType(name, age){

  // 5. Isso faz herda propriedades
  SuperType.call(this, name);

  // 6. Definindo uma nova propriedade além das herdadas
  this.age = age;

}

// 6. Agora a mágica fazemos SubType herdar os métodos de SuperType
SubType.prototype = new SuperType();

// 7. Um simples método de SuperType
SubType.prototype.sayAge = function() {

  console.log(this.age);

};

// 8. Criando um objeto instance 1
var instance1 = new SubType('Gregory', 29);
instance1.colors.push('black');
console.log(instance1.colors); //'red,blue,green,black'

instance1.sayName(); //'Gregory';
instance1.sayAge(); //29

var instance2 = new SubType('Dan', 27);
//alert(instance2.colors); //'red,blue,green'
instance2.sayName(); //'Dan';
instance2.sayAge(); //27
```

Neste exemplo acima, o construtor supertipo define duas propriedades, nome e cores, e o protótipo supertipo 
tem um único método chamado sayName (). O construtor subtipo chama o construtor supertipo, passando o 
argumento de nome, e define sua própria propriedade chamada idade. Além disso, o protótipo de subtipo 
é designado para ser um exemplo de supertipo, e, em seguida, um novo método denominado sayAge () é 
definido. Com este código, ele é então possível criar duas instâncias separadas do subtipo que têm as suas 
próprias propriedades, incluindo o as cores, mas todos usam os mesmos métodos. Dirigindo-se as desvantagens 
de ambos encadeamentos de protótipos e roubo de construtor (constructor stearling), (Combinação de herança) é 
o padrão mais utilizado em herança no JavaScript. E também preserva o comportamento de instanceof 
e isPrototypeOf () para identificar a composição de objectos.

### Functional Inheritance

Ao invés de criar uma função construtora sem precisar fazermos o uso da palavra `new` para criar um objeto,
pensa se nós retornarmos um objeto puro. 

Na JavaScript podemos criar um objeto em notação literal ex:

```JS
var object = {
  propriedade1: "valor1",
  propriedade2: "valor2",
  metodo1     : function() {
    console.log("Eu sou um simples método");
  },
  método2     : function(){
    console.log("eu tenho a função de imprimir a propriedade1: " + this.propriedade1 );
  }
};

console.log(objetc.propriedade1);
console.log(objetc.propriedade2);
console.log(objetc.metodo1);
console.log(objetc.método2);
```

Pensa então, como é possível implementar herança sem estar fazendo o uso dos prototypes. Mas como? utilizando
funções e o melhor não precisamos fazer o uso da palavra chave `new` para criar objetos.


**EXEMPLO: 01**
```JS


//1. Cria Função Construtora que retorna obeto base

function base(spec) {

  var that  = {};        // 2. Criamos um objeto literal vazio
  that.name = spec.name; // 3. Adicionamos a propriedade name
  return that;           // 4. Retornamos o objeto

}

// 5. Crie o Construtor de "Child", herda tudo do construtor "base"
function child(spec) {    

  var that = base(spec);        // 6. Cria o objeto através do construtor "base"
  
  that.sayHello = function() {  // 7.Método do objeto that.
    return 'Hello, I\'m ' + that.name;
  };

  return that;                  // 8.retorna that

}

// instanciando
var object = child({ name: 'a functional object' });

// Acessando método
console.log(object.sayHello()); // "Hello, I'm a functional object"
console.log(object.name);       // "a functional object"
```

> Nota: Cuidado quando você usar Funciona Inheritance não use o this, pois ele irá apontar para o objeto global `window`.

### Extra 

Apenas para nível de conhecimento aqui segue as ligações de protótipos e propriedades disponíveis nos
Objetos e na ligação de `prototypes`.

* **OBJECT LITERAL**
  * Construção - atravéz de chaves `{  }`
  * Ligação (é criado com base em) - `Object.prototype`
  * Propriedade Prototype - não possui

* **FUNCTION OBJECT**
  * Construção - através da palavra-chave `function`
  * Ligação (é criado com base em) - `Function.prototype`
  * Propriedade Prototype - Possui

* **OBJECT FROM FUNCTION**
  * Construção - via palavra-chave `new` com função de objeto
  * Ligação (é criado com base em) - Seja qual for a função de (que foi invocada por `new` ) propriedade protótipo está definido para ( `Object.prototype` por padrão)
  * Protótipo de propriedade - não existe  

* **[ HERANÇA COM CONSTRUTORES ]**
  * A forma mas padronizada  e bem conhecida e cross-browser para a  criação de objetos utilizando funções que agem como construtores. Lembrando que para utilizar um função construtora no JavaScript temos que utilziar o (`new`).
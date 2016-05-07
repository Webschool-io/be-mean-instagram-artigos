# Artigo - Herança
**Autor:** Gabriel Kalani
**Data**: 1454707555973

### Herança em JavaScript
JavaScript utiliza uma abordagem diferente para compartilhar código entre entidades, chamada de orientação a protótipo.
Nessa linguagem (quase) tudo é um objeto. Até mesmo as funções são objetos...<br>
No JavaScript, a herança se dá por meio de uma referência interna chamada prototype, que todos os objetos possuem.
Nesse caso do JavaScript, isso é particularmente útil para métodos.

Abaixo coloquei um exemplo básico.

```JS
function Suissa(){}
 var Suissa = new Suissa();
 
 function Suissa(){}
Suissa.prototype.cuidarDeGatinhosFofinhos = function(){
 return true;
}
 var Suissa = new Suissa();
 
console.log(Suissa.cuidarDeGatinhosFofinhos());
```

Acima podemos ver um exemplo bem bacana hahah.<br>
Criei uma classe denominada de `Suissa` que no começo estava vazia e depois disso havia adicionado um método `cuidarDeGatinhosFofinhos`.<br>
Uma coisa bem interesante sobre um protótipo, é que ele nos permite predefinir propriedades, incluindo métodos.

JavaScript tem somente um construtor: - Quando se trata de heranças - Objetos.<br>
Em JavaScript, qualquer função pode ser adicionada em um objeto em forma de propriedade. Uma herança de funções age como a herança de quaisquers outras propriedades que não sejam funções, e podemos inclusive realizar sobre-escrita de função!<br>

Veja abaixo mais um exemplo:<br>

```JS
function user(){}
 var user = new user();
 
 function user(){}
user.prototype.minerarBitcoins = function(){
 return true;
}
 var user = new user();
 
console.log(user.cuidarDeGatinhosFofinhos());
```

No exemplo acima vemos que a classe `user` possui um método que é `minerarBitcoins`.

Em Javascript existe a possibilidade de se programar orientado a objetos, embora este estilo de programação nesta linguagem seja um pouco diferente, e acabe causando espanto nos iniciantes. Principalmente para quem é acostumado com linguagens como Java, o estilo de código orientado a objetos em Javascript é muitíssimo diferente.<br>

## Mais um exemplo
```JS
var aluno = {
// Características do objeto `aluno`
  resolverTodosOsArtigos: function() {
    
  },
  dormir: function() {
    
  },
  comer: function() {
    
  },
  jogarCsGo: function() {
    
  },
  desistir: function() {
    
  }
}

var Gabriel = Object.create(aluno)
Gabriel.resolverTodosOsArtigos()
```

Bom, acima vemos que eu defini um objeto chamado `aluno` e também depois havia criado uma variável cujo valor é mostrar as características do aluno. (Minha realidade agora rsrs)

## Compatibilidade nos navegadores.
[!Compatibilidade]()<br>
O ES5 - ECMAScript 5 - funciona parcialmente em versões do IE da 9 para as mais atuais. Na versão 9, funciona parcialmente.
Para quem prefere um Graceful Degradation.
Leia o artigo abaixo para que estes novos métodos funcionem em navegadores antigos.<br>
- [ES5-Shim](https://github.com/kriskowal/es5-shim)

## Conclusão
Como vimos as heranças são varias e elas são baseadas em protótipos.

Vamos dar um ultimo exemplo:
```JS
function Usuario(user,email,senha,ultimoAcesso) { 
    var user = gkal19;
    var email = gabrielsilva1956@gmail.com;
    var senha = senha;
    var ultimoAcesso = Date.now();

    this.getUser = function () { 
        return user; 
    }; 
    
    this.getEmail = function () { 
        return email; 
    }; 

    this.getSenha = function () { 
        return senha; 
    };

    this.getUltimoAcesso = function () { 
        return ultimoAcesso; 
    };
}
```

## Instâncias
O que são as instâncias? Quando você cria um objeto ou seja, a instanciação de uma classe é realizada com uso do operador new.<br>
Após este operador vem o nome da função construtora, responsável pela inicialização do objeto.<br>
```JS
var obj = new Object();
var data = new Date();
```

## Literais de Objetos
Com os `Literais de Objetos` podemos criar e iniciar objetos de uma maneira diferente. A sintaxe é definida por uma lista de pares nome/valor separados por vírgulas entre um par de chaves. Cada par nome/valor é definido pelo nome da propriedade seguido de dois pontos e do valor correspondente.<br>
```JS
var Aula = {
    titulo : "BeMEAN Instagram",
    autor : "Jean Carlo Nascimento",
    modulo1 : {
        titulo : "MongoDb",
        aulas : 7
    },
    modulo2 : {
        titulo : "Node.JS",
        aulas : 21
    }
}

// Acessando as propriedades:
console.log(Aula.titulo + " - " + Aula.autor + "\\n\\t" +
      Aula.modulo1.titulo + " - " +
      Aula.modulo1.aulas + " Aulas\\n\\t" +
      Aula.modulo2.titulo + " - " +
      Aula.modulo2.aulas + " Aulas");
```

## Objetos Construtores
Um dos meios mais comuns de se criar um objeto é com o construtor `Object`.Ele é uma função usada para inicializar novos objetos, e você usa a palavra-chave `new` para chamar o construtor.
```JS
var artigo = new Object();
artigo.quaoDificilEuSou = function () {
    console.log("Dificil pra poarr!!");
}
```


ATENÇÃO: Objetos podem conter qualquer outro tipo de dados, incluindo Numbers, Arrays e também outros objetos.<br>


## Conhecendo o `__proto__`

```JS

var animal = {
			come: true
			}

var gato = {
			fazParkour: true
}

gato.__proto__ = animal

alert(gato.come) // true

```

O ``__proto__` é uma propriedade de acessoque expõe o interno [[Prototype]] do objeto através do qual é acessado.<br>
A propriedade __proto__ poderá também em uma definição literal de objeto para definir o objeto [[Prototype]] na criação, como uma alternativa ao Object.create().<br>

## Herança Multipla
Em linguagens de orientação a objetos, Herana Multipla é o conceito de herança de duas ou mais classes.

```JS
function Chocolate(){
    this.nome = 'Kinder Ovo';
}
function Produto(){
    this.valor = 'R$3,00';
}
 
Chocolate.prototype = new Produto;
Chocolate.prototype.imprimir = function(){
    return this.valor + ' - ' + this.nome;
	
// Sim, dependendo da loja aqui em Belém. O Kinder Ovo está quase 3 reais kjkjkjkk	

```

Javascript não suporta múltipla herança pelo método de prototype que é o método que define o tipo do objeto, existe entretanto como contornar isso, um exemplo é a biblioteca zInherit (http://www.nczonline.net/downloads/) de Nicholas C. Zakas (http://www.nczonline.net/writing/), autor dos excelentes livros “Professional Ajax” e “Professional JavaScript for Web Developers”.

## Heranças via Call e Apply
```JS

function theFunction(name, sou) {
    console.log("Meu nome é " + name + " e eu sou " + sou + ".");
}

theFunction("Gabriel", "aluno");
theFunction.apply(undefined, ["nãoSei", "Esse cara"]);
theFunction.call(undefined, "Batman", "Fodão");
```

A diferença deles, é que o `apply` permite invocar a função com argumentos como uma matriz ; a `call` requer os parâmetros ser listados explicitamente.<br>
Só lembrando também que a herança que utiliza `call `e `apply` funcionam para itens públicos. Os itens privados precisam de algum método publico para uma chamada indireta.<br>
Um exemplo básico abaixo:<br>
```JS
obj = function(){
	var nome= Gabriel;
		this.getNome=function(){
	return nome;
	}
}
```

## Heranças via Object Masquerading
É uma estratégia que emula a herança, um construtor associa todas as propriedades e métodos (com o paradigma de declaração de Construtor) usando a keyword “this” para o contexto interno do qual foi referenciado.<br>
Porque o construtor é como uma função, voce pode montar o construtor de um objeto “A” dentro de um método de um objeto “B” e chamá-lo. O objeto “B” então recebe as propriedades e métodos definidos no construtor de “A” porque o this agora aponta para o contexto do novo objeto.<br>
```JS
// Objeto que servirá de base
function A(sColor) {
    this.color = sColor;
    this.sayColor = function () {
        alert(this.color);
    };
}

// Objeto "B" que herdará de "A"
function B(sColor, sName) {
    this.newMethod = A;
    this.newMethod(sColor);
    delete this.newMethod;
    this.name = sName;
    this.sayName = function () {
        alert(this.name);
    };
}

var objA = new ClassA('red');
var objB = new ClassB('blue', 'Gabriel');
objA.sayColor(); //outputs 'red'
objB.sayColor(); //outputs 'blue'
objB.sayName(); //outputs 'Gabriel'
alert(objB instanceof A ); //false
alert(objB instanceof B ); //true

```

Temos as mesmas chamadas de A em B com emulação completa da herança.O Object Masquerading suporta a implementação de múltipla herança.<br>

## Keyword This
A palavra chave `this` faz referência ao escopo do objeto onde ela está sendo chamada.<br>

```JS
function pessoa(first, last, age, eye) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eye;
}
var me = new pessoa("Gabriel", "Kalani", 14, "black");
var meuPai = new pessoa("Givaldo", "Oliveira", 38, "brown");
var minhaMãe = new pessoa("Cristiane", "Nunes", 31, "black");
var minhaIrmã = new pessoa("Victória", "Cristiane", 9, "brown");
```

Dei muitas mudanças e também e esse artigo em mais de 200 linhas.
Saibam o porquê na imagem abaixo.<br>

![Conversa com Suissa](http://i.imgur.com/koyTZzd.png)


Então...<br>
Este artigo é uma introdução á Heranças no JavaScript, espero que desta vez esse artigo seja aceito.<br>
Era só isso mesmo, Obrigado por ler meu artigo.

# Artigo - Herança

**Autor**: Filipe Leuch Bonfim - [filipe1309](https://github.com/filipe1309)<br>
**Data**: 1455582852937

## O que é herança mesmo?
> “É um princípio de orientação a objetos, que permite que classes compartilhem atributos e métodos,” [Wikipedia] (https://pt.wikipedia.org/wiki/Heran%C3%A7a_%28programa%C3%A7%C3%A3o%29)

*Herança* é um conceito base de orientação a objetos, que serve para evitar a repetição de código, além de deixá-lo mais limpo, possibilitando também a especialização das classes.

## Ok, mas como a herança funciona no Javascript
O funcionamento ocorre de forma um pouco diferente do que em orientação a objetos, pois javascript é uma linguagem orientada a protótipos. que não possui classes, e a herança ocorre através de um processo de clonagem de objetos existentes que são utilizados como protótipos dos novos objetos que serão criados.

## Pera, pera!!! tipo, o que são protótipos mesmo!?
Praticamente todos os objetos no javascript possuem um protótipo, e neste estão contidas outros atributos e métodos, além dos já definidos no próprio objeto.

Por exemplo:
Ao criarmos um objeto, este terá como protótipo o objeto ``Object``, que tem `null` como protótipo.
```js
var objeto = {}
objeto.__proto__ // Object
// objeto → Object → null
```

Ao criarmos um Array, este terá como protótipo o objeto  ``Array``, que tem como protótipo o objeto ``Object``.
```js
var arr = []
arr.__proto__ // Array
// arr → Array → Object → null
```

Ao criarmos uma Função, esta terá como protótipo o objeto  ``Function``, que tem como protótipo o objeto ``Object``.
```js
var func = function() {}
func.__proto__ // function
// func → function → Object → null
```

## Hum.. e como eu implemento isto?
Existem diversas formas de se implementar herança em Javascript, destacarei neste artigo as três formas (Prototype-chaining , Parasitic Combination e Functional) mais difundidas de se fazer isto.

#### Prototype-chaining Inheritance (Herança com o encadeamento de protótipos)
Esta é a maneira mais comum de se implementar herança em javascript, e utilizando-a é possível obter ganho de performance e diminuição do uso de memória, mas o código acaba por ficar mais poluído.

Desta maneira, utiliza-se o construtor do objeto a ser herdado como protótipo do novo objeto.

Primeiro é declarado o objeto a ser herdado, o “objeto Pai”, com seus atributos e métodos.
```js
var Veiculo = function(modelo, ano) {
     this.modelo =  modelo;
     this.ano= ano;
}

Veiculo.prototype.acelerar = function(){
    console.log("Celera, celera!!!!");
}

Veiculo.prototype.buzina = function(){
    console.log("Biii Biii!!!!");
}

```

Depois, é declarado, o novo objeto, ou “objeto Filho”, que herdará do “objeto Pai”
```js
var Carro = function(modelo, ano, qtd_portas){
    Veiculo.call(this, modelo, ano);
    this.qtd_portas = qtd_portas;
};
```

É feita então a relação entre os objetos através da propriedade ``prototype``. Nesta etapa, percebe-se uma má pratica da utilização deste método, pois instancia-se o objeto Veiculo, sem argumentos, para que seja atribuído ao protótipo de Carro.
```js
Carro.prototype = new Veiculo();
Carro.prototype.constructor = Carro;
```

E por fim, os métodos do objeto filho são declarados
```js
Carro.prototype.ligaTurbo = function(){
    console.log(“Tzuzzzzzzzzzzzz!!!!”);
};
```

E ao instanciarmos um “objeto Filho” temos as suas propriedades, além daquelas contidas no “objeto Pai”
```js
var ka = new Carro("ka", "2016", 2);
ka.buzina(); // Biii Biii!!!!
ka.ligaTurbo(); // Tzuzzzzzzzzzzzz
```

#### Parasitic Combination Inheritance (Herança com a combinação parasita)
Desta maneira, simplesmente efetuamos uma cópia do “objeto Pai” no “objeto Filho”, através do método ``Object.create``, que retorna um objeto vazio que herda, pelo ``prototype``, o objeto que foi passado como parâmetro. Além disto, ao instanciarmos um “objeto Filho” chamamos apenas uma vez o “objeto Pai”, e não duas como no Prototype-chaining.

```js
var aplica_heranca = function(objeto_pai, objeto_filho){
    // Faz uma cópia do protótipo da objeto_pai
    // objeto_pai → object → null
    var copiaDoObjetoPai = Object.create(objeto_pai.prototype);

    // Objeto filho herda o objeto pai através do prototype
    // objeto_filho → objeto_pai → object → null
    objeto_filho.prototype = copiaDoObjetoPai;

    //Ajusta construtor referente ao objeto filho    
    objeto_filho.prototype.constructor = objeto_filho;
}

var Carro = function(modelo, ano, qtd_portas){
    Veiculo.call(this, modelo, ano);
    this.qtd_portas = qtd_portas;
};

// Aque é utilizado o Object.create para criar a herança entre os objetos
aplica_heranca(Veiculo, Carro);

Carro.prototype.ligaTurbo = function(){
    console.log(“Tzuzzzzzzzzzzzz!!!!”);
};

var ka = new Carro("ka", "2016", 2);
ka.buzina(); // Biii Biii!!!!
ka.ligaTurbo(); // Tzuzzzzzzzzzzzz
```

#### Functional Inheritance (Herança funcional)
Nesta maneira, retorna-se um objeto utilizando notação literal, e não o **new**. E para os atributos e métodos do “objeto Filho”, altera-se o “objeto Pai” para adiciona-los.
```js
var carro = function(modelo, ano, qtd_portas){
    var veiculo = vaiculo(modelo, ano);
    veiculo .ligaTurbo= function(){
        console.log(“Tzuzzzzzzzzzzzz!!!!”);
    };
    return veiculo;
}

```

## Referências

[Caleum - Reaproveitando código com Javascript herança e protótipos](http://blog.caelum.com.br/reaproveitando-codigo-com-javascript-heranca-e-prototipos/)<br>
[Mozilla - Javascript Inheritance and the prototype chain](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)<br>
[Stackoverflow - Este é um exemplo correto de herança em Javascript](http://pt.stackoverflow.com/questions/7220/este-%C3%A9-um-exemplo-correto-de-heran%C3%A7a-em-javascript)<br>
[Frontinbrazil - Entendendo herança no Javascript](http://frontinbrazil.com.br/entendendo-heranca-no-javascript/)<br>
[Loopinfinito - herança em Javascript parte 1](http://loopinfinito.com.br/2012/05/04/heranca-em-javascript-parte-1/)<br>
[Loopinfinito - herança em Javascript parte 2](http://loopinfinito.com.br/2013/02/05/heranca-em-javascript-parte-2/)<br>
[Stackoverflow - Javascript inheritance](http://stackoverflow.com/questions/931660/javascript-inheritance)<br>

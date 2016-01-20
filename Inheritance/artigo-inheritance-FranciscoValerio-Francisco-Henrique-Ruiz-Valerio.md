# Artigo - Herança

**Autor**: Francisco Henrique Ruiz Valério - [FranciscoValerio](https://github.com/FranciscoValerio)

**Data**: 1453326364374

### Herança em JavaScript

O Javascript é uma linguagem orientada a objeto porém diferente da maioria das linguagens orientada a objetos em que a herança é baseada em classes no javascript é baseado em prototype (protótipo). Em javascript quase tudo é um objeto não existem classes. Até as funções são objetos, ou seja, você instância novos objetos a partir de outros objetos.

O prototype faz referência a todas as propriedads e métodos herdados, incluíndo seu prototype.

Podemos pensar nele como uma lista referenciando o elemento anterior na mesma. Uma das principais características da herança de prototype é que se alteramos uma propriedade ou método herdado de um objeto, essas mudanças irão refletir nos seus filhos mesmo depois de serem criadas. Isso porque os filhos remetem as propriedades e métodos do pai.

Outro aspecto importante é que você pode mudar o prototype de qualquer um dos objetos globais irá afetar todos os objetos criados usando esse prototype. Isso é perigoso, e considerado uma má prática pois objetos globais são definidos por um padrão, por isso fazer que se eles comportarem de outra forma pode ser perigoso e perder seus benefícios.

Abaixo temos um exemplo básico de como é o conceito de herança em javascript.

Exemplo:
```JS
/*
** criamos um novo objeto qualquer com o nome carro
** ele automáticamente usará o prototype de Object
*/

var carro = {};
carro.modelo = 'Fusion';
carro.marca  = 'Ford';
carro.hasOwnProperty( 'modelo' ); // metodo herdado de Object
Object.getPrototypeOf( carro );   // retorna Object

var cervejas = [ 'leff', 'insana', 'brahma red lager' ];
cervejas.reverse();                                         // metodo herdado de Array
cervejas.hasOwnProperty( 'length' );                        // metodo herdado de Object
Object.getPrototypeOf( cervejas );                          // retorna []
Object.getPrototypeOf( Object.getPrototypeOf( cervejas ) ); // retorna Object

// temBreja irá herdar propriedades de function
function temBreja(){
   console.log( 'tem mé sim!' );
   return true;
}

// aqui vemos uma função se comportando como um objeto
temBreja.call();                   // metodo herdado de function
Object.getPrototypeOf( temBreja ); // retorna function Empty(){}
```

> Obs: Podemos notar que a herança ocorre em toda a cadeia de protótipos. Quando chamamos a propriedade dentro do objeto, o interpretador de javascript irá procurar no objeto, caso não encontre procurará em seu prototype, caso também não encontre procura no prototype do prototype, e assim por diante percorrendo toda a cadeia até alcançar um prototype com valor null.

A criação de objetos e herança em JavaScript é através de funções que funcionam como construtores. Este é o padrão de herança em JavaScript, porém não entrarei a fundo nele pois a ECMAScript 5, o grupo que padroniza o JavaScript, vem criando várias funções para o trabalho com herança ficar mais agradável.

### ECMAScript 5

**Clonando**
Um novo metodo adicionado pela especificações ECMAScript 5 é o Object.create. Com ele podemos criar um novo objeto usando um objeto existente como prototype.

Exemplo:

```JS
var carro = {
   ligar: function(){
      console.log( 'carro ligado!' );
   },
   desligar: function(){
      console.log( 'carro desligado!' );
   }
};

var fusion = Object.create( carro );
fusion.ligar(); // metodo clonado de carro
```

> Caso seja nescessário com esse metodo podemos criar uma objeto que não possua prototype.

Exemplo:

```JS
var objeto = {};
objeto.toString(); // herdado de object

var objetoSemPrototipo = Object.create( null );
objetoSemPrototipo.toString(); // não reconhece esse metodo
```

**Extendendo**

Ao invés de apenas clonar, podemos criar um novo objeto herdando de um já existente. E também incluir novos metodos.

Exemplo:
```JS
var animal = {
   respirar: function(){
      console.log( 'respirando!' );
   },
   reproduzir: function(){
      console.log( 'hummmm ^^!' );
   }
};

var passaro = Object.create( animal, {
   voar: {
      value: function(){
         console.log( 'voando!' );
      }
   }
});

passaro.respirar() // metodo herdado
passaro.voar()     // metodo criado
```

**Definindo propriedades**

Nas especificações do ECMAScript 5 o novo meio de definir novas propriedades em um objeto é usando Object.defineProperty, ou seja nessa definição as propriedades de um objeto não são mais apenas valores acessados através de uma chave. Podendo assim definirmos setters, getters, se a propriedade for apenas de leitura, enumerável, etc.

Exemplo:
```JS
Object.defineProperty( passaro, 'dormir', {
   value: function(){
      console.log( 'dormindo!' );
   },
   writable: false,
   enumerable: false,
   configurable: false
});
```

* Os descriptors de uma propriedade podem ser:
   * value: o valor da propriedade, pode ser uma função, um objeto, um número, um array, entre outros.
   * writable: se false, o valor não pode ser alterado.
   * configurable: se false, não é possível deletar a propriedade ou modificar seus atributos ( writable, configurable ou enumerable ).
   * enumerable: se true, a propriedade será iterada em um loop( for( var prop in objeto ){} ).

 > Definindo getters e setters que irão ser disparados de forma transparente toda vez que a propriedade for acessada ou modificada.

 Exemplo:
 ```JS
 Object.defineProperty( cachorro, 'idade', {
    value: 1,
    get: function(){
      // já que um ano do cachorro vale 7 do homem
      return idade * 7;
   },
   set: function( valor ){
      idade = valor;
   }
})

cachorro.idade = 4;
cachorro.idade // retorna 28 ( 4 * 7 )
```

**Metodo Super**

No próximo exemplo iremos "sobreescrever" a propriedade respirar do passaro onde a mesma foi herdade de animal.

```JS
Object.defineProperty( passaro, 'respirar', {
   value: function(){
      console.log( 'respirando como um passáro!' );
   }
})

passaro.respirar(); // retorna 'respirando como um passáro!'

//Caso queria acessar a propriedade respirar definida em animal podemos fazer da seguinte maneira:

Object.getPrototypeOf( passaro ).respirar();
//o mesmo que
animal.respirar();
```

**Modificando protótipo em tempo de execução**
Em linguagens baseadas em protótipos podemos adicionar ou remover propriedades nos objetos. modificando apenas seu prototype. Caso seja adicionado um novo metodo no objeto animal todos os objetos que usaram o animal como prototype terão esse novo metodo disponível.

Exemplo:
```JS
passaro.comer(); //undefined

Object.defineProperty( animal, 'comer', {
   value: function(){
      console.log( 'comendo!' );
   }
})

passaro.comer(); // retorna comendo

###Referências
>http://loopinfinito.com.br/2012/05/04/heranca-em-javascript-parte-1/
>http://frontinbrazil.com.br/entendendo-heranca-no-javascript/

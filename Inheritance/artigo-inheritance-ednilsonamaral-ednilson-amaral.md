# Artigo - Herança  

**Autor**: Ednilson Amaral - [ednilsonamaral](https://github.com/ednilsonamaral)  
**Data**: 1454434456574


## Decifrando Herança em JavaScript


Para linguagens orientadas à objetos, como em C++ e Java, é utilizado um comportamento diferente para compartilhar código entre entidades. Sua herança nada mais é que a capacidade de suas classes em compartilharem seus métodos e atributos entre si. Um exemplo disso, é definir a relação de uma classe com outra através da palavra `extends`.  

Agora, no JavaScript essa abordagem é um pouco diferente. Para compartilharmos código entre entidades devemos conhecer um pouco sobre protótipos.


### Protótipos  

Quase todos os objetos, além de sua lista de propriedades, possuem um *prototype*, ou **protótipo**.  

Basicamente, um protótipo é outro objeto que é utilizado como fonte de *fallback* para as propriedades. Ou seja, assim que um objeto recebe uma chamada em determinada propriedade que ele não possui, seu *prototype* designado para aquela propriedade vai ser buscado, e, caso não encontre, irá buscar em outro, e assim por diante.  

Porém, se nos depararmos com um objeto vazio, o seu protótpio é o ancestral de todos os protótpios, ou seja, a entidade por trás da maioria dos objetos, o *Object.prototype*.


> As relações dos objetos JavaScript formam uma estrutura em forma de árvore, e na raiz dessa estrutura se encontra o Object.prototype. Ele fornece alguns métodos que estão presentes em todos os objetos, como o toString, que converte um objeto para uma representação em string.  
>  
> Muitos objetos não possuem o Object.prototype diretamente em seu prototype. Ao invés disso eles têm outro objeto que fornece suas propriedades padrão. Funções derivam do Function.prototype, e arrays derivam do Array.prototype.


### Construtores  

Podemos consedirar a maneira mais simples de criar objetos que vai herdar algum *prototype* é através de um construtor. Para isso, ao chamarmos uma função, basta à invocarmos com a palavra-chave `new`, fazendo assim que ela seja um construtor.


```js  
var Aluno = function (nome, idade) {  
	this.nome = nome;  
	this.idade = idade;  
};  

Aluno.prototype.serie = "1º Ano";  

//criando o objeto com o new  
var bruna = new Aluno("Bruna", 7);  
console.log(bruna);  
console.log(bruna.serie);  
```


O *prototype* de um construtor é o `Function.prototype`, onde sua propriedade *prototype* será o *prototype* de instâncias criadas através dele, porém não será seu próprio *prototype*. Enquanto outros objetos têm um *prototype* que pode ser obtido através do `Object.getPrototypeOf`.  

Vale ressaltar que devemos notar a diferença entrea a maneira que um *prototype* é associado a um construtor e a maneira que outros objetos têm um *prototype*.  

Além disso, nessa aborgadem de invocarmos o `new`, se não o invocarmos, o `this` dentro da função irá apenas se referenciar ao objeto global, assim podendo sobreescrever algumas variáveis que foram declaradas anteriormente.


No exemplo a seguir, vemos mais uma possibilidade de herança:  

```js  
var criaAluno = function (nome, idade) {  
    return {  
        nome: nome,  
        idade: idade,  
        serie: "1º Ano"  
    };  
};  

var bruna = criaAluno("Bruna", 7);  
var jose = criaAluno("José", 6);  

console.log(bruna);  
console.log(jose);  
```  

O exemplo acima demonstra, que, ao invés de criarmos um objeto para cada aluno, crio um único objeto, que irá retornar outro objeto, com os campos. Então, a cada novo aluno, basta chamarmos essa função e setarmos os valores dos campos.


### Utilizando `call` e `apply`  

```js  
var Aluno = function (nome, idade) {  
    this.nome = nome;  
    this.idade = idade;  
};  

Aluno.prototype.serie = "1º Ano";  

//criando o objeto com o new  
var bruna = new Aluno("Bruna", 7);  
console.log(bruna);  
console.log(bruna.serie);  

//criando através de um objeto vazio e utilizando __proto__  
var jose = {};  
jose.__proto__ = Aluno.prototype;  

//então, passamos como contexto e invocamos com o call ou apply  
Aluno.call(jose, "José", 6);  
//Aluno.apply(jose, ["José", 6]);  
console.log(jose);  
console.log(jose.serie); 
```  

No exemplo acima temos os mesmos métodos e atributos vistos anteriormente. Porém, agora, utilizamos a função `Aluno` para herdamos nossos outros objetos e protótipos.

Note que para `bruna` invocamos `new`; enquanto `jose` criamos um objeto vazio, passamos como contexto e invocamos através de `call` ou `apply`.


### Como fica a compatibilidade com os navegadores? (IE)  

Infelizmente (SQN), a ES5 funciona *parcialmente* em versões do IE da 9 para as mais atuais. Na versão 9, funciona parcialmente. Porém, caso você seja um carinha chato, do tipo que curte *Graceful Degradation*, vai de [shim](https://github.com/es-shims/es5-shim). Mas, se você for legal e adepto a *Progressive Enhancement*, então não terás muitos problemas.


![Compatibilidade ECMAScript5](https://raw.githubusercontent.com/ednilsonamaral/be-mean-artigos/master/img/es5.jpg)


### Concluindo  

Como vimos, existem várias formas heranças no JavaScript, baseado em protótipo. E, antes de sair escrevendo códigos complexos, devemos estar claros quanto ao entendimetno de protótipos e como funcionam heranças neles.  

Um ponto vital no qual devemos pensar quando pensamos em desempenho de nossa aplicação, para evitar algumas dores de cabeça, podemos utilizar closures ao invés de *prototypes*. Isso se diz necessário quando temos um número grande de uma cadeia de protótipos.  

Esse artigo é uma introdução à herança em JS, espero que tenha sido o mais claro e objetivo possível. Se encontrarem algum errinho, *please*, diz aí, de boa! Até mais!! :)


### Fontes  

[A vida secreta dos objetos - Eloquente JavaScript](https://github.com/braziljs/eloquente-javascript/blob/817711f1145f331ac7a5e08c4de1fa987b886a21/chapters/06-a-vida-secreta-dos-objetos.md)  
[Desvendando a linguagem JavaScript - #15 - Herança - Parte 1 - Rodrigo Branas](https://www.youtube.com/watch?v=1Y0nSEMvTt0)  
[Desvendando a linguagem JavaScript - #15 - Herança - Parte 2 - Rodrigo Branas](https://www.youtube.com/watch?v=hDhoO86cfh8)  
[Herança em JavaScript parte I - Loop Infinito](http://loopinfinito.com.br/2012/05/04/heranca-em-javascript-parte-1/)  
[Herança em JavaScript parte II - Loop Infinito](http://loopinfinito.com.br/2013/02/05/heranca-em-javascript-parte-2/)  
[Entendendo herança no JavaScript](http://frontinbrazil.com.br/entendendo-heranca-no-javascript/)  
[ES5 - Shim](https://github.com/es-shims/es5-shim)
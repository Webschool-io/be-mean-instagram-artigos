# Artigo - Herança  

**Autor**: Ednilson Amaral - [ednilsonamaral](https://github.com/ednilsonamaral)  
**Data**: 1453856004759


## Decifrando Herança em JavaScript


Para linguagens orientadas à objetos, como em C++ e Java, é utilizada um comportamento diferente para compartilhar código entre entidades. Sua herança nada mais é que a capacidade de suas classes em compartilharem seus métodos e atributos entre si. Um exemplo disso, é definir a relação de uma classe com outra através da palavra `extends`.  

Agora, no JavaScript essa abordagem é um pouco diferente. Para compartilharmos código entre entidades devemos conhecer um pouco sobre orientção à protótipo.


### Protótipos  

Bem como sabemos, um objeto é uma coleção dinâmica de chaves e valores, onde eles podem ser de qualquer tipo, e um protótipo que pode ser um objeto ou *null*. E, também sabemos, que todo objeto em JavaScript tem um protótipo.  

Para herdarmos os métodos e atributos de um determinado objeto, utilizamos como protótipo do novo objeto a ser criado. Mesmo que nós não declaremos isso em nossos códigos, todos os objetos em JavaScript utilizam outro objeto como protótipo, exceto o objeto `Object`.

```js  
var aluno = {  
	serie: "1º Ano"  
};  

var bruna = {  
	nome: "Bruna",  
	idade: 7,  
	__proto__: aluno  
};  

var jose = {  
	nome: "José",  
	idade: 6  
};  

Object.setPrototypeOf(jose, aluno);  

console.log(bruna);  
console.log(bruna.serie);  

console.log(jose);  
console.log(jose.serie);  
```

Note no exemplo acima, que herdamos métodos e atributos de outros objetos de duas formas. A primeira delas é utilizando a propriedade `__proto__` dentro do objeto `bruna`, então ele vai herdar do objeto `aluno` a série que ela se encontra.  

Para o objeto `jose` herdamos os métodos e atributos através do objeto `Object.setPrototypeOf()`, que é o mais recomendado para utilizarmos.  

Os resultados são os mesmos:  

```  
$ node heranca.js  
{ nome: 'Bruna', idade: 7 }  
1º Ano  
{ nome: 'José', idade: 6 }  
1º Ano  
```

A propriedade `__proto__` não é padrão nos navegadores, então, pode muitas vezes não funcionar. Devido a isso que a propriedade `setPrototypeOf()` é mais recomendada.  

Com esse pequeno código conseguimos ver uma cadeia de protótipos, ou seja um objeto vai herdar o protótipo de outro objeto e assim por diante. Agora fica uma dúvida, e se temos 20 alunos, qual o tamanho dessa cadeia de protótipos? E se forem 200? Como fica o desempenho de nossa aplicação? ):


### Herança por Função Fábrica  

E se eu não quiser ficar criando vários objetos e ficar herdando através de uma cadeia de protótipos, como nos exemplos acima, o que faço? Putz, vamos pensar...


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

Simples? Acho que sim! Então, criamos uma função fábrica chamada `criaAluno()` que vai nos retornar um objeto, que nele irá conter os atributos que queremos, no caso, nome, idade e série. Então, ao invés de criamos um objeto para cada aluno, criamos uma variável chamando a função fábrica e atribuindo os valores.

```  
$ node heranca.js  
{ nome: 'Bruna', idade: 7, serie: '1º Ano' }  
{ nome: 'José', idade: 6, serie: '1º Ano' }  
```


### Herança por Funções Construtoras  

É o método mais difundido de criação de objetos e herança em JavaScript. Como sabemos, uma função também é um objeto, e por isso, ela possui a propriedade `prototype`, por padrão. Nela nós definimos o protótipo da função, ou então as propriedades que os objetos irão ter caso seja invocado via `new`.


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

No exemplo acima temos os mesmos métodos e atributos vistos anteriormente. Porém, agora, utilizamos uma função construtora para herdamos nossos outros objetos e protótipos.  

Note que para `bruna` invocamos `new`; enquanto `jose` criamos um objeto vazio, passamos como contexto e invocamos através de `call` ou `apply`. Os resultados são:


```  
$ node heranca.js  
{ nome: 'Bruna', idade: 7 }  
1º Ano  
{ nome: 'José', idade: 6 }  
1º Ano  
```


Precisamos ressaltar que nessa aborgadem de invocarmos o `new`, se não o invocarmos, o `this` dentro da função irá apenas se referenciar ao objeto global, assim podendo sobreescrever algumas variáveis que foram declaradas anteriormente.


### Como fica a compatibilidade com os navegadores? (IE)  

Infelizmente (SQN), a ES5 funciona *parcialmente* em versões do IE da 9 para as mais atuais. Na versão 9, funciona parcialmente. Porém, caso você seja um carinha chato, do tipo que curte *Graceful Degradation*, vai de [shim](https://github.com/es-shims/es5-shim). Mas, se você for legal e adepto a *Progressive Enhancement*, então não terás muitos problemas.


![Compatibilidade ECMAScript5](https://raw.githubusercontent.com/ednilsonamaral/be-mean-artigos/master/img/es5.jpg)


### Concluindo  

Como vimos, existem várias formas heranças no JavaScript, baseado em protótipo. E, antes de sair escrevendo códigos complexos, devemos estar claros quanto ao entendimetno de protótipos e como funcionam heranças neles.  

Um ponto vital no qual devemos pensar quando pensamos em desempenho de nossa aplicação, para evitar algumas dores de cabeça, podemos utilizar closures ao invés de *prototypes*. Isso se diz necessário quando temos um número grande de uma cadeia de protótipos.  

Esse artigo é uma introdução à herança em JS, espero que tenha sido o mais claro e objetivo possível. Se encontrarem algum errinho, *please*, diz aí, de boa! Até mais!! :)


### Fontes  

[Desvendando a linguagem JavaScript - #15 - Herança - Parte 1 - Rodrigo Branas](https://www.youtube.com/watch?v=1Y0nSEMvTt0)  
[Desvendando a linguagem JavaScript - #15 - Herança - Parte 2 - Rodrigo Branas](https://www.youtube.com/watch?v=hDhoO86cfh8)  
[Herança em JavaScript parte I - Loop Infinito](http://loopinfinito.com.br/2012/05/04/heranca-em-javascript-parte-1/)  
[Herança em JavaScript parte II - Loop Infinito](http://loopinfinito.com.br/2013/02/05/heranca-em-javascript-parte-2/)  
[Entendendo herança no JavaScript](http://frontinbrazil.com.br/entendendo-heranca-no-javascript/)  
[ES5 - Shim](https://github.com/es-shims/es5-shim)
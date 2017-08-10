# Artigo - Herança (Inheritance)

**Autor**: Carlos Henrique Oliveira Ribeiro - [carloshenriqueribeiro](https://github.com/carloshenriqueribeiro)  
**Data**: 1502383761385

## O que é herança ?
A herança em programação foi baseada na herança da natureza, mas se refere à característica da linguagem de permitir que uma entidade adquira os métodos e propriedades de outra entidade.
Essas entidades compartilharão esses métodos e propriedades.  
É uma das principais características de linguagens orientadas a objeto, nelas a herança é feita por meio de classes, uma classe herda de outra classe suas propriedades e métodos.

## Para que usar herança?
Alguns dos grandes problemas enfrentados no desenvolvimento de sistemas são: Duplicidade de código, grande quantidade de linhas de código e lógicas inconsistentes.  
A herança é uma das formas de atenuar esses problemas, herdando métodos permite reuso de código, consequentemente diminuição na quantidade de linhas de código, alterações e testes no algoritmo serão feitas apenas uma vez, diminuindo tempo de manutenção e probabilidade de erros e inconsistências.  

## Herança em Javascript - Prototype
Em Javascript não existem classes e sim objetos portanto a herança é feita via protótipo, cada objeto herda as propriedades e "métodos" de seu objeto protótipo, que é um objeto como qualquer outro.  
Um objeto possue internamente a referência de qual objeto é seu protótipo, este por sua vez também possue referência de outro objeto que é seu protótipo, formando assim uma relação entre todos eles, essa relação é conhecida como *prototype chain* ou *cadeia de protótipos*. O objeto do topo da caia é o *Object*.  
Apenas em casos de heranças feitas com funções a referência ao protótipo não é apenas interna, é explícita na propriedade *prototype*.
Um detalhe importante é que a propriedade herdada não passa a ser uma propriedade do objeto que a herda, a propriedade é procurada por toda a cadeia a cada vez que é acessada, ao tentar acessar uma propriedade de um objeto, que pode ser feito usando **dot notation**: `nomeObjeto.nomePropriedade`.  

## O que exatamente é um objeto (object) em Javascript?
Objeto em Javascript é um conjunto dinâmico de propriedades, as propriedades são compostas por chave e valor, onde chave é do tipo string e designa o nome da propriedade e valor pode ser de qualquer tipo e contém o valor dessa propriedade, o valor pode ser inclusive uma função.  
**Exemplos de objetos:** arrays (matrizes), functions (funções), date (data) e regular expressions (expressões regulares)
**O que não são objetos:** tipos primitivos (*string*, *boolean*, *number*, *null* e *undefined*).  

Quando o valor de uma propriedade é uma função essa também é conhecida como método.  

## Como uso herança na prática?

A herança pode ser feita por meio de funções ou métodos, seguem alguns exemplos básicos de algumas formas.  

#### Pela propriedade `__proto__`:
```js
//objeto que contém a propriedade corDosOlhos que será herdada
var pai = {
    corDosOlhos: 'Azul'
};
//Objeto que herdará as propriedades do objeto "pai"
var filho = {
    nome: 'Sofia',
    altura: 1.50,
    __proto__: pai
};
console.log(pai); //{ corDosOlhos: 'Azul' }
//As propriedades herdadas não possam a fazer parte do objeto que recebeu a herança
console.log(filho); // { nome: 'Sofia', altura: 1.50 }
//As propriedades herdadas precisam ser acessadas explícitamente
console.log(filho.corDosOlhos) // 'Azul'
```
**método mais utilizado antigamente.**


#### Pelo método `setPrototypeOf`:
```js
//objeto que contém a propriedade corDosOlhos que será herdada
var pai = {
    corDosOlhos: 'Azul'
};
//Objeto que herdará as propriedades do objeto "pai"
var filho = {
    nome: 'Flariston',
    altura: 1.74
};
//Chamada do método que fará a herança
Object.setPrototypeOf(filho, pai);
console.log(pai); //{ corDosOlhos: 'Azul' }
//As propriedades herdadas não possam a fazer parte do objeto que recebeu a herança
console.log(filho); // { nome: 'Flariston', altura: 1.74 }
//As propriedades herdadas precisam ser acessadas explícitamente
console.log(filho.corDosOlhos) // 'Azul'
```
**Essa forma não é padronizada, então nem todos os interpretadores a reconhecem, por isso pode não funcionar em alguns ambientes.**


#### Pelo método "Object.create":
```js
let mae = {
    corDosOlhos: 'Verde'
};
// A herança já ocorre na execução da função
// pois nela já é passado o objeto protótipo como parâmetro
let filha = Object.create(mae);
filha.nome = 'Maria',
filha.signo = 'virgem';
console.log(filha); // {nome: 'Maria', signo: virgem}
console.log(filha.signo); // virgem
```
**Esse método a partir do [ES6](http://www.ecma-international.org/ecma-262/6.0/).**


#### Funções construtoras:
```js
let Endereco = function(logradouro, numero) {
    this.logradouro = logradouro;
    this.numero = numero;
};
// `prototype` é uma propriedade que existe apenas
// em funções
Endereco.prototype.bairro = 'Mooca, belo!';

let enderecoResidencial = new Endereco('Rua Juventus', 666);

console.log(enderecoResidencial);
console.log(enderecoResidencial.bairro);
```
**Esse método constrói um objeto já definindo um protótipo, que pe o objeto passado como parametro.**

**Dicas:**
1. A criação de um objeto por uma função construtora usando o `new` também já cria o objeto tendo a função como objeto protótipo.  
2. O objeto também poderia ser criado pelos métodos `call` e `apply` da função construtora mas, dessa forma, o protótipo não seria atribuido automaticamente, teria que declarar explicitamente o protótipo do objeto recém criado via propriedade `__proto__` ou via método `Object.setPrototypeOf`.

Fonte:  
[www.github.com/webshool.com](www.github.com/webshool.com)

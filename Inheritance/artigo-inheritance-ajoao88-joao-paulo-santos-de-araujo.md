# Artigo - Herança (Inheritance)

**Autor**: João Paulo S. de Araújo - [ajoao88](https://github.com/ajoao88)  
**Data**: 1472673592494
## O que é herança?
Em biologia é o processo pelo qual um organismo ou célula adquire ou torna-se predisposto a adquirir características semelhantes à do organismo ou célula que o gerou.
## Mas o que é herança em programação?
A herança em programação foi baseada na herança da natureza, mas se refere à característica da linguagem de permitir que uma entidade adquira os métodos e propriedades de outra entidade.
Essas entidades compartilharão esses métodos e propriedades.  
É uma das principais características de linguagens orientadas a objeto, nelas a herança é feita por meio de classes, uma classe herda de outra classe suas propriedades e métodos.  
## Para que usar herança?
Alguns dos grandes problemas enfrentados no desenvolvimento de sistemas são: Duplicidade de código, grande quantidade de linhas de código e lógicas inconsistentes.  
Esses causam muita dor de cabeça para dar manutenção no sistema, pois um mesmo algoritmo pode estar repetido várias vezes no projeto, qualquer necessidade de alteração o código terá que ser rastreado e alterado várias vezes, correndo sérios riscos de não encontrar uma das duplicatas, de cometer algum erro em alguma das alterações, causando erros ou deixando diferenças entre os algoritmos que deveriam estar iguais. Na melhor das hipóteses fará uma manutenção bem mais lenta, porque terá que alterar o algoritmo várias vezes e terá que testar cada repetição dele.  
A herança é uma das formas de atenuar esses problemas, herdando métodos permite reuso de código, consequentemente diminuição na quantidade de linhas de código, alterações e testes no algoritmo serão feitas apenas uma vez, diminuindo tempo de manutenção e probabilidade de erros e inconsistências.  
Isso é muito útil, pois a curto prazo um código ruim já causa transtornos a longo prazo será muito prejudicial, pois código ruim dará dor de cabeça enquanto existir.
## Herança em Javascript
Em Javascript até a especificação [ES5](http://www.ecma-international.org/ecma-262/5.1/) não existem classes e a herança é feita apenas via protótipo, cada objeto herda as propriedades e métodos de seu objeto protótipo, que é um objeto como qualquer outro. Nesse artigo falaremos apenas de herança via protótipos.  
Um objeto possue internamente a referência de qual objeto é seu protótipo, este por sua vez também possue referência de outro objeto que é seu protótipo, formando assim uma relação entre todos eles.  
Essa relação é conhecida como *prototype chain* ou *cadeia de protótipos*, ela funciona de maneira análoga à uma cascata, cada objeto herda as propriedades de todos os protótipos acima dele, o segundo objeto da cadeia herda do primeiro, o terceiro herda do segundo e do primeiro, o quarto herda dos três e assim sucessivamente, o primeiro objeto é o *Object*, todos os objetos herdam dele, está no topo de todas as cadeias de protótipos e  por isso ele não possue protótipo.  
Apenas em casos de heranças feitas com funções a referência ao protótipo não é apenas interna, é explícita na propriedade *prototype*.
Uma propriedade herdada não passa a ser uma propriedade do objeto que a herda, a propriedade é procurada por toda a cadeia a cada vez que é acessada, ao tentar acessar uma propriedade de um objeto, que pode ser feito usando **dot notation**: `nomeObjeto.nomePropriedade`, o interpretador irá verificar se a propriedade existe nesse objeto, caso não exista irá procurá-la em seu protótipo, caso não exista irá procurá-la no protótipo do protótipo e assim irá procurar a propriedade por toda a cadeia de protótipos desse objeto, como se estivesse subindo cada nivel da cascata, até que seja encontrada a propriedade ou uma referência de protótipo nula, que ocorre quando chega no topo da cadeia que é o objeto *Object*, ele possue *null* em sua referência de protótipo.  
## O que exatamente é um objeto (object) em Javascript?
Objeto em Javascript é um conjunto dinâmico de propriedades, as propriedades são compostas por chave e valor, onde chave é do tipo string e designa o nome da propriedade e valor pode ser de qualquer tipo e contém o valor dessa propriedade, o valor pode ser inclusive uma função.  
Um objeto em Javascript é do tipo *object*, é o tipo de dado principal da linguagem, pois praticamente tudo em Javascript é um objeto (herdam de *Object*), incluindo Arrays (matrizes), functions (funções), date (data) e regular expressions (expressões regulares), apenas fogem dessa regra os tipos primitivos (*string*, *boolean*, *number*, *null* e *undefined*).  
Quando o valor de uma propriedade é uma função essa tambpem é conhecida como método.  
Lembrando que, uma propriedade herdada não passa a fazer parte do objeto que as herdou.
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
Essa forma não é padronizada, então nem todos os interpretadores a reconhecem, por isso pode não funcionar em alguns ambientes.
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
Esse método pertence a especificação oficial, mas apenas a partir da especificação [ES6](http://www.ecma-international.org/ecma-262/6.0/).
#### Pelo método "Object.create":
```js
var pai = {
    corDosOlhos: 'Azul'
};
// A herança já ocorre na execução da função
// pois nela já é passado o objeto protótipo como parâmetro
var filho2 = Object.create(pai);
filho2.nome = 'Euclides',
filho2.altura = 1.88;
console.log(filho2); // {nome: 'Euclides', altura: 1.88}
console.log(filho2.altura); // 1.88
```
Esse método constrói um objeto já definindo um protótipo, que pe o objeto passado como parametro.
#### Funções construtoras:
```js
var Endereco = function(logradouro, numero) {
    this.logradouro = logradouro;
    this.numero = numero;
};
// `prototype` é uma propriedade que existe apenas
// em funções
Endereco.prototype.bairro = 'Bairro';

var enderecoResidencial = new Endereco('Rua Tal', 123);

console.log(enderecoResidencial);
console.log(enderecoResidencial.bairro);
```
A criação de um objeto por uma função construtora usando o `new` também já cria o objeto tendo a função como objeto protótipo.  
O objeto também poderia ser criado pelos métodos `call` e `apply` da função construtora mas, dessa forma, o protótipo não seria atribuido automaticamente, teria que declarar explicitamente o protótipo do objeto recém criado via propriedade `__proto__` ou via método `Object.setPrototypeOf`.

Fontes:  
FLANAGAN, D. *Javascript: The Definitive Guide.* 6. ed. : O'Reilly, 2011.  
http://blog.caelum.com.br/reaproveitando-codigo-com-javascript-heranca-e-prototipos/  
http://blog.caelum.com.br/criacao-de-objetos-em-javascript/  
https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain  
http://loopinfinito.com.br/2012/05/04/heranca-em-javascript-parte-1/  
http://loopinfinito.com.br/2013/02/05/heranca-em-javascript-parte-2/  
http://frontinbrazil.com.br/entendendo-heranca-no-javascript/  
https://pt.wikipedia.org/wiki/Heran%C3%A7a_(programa%C3%A7%C3%A3o)  
https://github.com/braziljs/eloquente-javascript  
[Desvendando a linguagem JavaScript - #14 - Herança - Parte 1 - Rodrigo Branas](https://www.youtube.com/watch?v=1Y0nSEMvTt0)  
[Desvendando a linguagem JavaScript - #15 - Herança - Parte 2 - Rodrigo Branas](https://www.youtube.com/watch?v=hDhoO86cfh8)

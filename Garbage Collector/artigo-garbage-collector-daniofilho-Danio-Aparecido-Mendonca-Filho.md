# Artigo - Garbage Collector

**Autor**: Dânio Aparecido Mendonça Filho - [daniofilho](http://github.com/daniofilho)

**Data**: 1450438298356

## Garbage Collector ##

Este artigo tratará sobre Garbage Collector, explicando o que é e como isso funciona dentro do Javascript.

Primeiramente é importante entender o que esse termo significa. Garbage Collector (Coletorde Lixo) é um processo que é usado no gerenciamento da memória visando otimizar seu uso. Este processo procura dentro da memória por partes que não estão sendo mais usadas pela aplicação e as remove, com isso libera espaço e evita estouros de memória.

Vale ressaltar que este é um processo que é feito automaticamente.

### O Garbage Collector no Javascript ###

O Garbage Collector no Javascript atua nas variáveis, objetos e arrays. Em linguagem de baixo nível, como o C por exemplo, existem primitivas que gerenciam memória de baixo nível. Já no caso do Javascript as variáveis e objetos são criadas dinamicamente e liberadas automaticamente quando não estão em uso. O Javascript consegue mapear esses itens e determinar quando eles não serão mais usados para liberar a memória.

O ciclo de vida da memória funciona em 3 etapas:

**1. Alocar a memória quando houver necessidade**

Como por exemplo na criação de uma variável.

~~~ js
var x = 10;
~~~

**2. Utilizar este espaço alocado**

Ler a variável, processa-la, etc.

~~~ js
x++;
console.log(x);
~~~

** 3. Liberar este espaço caso não tenha mais uso.**

No caso do Javascript, este processo é feito automaticamente, o que signifca que o programador não precisa ficar limpando este item da memória manualmente como é feito em linguagens de baixo nível.

Não há como saber exatamente quando um item pode, ou não, ser removido da memória para liberar espaço, na maioria das vezes apenas o programador sabe disso. Para que esta técnica funcione automaticamente, o Garbage Collector é usado para situações de maneira geral onde ele possa ser aplicado. Para isso foram criados algoritmos de Garbage Collector, como **Referências**, **Referência de contagem do garbage collection** e **Algoritmo de varredura e rotulação**

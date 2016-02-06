# Artigo - Garbage Collector
**Autor**: Sérgio Kopplin - [sergiokopplin](https://github.com/sergiokopplin)
**Data**: 1454719306196

Garbage Collector no Javascript.

O Javascript utiliza alocações de espaço na memória para guardar variáveis, tais como Strings, Objects, Arrays, etc.
O JS é inteligente o suficiente para alocar o espaço quando elas são criadas e liberar o mesmo quando elas não são mais utilizadas. Esse último processo pode ser chamado de **Garbage Collector**, e ele consiste em entender onde as referências de um objeto não estão sendo utilizadas, sendo assim não existe uma referência ao **prototype** do mesmo.

O Garbage Collector atua de forma inteligente e apenas libera o que não é mais utilizado, sendo usado com diferentes algorítmos para navegadores distintos, mas tendo como resultado a mesma liberação de memória alocada.

Na prática o garbage collection atravessa a lista de todas as variáveis e marca todos os valores com referência a estas variáveis. O garbage collector é capaz de encontrar cada valor único que ainda está acessível, portanto os valores não marcados são inacessíveis, logo, podem ser descartados da memória.

ref:
- [developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)
- [v8project.blogspot.com.br](http://v8project.blogspot.com.br/2015/08/getting-garbage-collection-for-free.html)

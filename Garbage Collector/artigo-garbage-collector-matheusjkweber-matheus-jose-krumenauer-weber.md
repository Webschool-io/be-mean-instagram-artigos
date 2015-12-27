# Artigo - Garbage Collector
**Autor**: Matheus Jose Krumenauer Weber - [matheusjkweber](https://github.com/matheusjkweber)
**Data**: 1451170776959

#Garbage Collector
Ao programar em qualquer linguagem temos que ter maneiras de controlar a memória, algumas abordagens para o manejamento de memória podem ser manuais(C, C++) ou automaticamente(Java, Javascript), nesse artigo explicarei brevemente sobre o Garbage Collector, a maneira a qual o Javascript desaloca automaticamente valores da memória.

A maneira mais simples de explicar como funciona o Garbage Collector é imaginar o mesmo como um caminhão de lixo que recolhe automaticamente todo o lixo(objetos) que não está mais sendo utilizado na sua memória, por exemplo, podemos definir uma lista encadeada com os seguintes objetos:

```
1-2-3-4-5
```

Para remover o '2' em uma linguagem como C teríamos que, além de remover o ponteiro que liga o nodo 1 para o nodo 2, desalocar o nodo 2 da memória para que o mesmo deixe de ocupar memória, em linguagens com Garbage Collector não precisamos nos preocupar em desalocar o nodo 2 da memória, pois assim que removemos a referência para ele, o Garbage Collector tratará de remover o nodo 2, pois o mesmo não é mais acessível e é considerado lixo.

Essa é uma abordagem mais alto nível para o manejamento de memória em uma linguagem, faz com que o programador não precise se preocupar tanto com a mesma.
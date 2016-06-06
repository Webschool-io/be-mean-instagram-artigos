# Artigo - Garbage Collector

**Autor**: Eduardo Pinheiro Quaresma Castro - [edupinheiro](https://github.com/edupinheiro)

**Data**: 1465212318

### Garbage Collector

O Garbage Collector (Coletor de Lixo) nada mais é do que um coletor que busca limpar a memória ocupada por objetos que não estão mais em uso. Ocorre com mais frequencia quando objetos estão sendo criados com pouco intervalo de tempo.
Tem por característica fundamental saber quando é seguro para recuperar a memória: nunca deve recuperar valores ainda em uso, recuperando somente valores inacessíveis, ou seja, valores que não podem ser referidos por qualquer variável, atributos de objetos ou elementos de matrizes no programa.


### Como é a execução de funções e armazenamento de variáveis com JavaScript?

Os valores declarados em Javascript são armazenados quando objetos são criados, e logo após seu uso, o espaço da memória alocado para eles é liberado.

As variáveis em Javascript são alocadas conforme são declaradas:
```
//aloca memória para um número
var x = 345;

//aloca memória para um objeto
var obj = { a: 1, b: "teste"};
```

Com as funções também não é diferente: o espaço é alocado conforme elas são definidas e o javascript as trata como um objeto que pode ser chamado. Não basta apenas definir uma função, ela precisa ser chamada para entrar em execução, da seguinte maneira:

```
exemplo(12, "teste");
```

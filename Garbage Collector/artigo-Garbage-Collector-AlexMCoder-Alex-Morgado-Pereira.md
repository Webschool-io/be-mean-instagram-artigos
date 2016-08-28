# Artigo - Garbage Collector
**Autor**: Alex Morgado Pereira - [AlexMCoder](https://github.com/AlexMCoder)

##Explique como funciona o Garbage Collector no JavaScript e como é a execução de funções e armazenamento de variáveis com JavaScript.

No Javascript quando são criados Objetos, Strings, Variáveis e entre outros, automaticamente são liberados quando não são mais usados, isto é chamado de **garbage collection**. É um erro pensar em não se preocupar com o consumo de memória por causa disso.

O ciclo de vida é bem simples e segue um padrão para todas as linguagens, primeiro aloca a memória que precisa. Segundo é utilizado e nessa parte ela pode ler ou escrever - por exemplo. Em terceiro ela libera da memória alocada quando não for mais necessário.
Vale lembrar que a primeira e a segunda parte são usadas em praticamente em todas as linguagens, exceto a terceira, pois ela é usada em linguagens de baixo nível.

###Alocação no Javascript
Para não deixar o programador preocupado com isso, o javascript faz isso conforme os valores são declarados, vajamos um exemplo:
```
var idade = 23; // aloca memória para um número
var curso = "Be-Mean-Instagram"; // aloca memória para uma string 

var obj = {
a: 1,
b: null
}; // alocando memoria para um objeto

function soma(a){
return a + 2;
} // aloca uma função

```





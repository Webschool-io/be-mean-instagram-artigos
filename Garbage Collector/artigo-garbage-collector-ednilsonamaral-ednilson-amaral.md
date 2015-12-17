# Artigo - Garbage Collector  
**Autor:** Ednilson Amaral  
**Data:** 1450363509242


## Garbage Collection  

Basicamente é uma forma de gerenciamento autómática de memória. Onde o *garbage* ou simplesmente coletor, tenta recuperar lixo, ou seja, recuperar a memória ocupada por objetos que não estão mais em uso pelo sistema.  

Esse processo é automático e invisível ao programador. O JavaScript usa o *garbage collection* (coleta de lixo) para recuperar a memória ocupada por *strings*, *objects*, *arrays* e funções que não estão mais em uso.  

Isso é uma vantagem ao programador que não precisa se preocupar em liberar memória.  

Podemos destacar como característica fundamental de *garbage collection* que ele deve ser capaz de determinar quando é seguro recuperar memória. Ele nunca vai recuperar objetos que ainda estão em uso.


## Como é a execução de funções e armazenamento de variáveis com JavaScript  

Diferentemente de outras linguagens de baixo nível, como C, por exemplo, o JavaScript não possui gerenciamento de memória primitivas.  

Os valores do JavaScript são armazenados (alocados) quando objetos, *strings*, etc., são criados. E, após seu uso, essa memória na qual foi alocado é liberada. Esse processo de liberar memória do que não está mais em uso é o *garbage collection*, mencionado acima.  

As alocações de variáveis no JavaScript não feitas conforme elas são declaras, conforme o exemplo abaixo:

```js  
var n = 123; // aloca memória para um número  
var s = "azerty"; // aloca memória para uma string  

var o = {  
  a: 1,  
  b: null  
}; // aloca memória para um objeto e seus valores contidos  

// (assim como o objeto) aloca memória para o vetor e   
// seus valores contidos  
var a = [1, null, "abra"];  
```  

Como podemos observar no exemplo acima, ele aloca na memória conforme vamos declarando nossas variáveis.  

As funções, também são alocadas conforme são declaradas. No caso de funçoes, ele aloca como um objeto que pode ser chamado.


Definir uma função não basta para executá-la. Definir é simplesmente dar um nome a função e especificar o que será feito quando tal função for chamada.  

```js  
media(10, 8);  

//ou  

console.log(media(10, 8));  
```  

Podemos chamar a função das duas maneiras acima. Ou também, chamar uma função dentro de outra função. As funções são objetos, e, esses objetos têm métodos.


## Fontes  

[Garbage Collection - Docstore - O'Reilly - JavaScript The Definitive Guide](http://docstore.mik.ua/orelly/webprog/jscript/ch04_05.htm#IXT-4-56957)  
[Getting Garbage Collection for Free](http://v8project.blogspot.com.br/2015/08/getting-garbage-collection-for-free.html)  
[Garbage Collection - Mozilla Developer Network](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Memory_Management#Garbage_collection)  
[Funções - Mozilla Developer Network](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Fun%C3%A7%C3%B5es#Calling_functions) 
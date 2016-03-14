# Artigo - Garbage Collector
**Autor**: Filipe Leuch Bonfim - [filipe1309](https://github.com/filipe1309)<br>
**Data**: 1455582829631


## Introdução
Esse artigo visa passar um visão geral do funcionamento do Garbage Collector no Javascript.

## Mas primeiro, um pouco sobre Alocação e Ciclo de vida da memória

### Alocações no Javascript
Existem duas formas que podem ocorrer a alocação de memória, no Javascript:
#### 1. Inicialização de valor
Ocorre quando um valor é declarado.
```js
// Aloca memória na declaração de uma variável
var ini = “teste”;

// Aloca memória na declaração de um objeto
var obj = {atrib1: 1, atrb2: “2”};

// Aloca memória na declaração de um array
var arr = [1, “dois”];

// Aloca memória na declaração de uma função (que em javascript é um objeto que pode ser chamada)
function minhaFuncao( minhaVar ) {
    console.log( minhaVar );
}

// Aloca memória em expressões de funções
algumElemento.addEventListener('click', function(){
  algumElemento.style.fontSize = '20px';
}, false);


```

#### 2. Chamada de uma função
Ocorre quando a chamada de uma função, ou algum método, resulta na alocação de um objeto ou valor.
```js
// Alocação em chamadas de função
var meuElemento = document.creatElement(“span”);

// Alocação em chamadas de métodos que alocam novos valores ou objetos
var arr1 = [“um”, “dois”];
var arr2 = [“tres”, “quatro”];
var arr3 = arr1.concat(arr2);
```


### Ciclo de Vida da Memória
Praticamente todas as linguagens de programação adotam o mesmo **ciclo de vida da memória**:
- Alocar a memória necessária
- Utilizar (através da leitura ou escrita)
- Quando não for mais necessária, liberar a memória previamente alocada

O funcionamento da terceira etapa varia de acordo com o nível de abstração da linguagem adota. Se for de baixo nível, esta etapa deverá ser explicita na código. Caso for uma linguagem de alto nível, esta etapa é implícita, e geralmente é realizada pelo *Garbage Collector*.

## Agora sim, o que é **Garbage Collector**?
*Garbage Collector*, ou Coletor de lixo em português, é um mecanismo para facilitar o gerenciamento de memória, que é utilizado por diversas linguagens de programação. Ele tem a função de **liberar**, de forma automática, o **espaço na memória**, através do procedimento de  remover dela o que não será mais utilizado.

### Mas, nem tudo são flores
Descobrir se um espaço na memória pode ser liberado é um problema indecidível, ou seja, não existe um algoritmo para isto. Por conta disto, as implementações existentes são uma solução limitada para o problema geral.

## Funcionamento do Garbage Collector no JavaScript
A seguir serão apresentados dois algoritmos que foram utilizadas para se implementar o *Garbage Collector* no Javascript.

Como a implementação de um algoritmo para o *Garbage Collector* é um problema indecidível, na javascript foram adotados dois algoritmos com soluções parciais:
- Reference Counting
- Mark and Sweep.   

### Reference Counting (Referência de contagem)
O Garbage Collector remove um objeto quando:
> “Um objeto não tem outro objeto referenciando ele"

Nesse algoritmo, um objeto é removido quando não existe nenhum referência para o mesmo.

#### Exemplo
```js
var obj1 = {
    propriedade: “um”
}

var obj2 = obj1;

obj1 = 123;

 var obj2  = 456;

// neste momento o obj1 é removido pelo garbage collector, pois não existe mais nenhuma referência a esse objeto.
```
Um problema desta solução, é que objetos com referências cíclicas não são removidos. Por exemplo objetos que referenciam um ao outro, ou que se auto referenciam não são removidos da memória.

### Mark and Sweep (Varredura e Rotulação)
O Garbage Collector remove um objeto quando:
> "um objeto é inacessível"

Esse algoritmo utiliza uma lista de objetos (*roots*, ou raízes em português), para **marcar**, periodicamente, aqueles que são referenciados por estes, e depois marcar aqueles referenciados pelos marcados, e assim por diante.

Após a fase de marcação, também chamada de rotulação, é iniciada a fase de **varredura** . Nesta fase, todos os objetos não marcados, ou seja inacessíveis, são removidos da memória.

## Referências

[Mozilla - Javascript Memory Management](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Memory_Management)<br>
[Stackoverflow - What is Javascript Garbage Collection](http://stackoverflow.com/questions/864516/what-is-javascript-garbage-collection)<br>
[docstore - JScript](http://docstore.mik.ua/orelly/webprog/jscript/ch11_03.htm)<br>
[codesandscripts - Javascript and Garbage Collection ](http://www.codesandscripts.com/2014/06/javascript-and-garbage-collection.html)<br>

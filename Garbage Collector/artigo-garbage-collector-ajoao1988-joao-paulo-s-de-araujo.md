# Artigo - Garbage Collector
**Autor**: João Paulo S. de Araújo - [ajoao88](https://github.com/ajoao88)  
**Data**: 1471979415586

# Javascript - Garbage Collector (Coletor de lixo)
O **Garbage Collector** é a funcionalidade da Javascript responsável por automatizar a liberação de memória ocupada por strings, objetos, matrizes e funções que não serão mais utilizadas. Em algumas linguagens como **C** esse processo é manual.
Mas antes de explicar como o Garbage collector funciona preciso falar sobre armazenamento de variáveis e execução de funções em Javascript, pois o Garbage Collector é uma solução e não tem sentido explicar uma solução sem apresentar o problema.

## Armazenamento de variáveis
Entenda por variáveis também: objetos, suas propriedades, arrays (matriz) e seus elementos.
A Javascript é uma variável conhecida como *não tipada (untyped)*, isso significa que suas varáveis não possuem um tipo explícito e podem armazenar valores de qualquer tipo, na verdade para poder ter essa característica de *não tipada* os valores são armazenados em algum lugar na memória e as variáveis possuem apenas a referência do endereço onde estão os valores, dito isso, imagina se a variável perde a referência desse endereço, o valor ficaria na memória perdido e ocupando espaço. Após uma variável local ter um valor e o fluxo do programa sair de seu escopo (ex. sair da função que a variável foi declarada) ela é destruída e o valor fica perdido na memória, o **Garbage Collector** existe para resolver esse problema, automaticamente ele identifica esses valores que não serão mais necessários e os limpa do endereço da memória para que possa ser reusado.

## Mas como o Garbage Collector sabe que um valor não será mais necessário?
Elen possue algoritmos que o permitem identificar quando um valor não é mais necessário, existem dois algoritmos base: **Marcação e varredura (Mark-and-Sweep)** e **Contagem de referências (Reference Counting)**, mas esse último além de ser defasado e apresentar falhas é pouco utilizado, então falaremos apenas sobre o primeiro. São algoritmos base porque podem ter algumas variações e otmizações em sua implementação nos interpretadores Javascript.

## Como funciona o algoritmo **Marcação e varredura (Mark-and-Sweep)**?
Periódicamente, esse algoritmo executa um processo em duas fases.
Na fase de **marcação** ele percorre por todas as variáveis do ambiente Javascript e marca todos os valores referenciados por essas variáveis, após o término,
os valores que não são referenciados por nenhuma variável ficam desmarcados e se inicia a fase de varredura.
Na fase de **varredura** todos os valores do ambiente Javascript que não estão marcados são inalcançáveis, sendo inalcançáveis não podem ser usados e podem ser limpos da memória.
Nesse algoritmo base, o processo é feito de uma só vez, e por isso há um queda no desempenho da aplicação, para atenuar esse problema o algoritmo sofre algumas variações e otimizações no momento de de ser implementado em um interpretador Javascript, mas seu funcionamento segue o mesmo fluxo.

## Mas como é o escopo da variável?
O escopo de uma variável é a região do programa que ela foi declarada e região que é acessível.  
O escopo de uma variável pode ser local ou global, quando global ela é acessível por toda a aplicação e quando local é acessível apenas no contexto de declaração.  
Uma variável recebe **escopo global** quando declarada com a palavra-chave `var` fora de qualquer função ou atribuindo diretamente um valor à variável sem declará-la usando `var`, que é uma pratica ruim, por exemplo:
```js
function f() {
  teste = 'Sem prefixo var :(';
}
f(); //Invoca a execução da função
console.log(teste); //Sem prefixo var :(
```
Recebe **escopo local** quando declarada com o `var` dentro de uma função, sendo acessível dentro de toda a função e deixando de existir fora dela, por exemplo:
```js
function f() {
  var escopo = 'Eu existo';
  console.log(escopo); // Eu existo
}
f(); //Invoca a execução da função
console.log(escopo); //É lançado um erro, pois a variável não existe
```
Ao executar uma função é criado um contexto de execução (**Closure**) e com ele o escopo, depois são declaradas todas as variáveis antes de mais nada, isso se chama **Hoisting**, as váriáveis declaradas dentro da função só existirão nesse contexto que deixará de existir ao término da execução da função, tornando inacessíveis todos os valores que essas variáveis referenciam.

Fontes:  
FLANAGAN, D. *Javascript: The Definitive Guide.* 4. ed. : O'Reilly, 2002. Capítulos: 4 e 11.3  
Disponível em:
[Link](http://docstore.mik.ua/orelly/webprog/jscript/index.htm)  
Acesso em: 26/08/2016  

https://braziljs.org/blog/funcoes-em-javascript/  
https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Memory_Management  

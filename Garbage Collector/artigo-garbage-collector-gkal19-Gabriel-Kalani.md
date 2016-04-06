# Artigo - Garbage Collector  
**Autor:** Gabriel Kalani 
**Data:** 1454797528600


## Garbage Collection  

**Garbage Collector** significa **"Coletor de Lixo"**, seus princípios básicos do coletor de lixo são encontrar objetos de um programa que não serão mais acessados no futuro, e desalocar os recursos utilizados por tais objetos.<br>
Ele é básicamente uma forma de gerenciamento autómática de memória.<br>
No JavaScript, a liberação de memória é feita automaticamente.

Foi inventado por John McCarthy por volta de 1959 para resolver problemas de gerenciamento manual de memória em Lisp.

![John McCarthy](https://upload.wikimedia.org/wikipedia/commons/thumb/4/49/John_McCarthy_Stanford.jpg/800px-John_McCarthy_Stanford.jpg)<br>
John McCarthy é conhecido pelos estudos no campo da inteligência artificial e por ser o criador da linguagem de programação Lisp.

## O que é o Lisp
Lisp é uma linguagem madura, concebida atenciosamente, altamente portável, linguagem de força industrial com a qual desenvolvedores em todo o mundo contam para:

- Ferramenta rápida e altamente personalizável para fazer coisas do dia a dia.
- Aplicações grandes, complexas e críticas as quais seriam impossíveis desenvolver em outra linguagem.
- Prototipação rápida e Rapid Application Development (RAD).
- Aplicações de alta disponibilidade, principalmente aquelas que necessitam de mudanças após a etapa inicial.

A linguagem teve um grande sucesso em software do ramo de negócios, engenharia, processamento de documentos, hipermídia (incluindo a Web), matemática, gráficos e animação (Mirai), inteligência artificial e processamento de linguagem natural.<br>

certas categorias e defeitos de software são eliminadas ou reduzidas e uma das vantagens do **Garbage Collection**, ele livra o programador de lidar manualmente com o gerenciamento de memória.
Esse processo é automático e invisível ao programador. O JavaScript usa o *garbage collection* (coleta de lixo) para recuperar a memória ocupada por *strings*, *objects*, *arrays* e funções que não estão mais em uso.  

Algo bem fundamental no **Garbage Collection**. Ele nunca vai recuperar objetos que ainda estão em uso.

## Referências
O principal conceito de algoritmos do garbage collection depende do conceito de referência. Dentro do contexto de gerenciamento de memória, um objeto é dito para fezer referência à outro objeto.<br>

## Exemplo

```JS
setTimeout(function() { doSomething() }, 10);


var myfunc = function() { doSomething() }
setTimeout(myfunc, 10);
```

Agora falemos mais sobre o Garbage Collector.<br>
Mas neste momento vamos falar de sua desvantagem.

## Desvantagens
O **Garbage Collector** é um processo que consome muito seus recursos computacionais - equipamentos, as instalações ou bancos de dados direta ou indiretamente administrados - para decidir quais partes da memória podem ser liberadas, enquanto no gerenciamento manual esse consumo é mínimo.<br>
No momento em que o objeto é realmente desalocado não é determinístico, o que pode acarretar na variação do tempo de execução de algoritmo em partes aleatórias, o que impensável em sistemas como em tempo real, drivers de dispositivo e processamento de transações.<br>

Muitas linguagens exigem a coleta de lixo - De exemplo posso usar o Java, C#.

Resumindo: O trabalho do **Garbage Collector** se resume em vasculhar toda sua memória procurando por lixos. E isso é bastante prejudicial e ocupa muita memória.A estrutura do Garbage Collector dirá o melhor momento para ele coletar o lixo que está na memória.<br>

## Forçando o Garbage Collector
O Garbagge Collector ainda nos permite gerencia-lo de modo que em determinado momento podemos ordena-lo a fazer uma coleta de lixo.<br>
Caso em determinado momento dentro do nosso programa já tivéssemos usado bastante a memória, manipulado bastante objetos, criando diversos objetos, e quiséssemos forçar a coleta de lixo, ou que o Garbage Collector comece a trabalhar de maneira a liberar a memória ocupada por sujeira, poderíamos utilizar o método estático `Collect()` situado dentro do namespace `System` da classe `GC`.<br>
Faça isso utilizando o código abaixo:
```JS
System.GC.Collect();
```

## Mais um exemplo:
```JS
var miado = "meow";
console.log(miado);
```

Quando eu declarei a variável `miado` e automaticamente, estamos alocando um espaço.
Para isso, FAÇA MANUALMENTE!
```JS
meow = null;
```

## Mark and Sweep
O que diabos é o Mark and Sweep?
É a definição baseada de que um objeto não é mais necessário quando é inacessível.<br>
a vez em que o coletor de lixo mark-and-sweep termina marcando todos os valores alcançáveis, ele começa sua fase de varredura. Durante esta fase, aparece a lista de todos os valores do meio ambiente e desaloca qualquer que não são marcadas. Clássicos coletores de lixo Mark-and-Sweep fazem uma marca completa e uma varredura completa de uma só vez, o que provoca uma desaceleração perceptível no sistema durante a coleta de lixo.

## Counting Reference
Quando um objeto é criado, uma referência a ele é armazenado em uma variável com a contagem de referência do objeto. Quando a referência para o objeto é copiado e armazenado em outra variável, a contagem de referência é incrementado para dois. Quando uma das duas variáveis que detém essas referências é substituído com algum novo valor, a contagem de referência do objeto é diminuída de volta a um.<br>
As variações mais sofisticadas sobre o algoritmo se tornam um processo relativamente eficiente e realiza a coleta no fundo, sem interromper o desempenho do sistema.<br>

Exemplo:
```JS
var escola = "Webschool.io"; 
var nome = escola.toUpperCase();
escola = nome;
```
Após esse código ser executado, a sequência original da variável `escola` não é mais alcançável.<br>
O sistema detecta esse fato e libera seu espaço de armazenamento na memória para reutilização.<br>

## Ciclos
A falha básica com Reference Counting tem a ver com `referências cíclicas`.<br>
O ciclo inteiro pode ser lixo, se não há nenhuma maneira para se referir a qualquer um desses objetos de um programa, mas porque todos eles se referem um ao outro, Este problema com ciclos é o preço que deve ser pago para um esquema de coleta de lixo simples. A única maneira de evitar esse problema é através de uma intervenção manual.<br>
Para permitir um ciclo de objetos a serem lixo coletado, você deve quebrar o ciclo.<br>
Você pode fazer isso escolhendo um dos objetos no ciclo e definindo a propriedade dele que se refere ao próximo objeto para nulo.<br>

Sempre se preocupe com isso, para quem desenvolve games usando o HTML5. Isso é bem importante para o player ter uma ótima experiência com seu jogo e seu código cria muitos lixos, você vai ter problemas com isso.

## Fontes:
[Gerenciamento de Memória](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Memory_Management#Garbage_collection)
[Memory Leaks](http://javascript.info/tutorial/memory-leaks)
[Desmitificando o Garbage Collector](http://www.devmedia.com.br/desmistificando-o-garbage-collector/5451#ixzz3z9bl0BvE)

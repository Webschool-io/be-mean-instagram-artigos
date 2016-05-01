# Artigo - Garbage Collector
**Autor**: Guilherme Henrique Escarabel Silva - [oescarabel](https://www.github.com/oescarabel)
**Data**: 1462105522190


Quando estamos desenvolvendo, criamos nossas inumeras: *variáveis*,*funções* e definimos seus comportamentos no nosso ecossistema. Mas já parou para pensar na quantidade de declarações, inicializações e referências que realizamos na nossa aplicação? 

>Nunca pensou que aquela função complexa que você usou somente uma vez poderia estar atrapalhando o desempenho do seu sistema por estar ativa na memória? 

>Já percebeu que não alocamos memória explicitamente em linguagens de médio/alto nível?(como em linguagens de baixo nível como o C,C++ e/ou Objective-C).

Antes de chegarmos onde realmente precisamos neste artigo, vamos á um exemplo para podermos nos conceiturar melhor:

Imagine que acabou de comprar um guarda-roupa novinho em folha, e com o tempo você vai colocando:
- Roupas
- Perfumes
- Barbeadores 
- Pentes
- Alguns cabos
- Entre outras coisas

Vai começar a encher e ficar tudo bagunçado, seus perfumes irão acabar, suas roupas começarão a ficar velhas, e eventualmente com o uso constante dos objetos do guarda-roupa, irá se acumular sujeiras, papeladas, caixas(acredite,meu guarda-roupa é o reflexo 2* Guerra Mundial). Só que um dia você chega em casa e descobre que sua mulher arrumou todo seu guarda-roupa(toda aquela sujeira se foi,seu guarda-roupa voltou a brilhar e ficou mais rápido achar as coisas).

Imagine que: 
- O guarda-roupa é o nosso sistema
- Os objetos são as variáveis e as funções
- Sua mulher é o coletor de lixo

Tudo o que nós fazemos na computação envolve:
-Entrada
-Processamento
-Saída

Tudo o que fica em processamento significa que foi preciso alocar recursos da nossa máquina para poder realizar aquela tarefa, e quando ela vai para saída, aquele processo sai, ou seja deixa de consumir recursos. Então se nós declaramos uma função e a utilizamos para realizar uma ação, não deveriamos ter de retira-lá da memória(já que não estamos mais a utilizando)?.

Em linguagens de médio e/ou alto não precisamos nos preocupar com isso(mas deveriamos), pois elas nos trazem um recurso incrível chamado **Garbage Collector**, em sua definição:

>Coletor de lixo (em inglês: garbage collector, ou o acrônimo GC) é um processo usado para a automação do gerenciamento de memória. Com ele é possível recuperar uma área de memória inutilizada por um programa, o que pode evitar problemas de vazamento de memória, resultando no esgotamento da memória livre para alocação. - **Wikipédia**

Só que o coletor de lixo não é algo simples, vamos entender o porque.

O conceito de coletor de lixo nas linguagens de programação já é bem amadurecido(mas ainda não o suficiente, veremos mais a frente o porque)


# Mark and Sweep and Compact
Não vamos explicar todo aquele conceito chato, vamos direto ao que realmente importa, oque ele faz, e o melhor, vamos dividir por etapas! ;)

### Primeira Etapa - Mark
Ele varre todos os objetos alocados na memória(do nosso sistema, dãh) e começa a visitar todos os objetos que fazem referências as raizes(window), e vai descendo grau por degrau, até a última referência

### Segunda Etapa - Sweep
Todos os objetos que não foram visitados são marcados como lixo, para que na próxima vez que o *coletor de lixo* for chamado, ele remova estes objetos marcados, assim, a memória ocupada por estes objetos serão liberados

### Terceira Etapa - Compact
Na memória nossas informações são gravadas em posições, pense em um array que vai de 0 á 99, e nosso sistema ocupou algumas posições, mas em determinado momento o coletor de lixo faz o sue trabalho e deixa os espaços que foram considerados lixos em branco, desta forma, a compactação reordena nossa memória.

Entretanto este meio é pesaroso, pois comsome muito desempenho quando ativado. Para isto, a Google melhorou a forma do coletor de lixo trabalhar


# Mark Incremental and Lazy Sweep

Como o problema do modo anterior era o alto custo de desempenho de uma única vez, resolveram fatiar a pizza. O processo de marcação é feito pedaço por pedaço, ou seja, divide o processo e faz um, pausa, faz outro, pausa(pausas que variam de 5-10ms).

**Obs:** como é mais demorado este processo, pode acontecer de a engine do javascript ficar sem memória para terminar o processo de limpeza, desta forma, ele deixa de fazer este conceito incremental e faz o **Mark and Sweep** padrão para que haja como trabalhar.

Após o término da marcação, fica permitida a entrada da varredura onde ela sabe exatamente onde estão os objetos marcados ou não, e faz este processo aos poucos, após o término já se pode começar novamente o processo de marcação caso seja necessário.


# Mas porque melhorar e entender o Coletor de lixo?
Atualmente em muitos sites como Facebok, Google, Twitter(entre outros), não mudam de páginas quando acessamos seus recursos. 
> Sim, é uma única página principal onde ela vai sendo atualizada com recursos que você solicitou, e não da forma que estamos acostumados onde tem de mudar de página, tendo que recarregar, tudo novamente.

Desta forma, se não mudamos de página, não damos refresh na página inteira, e só vamos solicitando recursos, isto vai só enchendo a memória, por isto que o coletor de lixo é essencial para aplicações de **Single Page Applications**

















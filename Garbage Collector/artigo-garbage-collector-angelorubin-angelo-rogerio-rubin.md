# Artigo - Garbage Collector
**Autor**: Angelo Rogério Rubin - [angelorubin](https://github.com/angelorubin)
**Data**: // execute o Date.now() e coloque o timestamp aqui

O JavaScript utiliza o garbage collector (coleta de lixo) para recuperar a memória ocupada por strings, objetos, matrizes e funções (que não estão mais em uso). Consequentemente isso ajuda os programadores a não terem que fazer este trabalho manualmente, como é feito na linguagem C.

Uma característica fundamental do garbage collector é a capacidade de determinar quando é seguro recuperar memória. Obviamente, ele nunca deve recuperar valores que ainda estão em uso e deve remover apenas os valores que não são mais acessíveis, ou seja, valores que não podem ser referenciados através de quaisquer variáveis, objetos e matrizes no programa.

Mas como o garbage collector faz esta distinção entre o que pode ser descartado e o que ainda está sendo utilizado?

O garbage collection atravessa periodicamente a lista de todas as variáveis no ambiente JavaScript e marca todos os valores referidos por estas variáveis. Se algum dos valores de referência são objetos ou arrays, ele recursivamente marca estes elementos. De forma recursiva percorre a árvore com os valores, o garbage collection é capaz de encontrar (e marcar) cada valor único que ainda está acessível. Portanto os valores não marcados são inacessíveis e desta forma podem ser descartados da memória.

Alguns algoritmos de garbage collector clássicos fazem a marcação e varredura completa de uma só vez, ocasionando uma desaceleração perceptível no sistema durante a sua realização.

Variações mais sofisticadas destes algoritmos tornam o processo relativamente eficiente e realizam a coleta de lixo em segundo plano, desta forma não prejudicando o desempenho do sistema.


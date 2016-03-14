# Artigo - Garbage Collector
**Autor**: Josimar Zimermann - [josimarz](https://github.com/josimarz/)
**Data**: 1457193928889

# Garbage Collector

O JavaScript usa o _Garbage Collector_ (coletor de lixo, em português) para recuperar a memória ocupada por _strings_, objetos, _arrays_ e funções que não estão mais em uso.

A JavaScript implementa dois métodos diferentes para _Garbage Collector_. Esses métodos são explicados nos próximos tópicos.

## Mark-and-Sweep Garbage Collection

A expressão _Mark-and-sweep_ pode ser traduzido como "rotulação e varredura". Em termos simples, este algoritmo realiza leitura do código JavaScript, encontrando e rotulando (_mark_) todos os valores acessíveis. Os valores inacessíveis não são rotulados e, portanto, são varridos (_weep_) da memória, isto é, são considerados lixo.

Para realizar a leitura, este algoritmo mantém uma estrutura do tipo grafo contém referências para todos os objetos e valores de um programa JavaScript. A raíz deste grafo é um objeto especial denominado _Garbage Collector Root_, ou simplesmente _GC Root_. O objeto _GC Root_ mantém referência para os demais objetos e valores do programa JavaScript, direta ou indiretamente. Observe uma representação gráfica da estrutura na imagem abaixo.

![Object Graph](http://www.html5rocks.com/en/tutorials/memory/effectivemanagement/images/image04.png)

Percorrendo o grafo de objetos, o algoritmo irá procurar por objetos e valores inacessíveis. São considerados objetos e valores inacessíveis aqueles que não podem ser alcançados a partir do objeto _GC Root_. A próxima imagem destaca um objeto e um valor inalcançáveis, isto é, que não são referenciados pelo objeto _GC Root_.

![Unreachable Object](http://www.html5rocks.com/en/tutorials/memory/effectivemanagement/images/image03.png)

Os dois valores destacados na imagem anterior serão varridos (_sweep_) da memória.

Coletores de lixo clássicos realizam um processo de _mark-and-sweep_ completo em uma só vez, provocando notável lentidão no sistema durante a coleta de lixo. Variações sofisticadas deste algoritimo fazem o processo relativamente mais eficiente e executam a coleta de lixo em segundo plano, sem prejudicar o desempenho do sistema.

Todas as implementações modernas de JavaScript, a partir de 2012, usam algum tipo de coletor de lixo _mark-and-sweep_. No entanto, JavaScript 1.1, como implementado no Netscape 3, usou um esquema de coleta de lixo um pouco mais simples e que possui algumas deficiências.

## Garbage Collector por contagem de referência

No JavaScript 1.1, como implementado no Netscape 3, a coleta de lixo é executada por contagem de referência. Isto significa que cada objeto (pode ser um objeto criado pelo usuário ou um objeto HTML criado pelo navegador) mantém um controle do número de referências para si.

Quando um objeto é criado, uma referência para ele é armazenada numa variável, o contador de referência para o objeto é um. Quando a referência para o objeto é copiada e armazenada em outra variável, o contador de reerências é incrementado para dois. Quando uma das duas variáveis que mantém essas referências é sobreescrita com um novo valor, o contador de referências para o objeto é decrementado para um. Se o contador de referências alcança zero, não há mais referências para este objeto. Uma vez que o objeto não possui mais referências, significa que ele está inacessível. Portanto, a JavaScript sabe que é seguro destruir este objeto e coletar a memória por ele então ocupada.

Este método de coleta de lixo é bastante simples e de fácil implementação. Contudo, apresentada uma falha básica quando usa-se *referências cíclicas*. Se um objeto `A` contém uma referência para o objeto `B` e o objeto `B` contém uma referência para o objeto `A`, temos um ciclo de referências. Um ciclo pode também existir se, por exemplo, `A` é referenciado por `B`, `B` é referenciado por `C` e `C` é referenciado por `A`. Nestas situações, o contador de referências dos objetos nunca será inferior a um e, portanto, os objetos nunca serão coletados. No segundo exemplo, talvez os três objetos não possuam mais quaisquer referências externas. Contudo, como eles se referenciam entre si o contador de referências não é capaz de saber se eles podem de fato ser considerados lixo.

A única forma de previnir este problema é através da intervenção manual. Deve-se quebrar o ciclo para permitir que os objetos sejam coletados. Isto pode ser realizado por escolher um dos objetos do clico e alterar a propriedade dele que referencia outro objeto no ciclo para `null`. Por exemplo, suponha que `A`, `B` e `C` são objetos e cada um possui uma propriedade denominada `next`. No objeto `A`, `next` possui uma referência para `B`. No objeto `B`, `next` possui uma referência para o objeto `C`. Por fim, no objeto `C`, `next` possui uma referência para o objeto `A`. Portanto, temos um ciclo de referências. Quando estes objetos não estão mais em uso, o ciclo pode ser quebrado alterando a propriedade `next` de um dos três objetos para `null`.

No entanto, a coleta não irá ocorrer se os objetos `A`, `B` e `C` estão armazenados em variáveis globais em uma janela que está aberta. Contudo, se os três objetos são referenciados apenas por variáveis locais dentro de uma função por exemplo, se o ciclo é quebrado e a função finalizada, a coleta irá acontecer. Se os objetos estão armazenados em variáveis globais, a coleta somente irá ocorrer quando a janela que contém suas referências for fechada. Neste caso, é possível forçar a coleta dos objetos quebrando o ciclo e modificando as variáveis para `null`:

```js
A.next = null;     // Quebra o ciclo de referências
A = B = C = null ; // Remove todas as referências
```

# Execução de funções

A JavaScript fornece quatro métodos diferentes para execução de funções. Cada método difere em como `this` é inicializada.

## A palavra chave `this`

Na JavaScript, *`this`* é o objeto que possui o código corrente. O seu valor, quando usado numa função, é o objeto que possui a função.

Note que *`this`* não é uma variável. É uma palavra-chave. Portanto, seu valor não pode ser modificado.

Os próximos tópicos abordam as diferentes forma que uma função JavaScript pode ser executada.

## Invocando uma Função como um Função

```js
function myFunction (a, b) {
    return a * b;
};
myFunction(10, 2);
```

A função acima não pertence a qualquer objeto. Mas, em JavaScript, há sempre um objeto global padrão.

No HTML o objeto global padrão é a própria pa'gina HTML, de modo que a função acima pertence à página HTML.

Num navegador, o objeto da página é a janela do navegador. A função acima torna-se automaticamente uma função de janela. Portanto, `myFunction()` e `window.myFunction()` são a mesma função:

```js
function myFunction (a, b) {
    return a * b;
};
window.myFunction(10, 2);
```

Concluímos, portanto, que no exemplo apresentado, a palavra-chave `this` contém uma referência para a janela do navegador que contém a função.

## Invocando uma Função como um Método

Na JavaScript é possível definir funções como métodos de objetos.

O exemplo a seguir cria um objeto denominado `myObject`, com duas propriedades (`firstName` e `lastName`) e um método (`fullName`):

```js
var myObjetct = {
    firstName: "John",
    lastName: "Doe",
    fullName: function () {
        return this.firstName + " " + this.lastName;
    }
}
myObject.fullName();
```

O método `fullName` é uma função que pertence ao objeto `myObject`. No exemplo, `this` é o objeto que contém o código JavaScript. Neste caso o valor de `this` é `myObject`.

## Invocando uma Função com uma Função Construtora

Se a chamada de uma função é precedida pela palavra-chave `new`, significa que é a invocação de um construtor.

Na JavaScript funções são objetos. Observe o exemplo abaixo:

```js
function myFunction (arg1, arg2) {
    this.firstName = arg1;
    this.lastName = arg2;
}

var x = new myFunction ("John", "Doe");
x.firstName;
```

No contrutor, a palavra-chave `this` não possui valor. O valor para `this` será o novo objeto criado quando a função for invocada.

## Invocando uma Função como um Método da Função

Como mencionado no tópico anterior, na JavaScript, funções são objetos. Portanto, funções tem propriedades e métodos.

Os métodos `call()` e `apply()` são predefinidos e estão sempre presentes numa função. Ambos podem ser usados para invocar uma função e ambos devem receber um objeto proprietário como primeiro parâmetro.

Exemplo 1, usando o método `call()`:
```js
function myFunction (a, b) {
    return a * b;
};
myObject = myFunction.call(myObject, 10, 2);
```

Exemplo 2, usando o método `apply()`:
```js
function myFunction (a, b) {
    return a * b;
}
myArray = [10, 2];
myObject = myFunction.apply(myObject, myArray);
```

Ambos os métodos enviam um objeto proprietário como primeiro argumento. A única diferença é que `call()` envia os argumentos separadamente, ao passo que `apply()` envia os argumentos num vetor.

Nos dois exemplos apresentados, o primeiro argumento tornar-se-á o valor de `this` para a função invocada, mesmo que o argumento não seja um objeto.

Usando `call()` ou `apply()`, se o primeiro argumento é `null` ou `undefined`, então ele é substituído pelo objeto global.

# Armazenamento de variáveis

Tal qual em outras linguagens de programação, uma variável JavaScript é uma referência para um espaço na memória reservado para armazenar um valor. O espaço necessário para armazenar um valor é definido com base no tipo de dado armazenado.

A JavaScript é uma linguagem de tipagem dinâmico. Isto significa que o tipo de dado contido numa variável é conhecido no momento que um valor é atribuído à essa variável. Portanto, é no momento da inicialização da variável que o motor da JavaScript realiza a alocação de memória.

## Números

Os valores numéricos são representados usando o formato de ponto flutuante de 64-bit. Portanto, os limites inferior e superior de um tipo número na JavaScript são 1.7976931348623157e+308 e 5e-324, respectivamente. Podemos comprovar estes valores imprimindo o conteúdo das constantes `MAX_VALUE` e `MIN_VALUE` da classe `Number`:

```js
console.log(Number.MAX_VALUE); // imprime 1.7976931348623157e+308
console.log(Number.MIN_VALUE); // imprime 5e-324
```

Todos os valores numéricos JavaScript ocupam 8 bytes (64-bit) na memória, independente do tipo numérico. Os valores numéricos da JavaScript são:
* Inteiro.
* Hexadecimal.
* Octal.
* Ponto flutuante.

## _Strings_

_Strings_ são cadeias de caracteres. Para cada caracter dentro de uma _string_, a JavaScript armazena dois bytes na memória. Portanto, o espaço ocupado por uma _string_ será a sua largura multiplicado por dois bytes. O exemplo de código abaixo mostra como obter o total de bytes ocupados por uma _string_.

```js
var myString = 'Josimar';
console.log(myString.length * 2); // myString ocupa 7 * 2 = 14 bytes
```

## Booleanos

Os valores booleanos `true` e `false` ocupam quatro bytes na memória.

## Objetos e vetores

Os objetos e vetores são coleções de tipos primitivos e outros objetos. Portanto, o tamanho total de um objeto ou vetor será a soma do tamanho dos objetos e variáveis contidos por este objeto.

# Fontes
* http://docstore.mik.ua/orelly/webprog/jscript/ch11_03.htm
* https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Memory_Management
* http://www.html5rocks.com/en/tutorials/memory/effectivemanagement/?redirect_from_locale=pt
* http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/
* http://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/
* http://www.w3schools.com/js/js_function_invocation.asp
* http://docstore.mik.ua/orelly/webprog/jscript/ch03_01.htm
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures
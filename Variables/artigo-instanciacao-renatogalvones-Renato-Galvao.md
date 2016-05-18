# Entendendo a criação e instanciação de variáveis no JavaScript
**Autor**: [Renato Galvão](https://github.com/renatogalvones)
**Data**: 1462906440777

O JavaScript é uma linguagem maravilhosa, porém tem suas particularidades assim como qualquer linguagem. Um programador desavisado pode passar muito tempo debruçado em um problema que para ele não faz sentido mas que faria caso ele entendesse como o JavaScript se comporta "por debaixo dos panos".
E é isso que vamos esclarecer abaixo:

---

## Hoisting

Hoisting em inglês significa içar, levantar, puxar pra cima. No JavaScript o que acontece é exatamente isso: um içamento das variáveis e funções para o topo do escopo para que sejam **declaradas** antes de sua utilização **não inicializadas**, ou seja, independente de onde a declaração das variáveis seja feita, elas serão içadas, declaradas e somente então poderão ser inicializadas.

Encontramos dois tipos de hoisting no JavaScript: de **variáveis** e de **funções**.

##### Hoisting de variáveis
As variáveis declaradas dentro do escopo serão içadas até o topo daquele escopo para serem criadas, veja os exemplos abaixo:

```javascript
// Exemplo 1 - Variável não foi definida

try {
    console.log(x)
} catch (e) {
    console.error('A variável x não foi definida.')
}

// Resultado: Exibe o erro pego no `catch`
```
```javascript
// Exemplo 2 - Variável definida porém não inicializada

try {
    console.log(x)
    var x = 10
} catch (e) {
    console.error('A variável `x` não foi definida.')
}

// Resultado: Exibe `undefined`
```

##### Hoisting de funções

No hoisting de funções, diferentemente do hoisting de variáveis, o bloco inteiro da função é içado para o topo do escopo.
Isso faz com que independente da linha em que for chamada a função realizará sua ação.
Vamos aos exemplos para que as coisas fiquem mais claras:

```javascript

// Exemplo 1 - Execução antes da declaração

teste();

function teste(){
  console.log('aloha!');
}

// Resultado: Irá printar no console a string 'aloha'
```
```javascript
// Exemplo 2 - Redefinição de funções

function teste(){
    function inside(){
      return 10;
    }

    return inside();

    function inside(){
      return 20;
    }

}

console.log(teste());

/* Resultado: Irá printar 20 pois como todas as funções são içadas para o topo do escopo
antes da execução a de baixo irá sobrescrever a de cima.*/
```
```javascript
// Exemplo 3 - Atribuição de função à variável

x();

var x = function teste(){
  console.log('o que vai acontecer?')
}

// Nesse exemplo, ainda que o conteúdo da variável seja uma função, será
// respeitada a regra que se aplica ao hoisting de funções, ou seja, a variável
// x será criada porém não inicializada, devolvendo um erro de 'not a function'
```

---

## Closure

"Qualquer função definida dentro de outra função pode acessar os argumento passados e variáveis da função de fora. Este relacionamento é conhecido como closure." - Em tradução livre de um trecho de um post do Ben Alman.

Essa frase é tão boa e tão esclarecedora que prefiro ir direto para os exemplos:

Abaixo vamos ver um exemplo de como fazer uma closure que recebe uma array e um multiplicador e que vai percorrer esse array usando o multiplicador em cada um desses indices:

```javascript
function multiply(arr) {
    return function (multiplier) {
        for (var i = 0; i < arr.length; i++) {
          arr[i] = arr[i] * multiplier;
        }
        return arr;
    }
}
var x = multiply([2,4,5,10]);
var y = multiply([50,-10]);

x(2)
y(2)

// A função multiply recebe o array e retorna uma outra função.
// Essa função retornada recebe o argumento multiplier e retorna o array com multiplicações.

// Resultado de x: [4, 8, 10, 20]
// Resultado de y: [100, -20]
```

No exemplo acima, vimos que a função anônima filha tem acesso ao array que foi passado para a função pai. Já a função pai e qualquer outra função externa não tem acesso à essas variáveis e isso que torna as closures tão interessantes.

---

## Variável Global

Variáveis globais são variáveis que pertencem ao escopo global no JavaScript.
No caso do JavaScript interpretado pelo browser todas as variáveis globais ficam no escopo global window, então você pode acessá-la de duas maneiras:

```javascript
var x = 1;

console.log(x);
console.log(window.x);
```

Se estiver trabalhando com NodeJs você poderá acessar as variáveis também de quatro maneiras:

```javascript
var x = 1;

console.log(GLOBAL.x);
console.log(global.x);
console.log(root.x);
console.log(x);
```

As variáveis globais são acessíveis à toda e qualquer função. Por esse motivo, recomenda-se muito cuidado(em algumas literaturas até evitar) a utilização das mesmas.

---

## Variável por parâmetro

As variáveis passadas por parâmetro são utilizadas para que as funções trabalhem com valores dinâmicos podendo ser variáveis do próprio escopo(chamado de escopo local) ou do escopo global.

Vamos ver primeiro um exemplo do escopo local:
```javascript
function minus(arg) {
    var total = 10;
    return total = total - arg;
}

var x = minus(3);
var y = minus(5);

console.log(x);
// 7
console.log(y);
// 5

```

No exemplo acima, vemos a variavel total sendo inicializada localmente. Logo, toda vez que é executada, a variável é criada novamente e seu valor setado para 10.

Vamos ao segundos exemplo, agora utilizando uma variável no escopo global:

```javascript
var total = 10;

function minus(arg) {
    return total = total - arg;
}

var x = minus(3);
var y = minus(5);

console.log(x);
// 7
console.log(y);
// 2
```

Entendeu o que aconteceu de diferente dessa vez?
A variável total, como está no escopo global, manteve valor 7 já que foi setado no retorno da primeira passagem da função minus.
Sendo assim, na segunda passagem, o valor de total não era 10 e sim 7, por isso o resultado 2.

---

## Instanciação usando uma IIFE

**Immediately-Invoked Function Expression**, o que pode ser traduzido livremente como **Função/Expressão Invocada Imediatamente**. O seu uso traz algumas vantagens bem interessantes. Um deles é do ponto de vista de desempenho pois realiza 3 ações de uma vez só:

- Cria uma instância da função.
- Executa a função.
- Descarta a função. A função criada era uma função anônima e depois de sua execução não resta nenhuma referência para a mesma.

Outra vantagem, e que podemos aplicar o *Module Pattern* no nosso código, o que o deixa extremamente elegante.

Antes de falar do código, prestem atenção aos detalhes da IIFE. Ela é uma função declarada via [Function Expression](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/function), e não pelo uso tradicional [Function Statement](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/function). A princípio, o entendimento da IIFE pode ser complicado, mas se atente aos parênteses que tudo fará sentido aos poucos e em caso de dúvida, substitua o conteúdo dos parênteses, deve facilitar seu entendimento.

Dada essa introdução, agora sim, vamos à um exemplo de IIFE:

```javascript
var counter = (function(){
    var i = 0;

    return {
        get: function(){
            return i;
        },
        set: function( val ){
            i = val;
        },
        increment: function() {
            return ++i;
        }
    };
}());

// `counter` é um objeto com propriedades, que neste caso são métodos.

counter.get(); // 0
counter.set(3);
counter.increment(); // 4
counter.increment(); // 5

counter.i; // undefined (`i` não é uma propriedade do objeto retornado)

i; // ReferenceError: i is not defined (ele só existe dentro da closure)
```

Lindo não? A idéia aqui é deixar somente os métodos e variáveis necessário acessíveis de fora da IIFE, neste caso o objeto com as funções *get*, *set* e *increment*.
Já a variável *i* digamos que não interessa à ninguém de fora, portanto seu uso é somente dentro da closure. Dessa forma, seu código fica legível, elegante e performático(toda vez q uso essa palavra imagino os caracteres dançando...rsrs, desculpem, foi mais forte que eu.)

---

Referências:

- [stackoverflow.com](http://stackoverflow.com/questions/15395347/does-a-browser-truly-read-javascript-line-by-line-or-does-it-make-multiple-passe)
- [LoopInfinito - hoisting-e-escopo-em-javascript](http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
- [http://benalman.com/news/2010/11/immediately-invoked-function-expression/](http://benalman.com/news/2010/11/immediately-invoked-function-expression/)
- [http://skilldrick.co.uk/2011/04/closures-explained-with-javascript/](http://skilldrick.co.uk/2011/04/closures-explained-with-javascript/)

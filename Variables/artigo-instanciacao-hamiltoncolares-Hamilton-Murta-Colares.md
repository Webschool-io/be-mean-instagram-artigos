# Artigo: Instanciação
**Autor:** Hamilton Murta Colares - [hamiltoncolares](https://github.com/hamiltoncolares)
**Data:**

## Hoisting
Para o JavaScript, todas as variáveis declaradas, em qualquer posição, tem sua declaração elevada ao topo do código (*Hoisting = elevar*).
```JavaScript
// Irá imprimir `undefined` por que a variável não foi instanciada antes de ser usada
try {
  var a
  console.log(a)
  a = 2
} catch (e) {
  console.error('a não existe no contexto atual')
}
```
Apesar de a declaração das variáveis ser elevada ao topo, essas varáveis não são instanciadas nesse momento. Portanto, é necessário instanviar a variável para que ela receba um valor válido.
```JavaScript
// Exemplo 8
// Irá imprimir `2`
var a = 2
try {
  console.log(a)
} catch (e) {
  console.error('a não existe no contexto atual')
}
```
Dessa forma, é importante procurar declarar sempre as variáveis no início do código, para evitar erros indesejados e que seriam facilmente evitados com essa prática.

## Closure
São variáveis que são instanciadas e se mantém vivas enquanto a aplicação estiver rodando.

```JavaScript
var counter = 0;

function add() {
    counter += 1;
}

add();
add();
add();

// Nesse ponto, 'counter' terá seu valor igual a 3.
```

## Variável global
Variáveis glogais são aquelas cujo escopo de uso se extende por todo programa. Por exemplo, funções podem fazer uso de variáveis globais, mesmo que elas não estejam declaradas dentro da própria função.

```JavaScript
var a = 4;
function myFunction() {
    return a * a;
}
// 'a' é uma variável global e pode usada pela função
```

## Viriável por parâmetro
São variáveis cujo escopo de uso está restrito ao escopo de sua declaração. Por exemplo, só podem usadas pela função onde foram declaradas.

```JavaScript
var a = 4;
function myFunction() {
    var b = 2;
    return a * b;
}
// 'b' só é vista dentro da função 'myFunction'
```

## Instanciação usando uma IIFE
Seu significado é "Immediately-invoked function expression", ou função imediata. Como em JavaScript as variáveis tem escopo de acordo com a função em que são criadas, esse recurso faz com que a função seja criada e executada imediatamente. Com isso, evitamos que ocorram possíveis conflitos de criação de variáveis durante o tempo de execução do programa. Isso evita que variáveis de escopo restrito a função puluam o escopo global.

```JavaScript
var adder = (function() {
 var myPhrase = "";
 return function(x) {
 return myPhrase =
 !!myPhrase ? myPhrase.concat(" ", x) : myPhrase.concat(x);
 }
})();
adder("Olá"); // "Olá"
adder("Mundo!"); // "Olá Mundo!"

myPhrase; // myPhrase is not defined
```

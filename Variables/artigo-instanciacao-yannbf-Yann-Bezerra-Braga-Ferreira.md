#Artigo - Javascript e suas Peripécias Instanciais
Autor: Yann Bezerra Braga Ferreira

### Hoisting
O Javascript possui duas fases: Criação e Execução. Na criação, um espaço na memória é utilizado pra declaração de funções e variáveis. Pelo fato das declarações no javascript serem processadas antes da execução do código, não importa onde você declarar uma variável, será o equivalente a declará-la no topo. Com isso, é possível manipular uma variável antes mesmo dela ser declarada. Na fase de execução é onde os valores são setados, por isso é bom tomar cuidado com o entendimento de hoistings.
Exemplo:
```
console.log(name);
var name = 'Julian';
//undefined
```
O valor resultante é undefined pois o javascript só 'leva pro topo' a declaração, e não a definição dos valores.
Forma como javascript se comporta:
```
var name;
console.log(name);
name = 'Julian';
//undefined
```

### Closures

Dado o contexto onde se tem uma função dentro de outra, como no exemplo:
```
function sayHi(name) {
    var phrase = 'Hello, ';    
    function speak() {
        console.log(phrase + name + '! ' + 'What a lovely day.');
    }   
    return speak();
}

sayHi('Yohanna');
//Hello, Yohanna! What a lovely day.
```
A função interna(speak) tem acesso à variáveis que foram definidas na sua função externa(sayHi), assim como passadas por parâmetro na função externa. Quando se tem esse tipo de situação, define-se uma closure. Este fenômeno é bem comum e amplamente utilizado em arquiteturas não-assíncronas.

### Variável global

Se uma variável for declarada fora de uma função, esta será acessível dentro de uma função, pois seu escopo será definido como global.

```
var name = "Stephen";


function askQuestion() {
    console.log(name + ", what is your greatest ambition?");
    // Stephen, what is your greatest ambition?
}
```

Caso uma variável tenha um valor definido e não tenha sido declarada, não importa onde ela estiver, esta será declarada como variável global. 
``` 
askQuestion();
console.log(name + ", what is your greatest ambition?");
// Stephen, what is your greatest ambition?

function askQuestion() {
    name = "Stephen";
}
```


### Variável por parâmetro

Quando a variável é passada por parâmetro, a mesma funciona em um escopo local, sendo visível e manipulável somente dentro do escopo daquela função.
``` 

sumFunction(3, 47);

function sumFunction(x, y) {
    var result = x + y;
    console.log(result);
    // 50
}

console.log(x + y);
// Uncaught ReferenceError: y is not defined(...)
```

## Instanciação usando uma IIFE

IIFE (Immediately-Invoked Function Expression) define um escopo privado, definido dentro de uma função que imediatamente será auto-invocada, fazendo com que as variáveis sejam visíveis e manipuláveis apenas dentro do contexto desta função. Isto ajuda na segurança e faz com que as variáveis não sejam acessadas fora do escopo daquela função.
```
(function(a, b) {
    console.log('Hey,' + a + ' ' + b + '!');
    //Hey, Dave Grohl!
})('Dave', 'Grohl');
console.log(a);
//Uncaught ReferenceError: a is not defined(...)
```
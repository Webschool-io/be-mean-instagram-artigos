# Artigo - Instanciação
**Autor**: Matheus José Krumenauer Weber - [matheusjkweber] (https://github.com/matheusjkweber)
**Data**: 1451168613825

## Hoisting
Para entender como funciona o Hoisting em Javascript, temos que ter em mente que o escopo do Javascript é diferente das linguagens convencionais, mesmo sendo considerada como uma linguagem que pertence a família C, o escopo da mesma é feito apenas através de funções.

Quando declaramos uma varíavel ou função em Javascript a mesma é elevada para o topo do escopo, o nome desse procedimento é Hoisting, e existe diversos tipos como:

1 - Variable Hoisting:
Toda vez que uma varíavel é definida, sua declaração é hoisted, mas não inicalizada, ou seja a declaração da variável vai para cima do escopo antes mesmo do código ser executado, mas esta variável não recebe nenhum valor e permanece como undefined.

Exemplo 1:
```
try {
  console.log(a)
  var a = 2
} catch (e) {
  console.error('A variável `a` não foi definida.')
}
```
Irá entrar no catch do tratamento de exceções.

2 - Function Hoisting
O function hoisting é quando uma função é levantada para o topo do escopo, mas nesse caso além do nome da definição, o corpo da mesma também é levantado.

Exemplo 2:
```
foo()
function foo() {
  console.log('bar')
}
```
O código irá imprimir bar.

## Closure
Closure em Javascript são funções aninhadas uma dentro da outra.

Exemplo 3:
```
function x(a1) {          // "x" tem acesso a "a"
    var a2;
    function y(b1) {      // "y" tem acesso a "a" e "b"
        var b2;
        function z(c1) {  // "z" tem acesso a "a", "b", e "c"
            var c2;
        }
    }
}
```

## Variável Global
Uma varíavel global em javascript é uma variável que foi declarada fora de uma definição de função e seu valor pode ser acessado e modificado em todo o seu porgrama.

Exemplo 4:
```
var a = 1
function x(a) {          // "x" tem acesso a "a"
    ...
}
```

## Variável por parâmetro
Uma variável declarada por parametro é quando a mesma é repassada para uma função, a mesma será instacializada como variável local e só existirá no escopo daquela função.

Exemplo 5:
```
var a = 1
function x(a) {          // "x" tem acesso a "a"
   	a = a+1;
    console.log(a);
}
x(a);
console.log(a);

```
- Irá imprimir 2 e 1.

## Instanciação usando uma IIFE
IIFE significa Immediately-Invoked Function Expression, e é quando uma função é executada no momento em que é definida.

Exemplo 6:
```
(function () {
  console.log('Olá')
}())

```
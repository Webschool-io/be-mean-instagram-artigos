# Instanciação de variáveis no Javascript
Autor: **Anderson Machado Lemos**

Neste artigo vamos abordar alguns temas importantes sobre a instanciação de variáveis em JavaScript, entre eles: hoisting, closure, variáveis locais e globais, e IIFE.

## Hoisting

O nome hoisting, elevação ou até mesmo içamento, é só um termo especificado, pois ele arranca as declarações até o topo do seu escopo.
Isso acontece porque o JavaScript não obriga você a declarar variáveis, permite que você defina as funções em qualquer lugar que você queira, o que lhe permite usar uma função antes de sua definição.

Exemplo:

``` js
var number = 10;

function init(){
    console.log(number);
    var number = 5;
}

init();

```

Por causa da forma como o JavaScript trata declarações e escopos, a saída deste script é undefined (!)

## Closure

Genericamente, a idéia central de uma closure é a de confinamento de uma função dentro da outra.

Exemplo:

``` js

function funcaoExterna(){
    alert("Função Externa");
    function funcaoInterna(){
        alert("Função Interna");
    }
}

```

## Variável global
A variável global é acessível tanto de dentro de uma função como também estando fora dela. O funcionamento é através de enpsulamento dos blocos, liberando ou não o acesso das variáveis. O acesso é iniciado de dentro para fora, caso haja uma variável com o mesmo nome dentro da função e outra fora o JS irá utilizar a variável de escopo local ao invés do global.

Exemplo:
``` js
a = 50;
alert(window.a)
```
## Variável por parâmetro
Funções podem receber valores para serem utilizados dentro de seu escopo, são os chamados "parâmetros". Quando uma variável global é passada como parâmetro a uma função, seu valor é concedido a uma variável local.

Exemplo:
````js
var band = "Angra";
function changeBand(new) {
    new = "Megadeth";
    return new;
}

changeBand(band); // Megadeth
console.log(band); // Angra
```
## IIFE

O IIFE significa Immediately-invoked function expression, também conhecido como função imediata. Como o nome sugere, ela é uma função que é executada imediatamente após a sua criação. Podemos utilizar IIFE para encapsular funções e variáveis, assim evitamos poluição no nosso escopo global e possíveis conflitos de variáveis ou funções com o mesmo nome. Instanciação usando uma IIFE pode ser utilizando passando parâmetros ou não. Veja os paranauê abaixo:

Exemplo IIFE sem parâmetros:
``` js
(function() {
    alert('be MEAN!!');
})();
```
Exemplo IIFE como parâmetros:
``` js
(function(param) {
    alert('O nome é ' + param);
})();
```
##Referências  Bibliográficas

- iMasters <*http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife*>
- w3Schools <*http://www.w3schools.com/js/js_hoisting.asp*>
- JavaScript - Guia do Programador - Mauricio Samy Silva

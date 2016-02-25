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


## Variável global

## Variável por parâmetro

## IIFE

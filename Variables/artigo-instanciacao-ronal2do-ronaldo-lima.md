# Artigo - Instanciação de Variáveis no Javascript
autor: **Ronaldo da Silva Lima**
email: **ronal2do@gmail.com**

Primeiramente, tentarei espor tudo que entendi sobre o assunto. 

Uma variável local pode ter o mesmo nome que uma variável global, mas é totalmente separada. A alteração do valor de uma variável não afeta a outra.Somente a versão local tem significado dentro da função na qual ela é declarada.

No JavaScript, as variáveis são avaliadas como se fossem declaradas no início do escopo no qual existam.Às vezes, isso resulta em comportamentos inesperados, como mostrado aqui.

Existem 4 maneiras de um nome entrar em um escopo em JavaScript:
Definido pela linguagem: todo escopo possui o this, e caso seja uma função, também o arguments.
Parâmetros de uma função: caso uma função seja chamada na forma foo(a, b), a e b entram no escopo da função.
Declaração de uma função: funções declaradas na forma function foo() {}.
Declaração de uma variável: variáveis declaradas como var bar.

Porém, para cada diferente método de entrada no escopo há diferença na ordem de resolução de nomes. Alguns podem ser resolvidos primeiro mesmo aparecendo ao fim do escopo, enquanto outros podem ter apenas seus nomes resolvidos, sem ter seus valores inicializados. E é esse comportamento não explicito de resolução de nomes e inicialização de valores do JavaScript que foi batizado como hoisting por [Ben Cherry](https://twitter.com/bcherry).

## Hoisting

Hoist em inglês significa levantar ou suspender algo através de um aparato... e  bláh bláh bláh...

No JS ocorre quando declaramos uma variável e ela é levada para o topo do escopo que é citado acima. Eu sei é mais fácil de ser compreendido do que entendido.

Toda vez que definimos uma variável el é uma hoisted - Nessa hora ela vai para cima antes mesmo de ser executada, e se não recebe nenhum valor ela ela permanece undefined
```js
var hoisted
undefined

hoisted = "olá"
"olá"
```

Aqui ocorreu um hoisting da declaração da variável 'hoisted'. 


## Closure

Closure sinceramente é uma daquelas coisas que vc sempre usou e nunca soube que tinha um nome, ao me preparar para este artigo, descobri o que era... Ok, então vamos direto ao ponto:

Exemplo de internet...

```js
function showName (firstName, lastName) { 
​var nameIntro = "Your name is ";
    // this inner function has access to the outer function's variables, including the parameter​
​function makeFullName () {         
​return nameIntro + firstName + " " + lastName;     
}
​
​return makeFullName (); 
} 
​
showName ("Michael", "Jackson"); // Your name is Michael Jackson 

```
Na closure vc cria uma função dentro do corpo de outra... no caso "showName" foi criado no topo do código e definido dentro da mesma ('function showName (firstName, lastName) { ...') mais foi chamado fora da própria função.


## Variável Global

Variáveis globais são variáveis definidas fora de qualquer função e podem ser usadas em qualquer local do código.

```js
var global = 'Olá, sou uma global';

function fglobal(){
	global;
}

fglobal();

alert(global);  
```


## Variável por parâmetro

Uma variável passada por parâmetro pode ser acessada somente dentro do escopo da própria função, mesmo que a variável seja uma Global, definimos no próximo exemplo  um novo valor e tentamos acessar esse novo valor note que não conseguimos.


```js
var global = 'Olá, sou uma global';

function fglobal(global){
	global = 'nova global';
}

fglobal();

alert(global);  

```

## Instanciação usando uma IIFE

Quando você cria uma função, ela não faz nada até que seja invocada. 

Como qualquer função normal, você pode, ao invocar a IIFE, passar parâmetros para ela. O motivo de passar parâmetros é também escopo. Você pode passar um parâmetro que está no escopo global, para que ele seja usado como uma variável local. Um exemplo bastante comum é o jQuery:

```js
;(function( $ ) {
  // ... Aqui, $ é local. :)
})( jQuery );

``
Assim você injeta como parâmetro o objeto global jQuery, e recebe na IIFE como $, localmente, que agora pode ser usado sem medo de conflitar com qualquer outra lib que utilizar $.


## Fontes
[https://msdn.microsoft.com/pt-br/library/bzt2dkta(v=vs.94).aspx](https://msdn.microsoft.com/pt-br/library/bzt2dkta(v=vs.94).aspx)
[http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/](http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/)
[http://www.ime.usp.br/~elo/IntroducaoComputacao/Funcoes%20passagem%20de%20parametros.htm](http://www.ime.usp.br/~elo/IntroducaoComputacao/Funcoes%20passagem%20de%20parametros.htm)
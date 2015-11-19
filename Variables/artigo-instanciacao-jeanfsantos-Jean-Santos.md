# Artigo
**autor**: Jean Santos

## Hoisting

**Oque é**: Sua declaração é “elevada” para o topo do escopo.

**Variavel**: Toda vez que uma variável é definida, sua declaração é hoisted, mas não sua inicialização. O que quer dizer que a declaração da variável vai para cima do escopo antes mesmo do código ser executado, mas esta variável não recebe nenhum valor e permanece como undefined.

```javascript
// Irá imprimir o erro dentro do `catch`
try {
  console.log(a)
} catch (e) {
  console.error('A variável `a` não foi definida.')
}

// Exemplo 7
// Irá imprimir `undefined`
try {
  console.log(a)
  var a = 2
} catch (e) {
  console.error('A variável `a` não foi definida.')
}
```

**Função**: O hosting com funções acontece de maneira diferente. Aqui, não só o nome da função é hoisted como também seu corpo.

```javascript
// Exemplo 9
foo()
function foo() {
  console.log('bar')
}
```


## Closure

Closures (fechamentos) são funções que se referem a variáveis livres (independentes).

Em outras palavras, a função definida no closure "lembra" do ambiente em que ela foi criada.

```javascript
function init() {
  var name = "Mozilla";
  function displayName() {
    alert(name);
  }
  displayName();
}
init();
```

A função init() cria uma variável local chamada name, e depois define uma função chamada displayName(). displayName() é uma função aninhada (um closure) — ela é definida dentro da função init(), e está disponivel apenas dentro do corpo daquela função.

Aqui temos um exemplo prático: suponha que queremos adicionar alguns botões para ajustar o tamanho do texto de uma página. Um jeito de fazer seria especificar o tamanho da fonte no elemento body e então definir o tamanho dos outros elementos da página (os cabeçalhos, por exemplo) utilizando a unidade relativa em:

```css
body {
  font-family: Helvetica, Arial, sans-serif;
  font-size: 12px;
}

h1 {
  font-size: 1.5em;
}
h2 {
  font-size: 1.2em;
}
```

Nossos botões interativos de tamanho de texto podem alterar a propriedade font-size do elemento body, e os ajustes serão refletidos em outros elementos graças à unidade relativa.

O código JavaScript:

```javascript
function makeSizer(size) {
  return function() {
    document.body.style.fontSize = size + 'px';
  };
}

var size12 = makeSizer(12);
var size14 = makeSizer(14);
var size16 = makeSizer(16);
```

size12, size14 e size16 agora são funções que devem redimensionar o texto do elemento body para 12.14 e 16 pixels respectivamente. Nós podemos designá-las à botões (neste caso, links) como feito a seguir:

```javascript
document.getElementById('size-12').onclick = size12;
document.getElementById('size-14').onclick = size14;
document.getElementById('size-16').onclick = size16;
```

```html
<a href="#" id="size-12">12</a>
<a href="#" id="size-14">14</a>
<a href="#" id="size-16">16</a>
```

## Variável Global

Uma variável declarada fora de uma definição de função é uma variável global, e seu valor será acessível e modificável em todo o seu programa.

Uma variável local pode ter o mesmo nome que uma variável global, mas é totalmente separada. A alteração do valor de uma variável não afeta a outra.Somente a versão local tem significado dentro da função na qual ela é declarada.

```javascript
// Global definition of aCentaur.
var aCentaur = "a horse with rider,";

// A local aCentaur variable is declared in this function.
function antiquities(){

   var aCentaur = "A centaur is probably a mounted Scythian warrior";
}

antiquities();
aCentaur += " as seen from a distance by a naive innocent.";

document.write(aCentaur);

// Output: "a horse with rider, as seen from a distance by a naive innocent."
```

## Variável por parâmetro

Parâmetros primitivos (como um número) são passados para as funções por valor; o valor é passado para a função, mas se a função altera o valor do parâmetro, esta mudança não reflete globalmente ou na função chamada.

Se você passar um objeto (ou seja, um  valor não primitivo, tal como Array ou um objeto definido por você) como um parâmetro e a função altera as propriedades do objeto, essa mudança é visível fora da função, conforme mostrado no exemplo a seguir:

```javascript
function minhaFuncao(theObject) {
  theObject.make = "Toyota";
}

var meucarro = {make: "Honda", model: "Accord", year: 1998},
    x,
    y;

x = meucarro.make;     // x recebe o valor "Honda"

minhaFuncao(meucarro);
y = meucarro.make;     // y recebe o valor "Toyota"
                    // (a propriedade make foi alterada pela função)
```


## Instanciação usando uma IIFE

IIFE ou Imediately Invoked Function Expression (Expressão de Função Invocada Imediatamente), no momento em que ela é interpretada, ela é imediatamente invocada, para que o seu conteúdo seja executado.

```javascript
;(function( win, undefined ) {
  return 'hi!';
})( window );
```

Como qualquer função normal, você pode, ao invocar a IIFE, passar parâmetros para ela. O motivo de passar parâmetros é também escopo. Você pode passar um parâmetro que está no escopo global, para que ele seja usado como uma variável local. Um exemplo bastante comum é o jQuery:

```javascript
;(function( $ ) {
  // ... Aqui, $ é local. :)
})( jQuery );
```

Assim você injeta como parâmetro o objeto global jQuery, e recebe na IIFE como $, localmente, que agora pode ser usado sem medo de conflitar com qualquer outra lib que utilizar $.


## Fontes:
* http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/
* https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures
* https://msdn.microsoft.com/pt-br/library/bzt2dkta(v=vs.94).aspx
* https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Fun%C3%A7%C3%B5es
* http://blog.da2k.com.br/2015/02/20/os-segredos-da-iife/

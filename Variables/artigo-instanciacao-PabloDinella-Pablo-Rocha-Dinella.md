# Instânciação de Variáveis em JavaScript
**autor**: Pablo Rocha Dinella

Este artigo visa explicar alguns conceitos relacionados à instanciação de variáveis em JavaScript.

## Hoisting

Comecemos com um exemplo:

```
bla = 2
var bla;
```
É implícitamente entendido como:
```
var bla;
bla = 2;
```
Isso é hoisting. O que aconteceu foi que declarações de variáveis (e declarações em geral) são processadas antes de que qualquer código seja executado. Declarar uma variável em qualquer parte do código é equivalente a declarar ela no topo. Então a declaração foi içada (*hoist* em inglês) para o topo.

Observe que o mesmo vale para funções:

```
hoisted(); // mostra "foo"

function hoisted() {
  console.log("foo");
}
```

Não vale porém, para uma *function expression*, que é quando uma função é definida sem nome, dentro de uma variável:

```
notHoisted(); // TypeError: notHoisted is not a function

var notHoisted = function() {
   console.log("bar");
};
```

Agora veja este exemplo:

```
var myVar = "Variável Global";
function myFunction() {
  console.log(myVar); // --> undefined
  myVar = "Variável Local";
  console.log(myVar); // --> Variável Local
}
```

A princípio eu achei que o primeiro console.log imprimiria "Variável Global", mas isso não aconteceu justamente por causa do *Hoisting*. Nesse caso o efeito *Hoisting* fez com

## Closure

Um *closure*, basicamente é a característica que permite que uma função dentro de outra possa acessar as variáveis deste última.

Exemplo:

```
function showName (firstName, lastName) {
	var nameIntro = "Your name is";

	//esta função filho tem acesso as variáveis da função pai, incluindo os parâmetros
	function makeFullName () {
		return nameIntro + " " + firstName + " " + lastName;
	}

	return makeFullName ();
}

showName ("Michael", "Jackson");
//Your name is Michael Jackson
```

No JavaScript, toda função é um closure. Um closure engloba o escopo pai. Então o escopo de uma função engloba o escopo global, permitindo que acessemos suas variáveis.

Então:

```
var x = 1;
function a() {
  x = 2; //muda a variável global x para 2
  var y = 3;
  function b() {
    y = 4; //y em a() vira 4
    var x = 5; //nova variável local
  }
  b();
}
a();
```

Após a execução desse código, `x` será `2` e `y` será indefinido, pois só existe localmente em `a()`. Se existisse uma função dentro de `a()` que ainda estivesse referenciando `y`, essa função ainda poderia acessá-lo.

## Variável Global

Quando uma variável global é inicializada, esta pode ser acessada por qualquer função, devido ao conceito de *closure* explicado no tópico anterior.

## Variável por parâmetro

Veja o seguinte código:

```
function changeStuff(a, b, c)
{
  a = a * 10;
  b.item = "changed";
  c = {item: "changed"};
}

var num = 10;
var obj1 = {item: "unchanged"};
var obj2 = {item: "unchanged"};

changeStuff(num, obj1, obj2);

console.log(num);       //10
console.log(obj1.item); //changed   
console.log(obj2.item); //unchanged
```
As variáveis guardam referências a objetos. O que aconteceu nesse código foi que, ao passar as variáveis `num`, `obj1` e `obj2`, a função `changeStuff` recebeu **como valor**, as referências que essas variáveis continham. Ao fazer `a = a * a`, a função atribuiu a `a` o valor `100`, porém não alterou nada em `num`. Na verdade, `a` recebeu uma **cópia** do conteúdo de `num`, que por sua vez continham uma referência ao espaço na memória que contém o valor `10`.

Já o trecho `b.item = "changed";` modificou a variável `obj1`, por quê? Porque `b` continha a mesma referência de `obj1`, então alterou o mesmo espaço na memória.

A variável `c` continha a mesma referência de `obj2`, mas ao realizar `c = {item: "changed"};` a função não alterou o espaço da memória referenciado por `c`. Apenas mudou a referência de `c` para outra referência, que aponta para um novo objeto `{item: "changed"}`, e como `c` é uma variável local da função, foi extinguida após sua execução.

## Instanciação usando uma IIFE

IIFE significa “Immediately-invoked function expression”, conhecida também como função imediata. Ela executa a função imediatamente depois de criada. Tem o seguinte formato:

```
(function(){...});
```

O primeiro par de parênteses transforma o código dentro deste em uma expressão. E o segundo par chama a função transformada em expressão pelo primeiro par.

Esse padrão é frequentemente utilizado para despoluir o escopo global, exemplo:

```
(function(){
    // all your code here
    var foo = function() {};
    window.onload = foo;
    // ...
})();
// foo is unreachable here (it’s undefined)
```

# Leitura extra

- http://javascriptbrasil.com/2013/10/11/escopo-de-variavel-e-hoisting-no-javascript-explicado/
- http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/
- https://developer.mozilla.org/

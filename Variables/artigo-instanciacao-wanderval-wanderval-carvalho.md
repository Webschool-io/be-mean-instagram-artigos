# Artigo
**autor**: Wanderval Carvalho

## Hoisting

Hoist em inglês significa levantar ou suspender algo através de um aparato mecânico. Em bom português, significa usar o guindaste para elevar um objeto. E é isto o que acontece em JavaScript quando declaramos uma variável ou função. Sua declaração é “elevada” para o topo do escopo.

#Variavel Hoisting
Toda vez que uma variável é definida, sua declaração é hoisted, mas não sua inicialização. O que quer dizer que a declaração da variável vai para cima do escopo antes mesmo do código ser executado, mas esta variável não recebe nenhum valor e permanece como undefined.

```javascript
// Exemplo 1
// Irá imprimir `undefined`
try {
  console.log(a)
  var a = 2
} catch (e) {
  console.error('A variável `a` não foi definida.')
}

// Exemplo 2
// Irá imprimir `undefined`
try {
  var a
  console.log(a)
  a = 2
} catch (e) {
  console.error('a não existe no contexto atual')
}
```
No exemplo 1, undefined será impresso, mesmo tendo a declaração de a depois do comando console.log. Ali ocorreu um hoisting da declaração da variável a, mas não da sua inicialização.
No exemplo 2, Repare que apenas a declaração da variável vai para o topo, mas não sua inicialização. Esta continua no mesmo lugar em que definimos no nosso código. Por isso recebemos um undefined quando tentamos acessar seu valor.

#Função Hoisting
O hosting com funções acontece de maneira diferente. Aqui, não só o nome da função é hoisted como também seu corpo.

```javascript
// Exemplo 3
foo()
function foo() {
  console.log('bar')
}
```
O código acima irá imprimir bar, sem nenhum erro. Mesmo executando uma função antes mesmo de ser definida. Isso porque tanto o nome da função como seu corpo são hoisted.


## Closure

Um closure é uma função interior que tem acesso a variáveis de uma função exterior – cadeia de escopo. O closure tem três cadeias de escopo: ele tem acesso ao seu próprio escopo (variáveis definidas entre suas chaves), ele tem acesso as variáveis da função exterior e tem acesso as variáveis globais.
A função interior tem acesso não somente as variáveis da função exterior, mas também aos parâmetros dela. Note que a função interior não pode chamar o objeto arguments da função exterior, entretanto, pode chamar parâmetros da função externa diretamente.
Você cria um closure adicionando uma função dentro de outra função.

```js
// Exemplo 4
function showName (firstName, lastName) {
    var nameIntro = "Your name is ";
 
    //esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
    function makeFullName () {
        return nameIntro + firstName + " " + lastName;
    }
 
    return makeFullName ();
}
 
showName ("Michael", "Jackson"); //Your name is Michael Jackson
```
Closures são usados extensivamente em Javascript sendo assíncrono, com arquitetura não bloqueante. Closures também são frequentemente usados no jQuery e em todo pedaço de código JavaScript que você lê.

```jquery
// Exemplo 5
$(function () {
    var selections = [];
    $(".niners").click(function () { //este closure tem acesso as variáveis de selections
        selections.push(this.prop ("name")); //atualiza a variável selection no escopo da função exterior
    });
});
```

#Utilidade
Existem muitas vantagens em se usar closures. Um exemplo seria simular variáveis privadas - algo que não é normalmente suportado pela linguagem JavaScript:
```js
// Exemplo 6
function MeuObjeto() {
    this.publico = { ... }
    var privado = { ... }
    this.foo = function() {
        return privado.a;
    }
    this.bar = function(x) {
        privado.a = x;
    }
    ...
}
var obj = new MeuObjeto();
obj.publico; // Acessível
obj.privado; // undefined
```

## Variável Global

Todas as variáveis declaradas fora de uma função estão no escopo global. No navegador, que é onde estamos interessados como desenvolvedores front-end, o contexto global ou escopo é o objeto window (ou o documento HTML inteiro).
Qualquer variável declarada ou inicializada fora de uma função é uma variável global, e estará portanto disponível para toda a aplicação.

```js
// Exemplo 7
//Para declarar uma variável global, você pode utilizar qualquer dos seguintes métodos:
var myName = "Richard";
 
//ou mesmo
firstName = "Richard";
 
//ou
var name;
name;
```
É importante notar que todas as variáveis globais são anexadas no objeto window. Então, todas as variáveis globais que nós declaramos podem ser acessadas pelo object window

```js
// Exemplo 8
console.log(window.myName); //Richard;
 
//ou
console.log("myName" in window); //true
console.log("firstName" in window); //true
```

## Variável por parâmetro

Quando uma função altera o valor de uma variável global, isso afeta toda a aplicação. Por isso o uso de variáveis globais não é considerado uma boa prática. Porém, uma variável global passada por parâmetro para uma função não tem o seu valor alterado:

```js
// Exemplo 9
var x = 1;
var y = 11;

function meh(x) {
	console.log("Dentro: ", x, y);
	x++;
	y++;
	console.log("Dentro: ", x, y);
}
meh(x);

// Dentro:  1 11
// Dentro:  2 12
console.log("Fora: ", x, y);
// Fora:  1 12
```

## Instanciação usando uma IIFE

O IIFE significa “Immediately-invoked function expression”, mas normalmente chamada de função imediata.
Ao criar uma função anônima com execução imediata, podemos criar um escopo temporário para nossas funções e variáveis. Com isso, evitamos poluição no nosso escopo global e possíveis conflitos de variáveis ou funções com o mesmo nome.

```js
// Exemplo 10
var adder = (function() {
 var myPhrase = "";
 return function(x) { 
 return myPhrase = 
 !!myPhrase ? myPhrase.concat(" ", x) : myPhrase.concat(x);
 }
})();
adder("Olá"); // "Olá"
adder("Mundo!"); // "Olá Mundo!"
myPhrase;
```
Neste exemplo, criamos uma função anônima imediata que retorna uma outra função que concatena uma string na variável chamada myPhrase. Note que a variável criada myPhrase está no escopo da IIFE e não na função retornada.
Portanto, o myPhrase não é definido toda vez que invocamos a função adder. Mas o mais importante disso tudo é que o myPhrase está limitado a escopo da função anônima imediata, não permitindo o seu acesso direto de maneira alguma.

#Formas de Declaraçôes
```js
// Exemplo 11
var fn = function () { return 'oi' }(); // Funciona
var fn = (function () { return 'oi' }()) // Funciona e é validado pelo JSLint 
var fn = (function () { return 'oi' })() // Também funciona, mas não é validado pelo JSLint

```
Isso é tudo a respeito de evitar ao máximo criar variáveis globais no JavaScript, e criar escopos quando e onde precisarmos.
A maior vantagem de usar IIFEs, é porque as IIFE faz com que todo o código que está em volta do seu script não tenha acesso as variáveis que você define.
Você deve usar uma IIFE sempre que surgir a necessidade de isolar as variáveis que você precisa do escopo global.

## Considerações

Quanto mais explicado melhor.

Lembre que isso fará parte do seu currículo como aluno e será disponilizado no sistema de vagas, ou seja, o contratante poderá ver todos seus projetos e trabalhos feitos nesse curso.

Boa sorte.

## Referências
1-http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/
2-http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/
3-http://pt.stackoverflow.com/questions/1859/como-funcionam-closures-em-javascript
4-http://javascriptbrasil.com/2013/10/11/escopo-de-variavel-e-hoisting-no-javascript-explicado/
5-http://imasters.com.br/desenvolvimento/programacao-funcional-com-javascript/?trace=1519021197&source=single



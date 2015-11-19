# Artigo
**Autor**: Jefferson Andrade Agostinho

Antes de começar a falar de Hoisting vou falar um pouco de Escopo

Para trabalhar com JavaScript de forma eficiente, uma das primeiras coisas que precisamos entender é o conceito de escopo de variáveis. O escopo de uma variável é controlada pela posição da declaração da variável, e define a parte do programa onde uma variável particular está acessível.

O JavaScript tem dois escopos - globais e locais. Qualquer variável declarada fora de uma função pertence ao âmbito global, e é, portanto, acessível de qualquer lugar no seu código. Cada função tem seu próprio âmbito de aplicação, e qualquer variável declarada dentro dessa função só é acessível a partir dessa função e quaisquer funções aninhadas. Porque escopo local em JavaScript é criado por funções, ele também é chamado de escopo de função. Quando colocamos uma função dentro de outra função, então nós criamos escopo aninhado.

Atualmente, JavaScript, ao contrário de muitas outras linguagens, não suporta bloco nível de escopo. Isto significa que declarar uma variável dentro de uma estrutura de bloco como um loop for, não restringe a essa variável para o loop. Em vez disso, a variável será acessível de toda a função.

Nós já sabemos que qualquer variável declarada dentro de um escopo pertence a esse escopo. Mas o que nós não sabemos ainda, é que não importa onde as variáveis são declaradas dentro de um escopo particular, todas as declarações de variáveis são movidos para o topo de seu escopo (global ou local). Isto é chamado de elevação, como as declarações de variáveis são elevadas (içadas) para o topo do escopo. Vamos ver um exemplo:

```
var locales = {
  europe: function(){           // The Europe continent's local scope
    var myFriend = "Monique";

    var france = function(){    // The France country's local scope

      var paris = function(){   // The Paris city's local scope
        console.log(myFriend);
      };

      paris();
    };

    france();
  }
};

locales.europe(); // output: Monique
```

No exemplo anterior, meu amigo Monique é representado pela Myfriend variável. Na última linha nós chamamos a função europa (), que chama france () e, finalmente, quando a função de paris () é chamado, a busca começa. O interpretador JavaScript funciona do âmbito atualmente em execução e trabalha-lo sair até encontrar a variável em questão. Se a variável não for encontrado em qualquer âmbito, em seguida, uma exceção é lançada.

No JavaScript, variáveis com o mesmo nome pode ser especificado em múltiplas camadas de escopo aninhado. Nesse caso, as variáveis locais tenham prioridade em relação as variáveis globais. Se você declarar uma variável local e uma variável global com o mesmo nome, a variável local terá prioridade quando você usá-lo dentro de uma função. Este tipo de comportamento é chamado de shadowing.

Esse é o mecanismo exato usado quando um interpretador de JavaScript está tentando encontrar uma variável particular. Ele começa no escopo interno que está sendo executado no momento, e continuar até a primeira correspondência for encontrada, não importa se há outras variáveis com o mesmo nome nos níveis exteriores ou não. Vamos ver um exemplo:

```
var test = "I'm global";

function testScope() {
  var test = "I'm local";

  console.log (test);
}

testScope();           // output: I'm local

console.log(test);     // output: I'm global
```

Como podemos ver, mesmo com o mesmo nome da variável local não substitui o global após a execução da função testScope (). Mas isso nem sempre é verdade. Vamos considerar o seguinte:

```
var test = "I'm global";

function testScope() {
  test = "I'm local";

  console.log(test);
}

console.log(test);     // output: I'm global

testScope();           // output: I'm local

console.log(test);     // output: I'm local (A variável global é reatribuída)
```

Desta vez, o teste variável local substitui a variável global com o mesmo nome. Quando executamos o código dentro da função testScope() a variável global é reatribuída. Se uma variável local é atribuído sem primeiro ter sido declarado com a palavra-chave var, ela se torna uma variável global. Para evitar esse tipo de comportamento indesejado você deve sempre declarar as variáveis locais antes de usá-los. Qualquer variável declarada com a palavra-chave var dentro de uma função é uma variável local. É considerada a melhor prática para declarar suas variáveis.

## Hoisting

Um interpretador JavaScript executa muitas coisas por trás, e um deles é chamado de Hoisting (elevação). Se você não está ciente deste comportamento "escondido", pode causar muita confusão. A melhor maneira de pensar sobre este comportamento de variáveis JavaScript para visualizá-los é sempre como consistindo de duas partes: uma declaração e uma atribuição:

```
var state;             // variable declaration
state = "ready";       // variable definition (assignment)

var state = "ready";   // declaration plus definition
```

No código acima, primeiro declarar estado variável, e, em seguida, atribuir o valor "pronto" para ele. E na última linha de código, vemos que estes dois passos podem ser combinados. Mas o que você precisa ter em mente é, mesmo que eles parecem ser uma declaração, na prática, o motor de JavaScript que trata única declaração como duas declarações separadas, assim como nas duas primeiras linhas no exemplo.

Nós já sabemos que qualquer variável declarada dentro de um escopo pertence a esse escopo. Mas o que nós não sabemos ainda, é que não importa onde as variáveis são declaradas dentro de um escopo particular, todas as declarações de variáveis são movidos para o topo de seu escopo (global ou local). Isto é chamado de elevação, como as declarações de variáveis são içadas para o topo do escopo. Note-se que só se move içar a declaração. Todas as atribuições são deixados no local. Vamos ver um exemplo:

```
console.log(state);   // output: undefined
var state = "ready"
```

Como você pode ver quando nós registramos o valor de estado, a saída é indefinido, porque nós referenciamos antes da atribuição real. Você pode ter esperado um ReferenceError ser lançado porque o estado ainda não foi declarado. Mas o que você não sabe é que a variável é declarada por trás da cena. Aqui está como o código é interpretado por um motor de JavaScript:

```
var state;           // moved to the top
console.log(state);
state = "ready";     // left in place
```

Hoisting (elevação) também afeta declarações de função. Mas antes de ver alguns exemplos, vamos primeiro aprender a diferença entre declaração de função e expressão de função.

```
function showState() {}          // function declaration
var showState = function() {};   // function expression
```

A maneira mais fácil de distinguir uma declaração de função a partir de uma expressão de função é verificar a posição da palavra function. Se a palavra function é a primeira coisa na instrução, então é uma declaração de função. Caso contrário, é uma expressão de função.

Declarações de função são completamente içada. Isto significa que todo o corpo da função é movida para o topo. Isso permite que você chamar uma função antes de ter sido declarada:

```
showState();            // output: Ready

function showState() {
  console.log("Ready");
}

var showState = function() {
  console.log("Idle");
};
```

A razão pela qual o código precedente funciona é que o motor de JavaScript move a declaração de função showState(), e todo o seu conteúdo, para o início do escopo. O código é interpretado como este:

```
function showState() {     // moved to the top (function declaration)
  console.log("Ready");
}

var showState;            // moved to the top (variable declaration)

showState();  

showState = function() {   // left in place (variable assignment)
  console.log("Idle");
};
```

Então...

Todas as declarações de funções e variáveis, são içadas para o topo do escopo que estão definidas, antes de qualquer parte do seu código ser executado.
As funções são içados primeiro, e depois as variáveis.
Declarações de função têm prioridade sobre declarações de variáveis, mas não sobre atribuições de variáveis.

## Closure (Fechamentos)

Closures permitem que os programadores de JavaScript escrevam um código melhor. Criativo, expressivo, e conciso. Nós frequentemente usamos Closures em JavaScript, e, não importa a sua experiência de JavaScript, você, sem dúvida, vai encontrá-los uma vez ou outra.

O que é um Closure?

Um Closure é uma função interna que tem acesso a cadeia de variáveis do escopo da função exterior. O Closure tem três cadeias de escopo: ele tem acesso ao seu próprio escopo de aplicação (variáveis definidas entre as suas chaves), ele tem acesso a variáveis da função exterior, e tem acesso às variáveis globais.

Você cria um Closure, adicionando uma função dentro de outra função. Um exemplo de Closures em JavaScript:

```
function showName(firstName, lastName) {
  var nameIntro = "Your name is ";

  // this inner function has access to the outer function's variables, including the parameter
  function makeFullName() {
    return nameIntro + firstName + " " + lastName;
  }

  return makeFullName ();
}

showName ("Michael", "Jackson"); // Your name is Michael Jackson?
```
Closures são usados extensivamente em Node.js; eles são burros de carga (workhorses) em assíncrono Node.js, arquitetura non-blocking. Closures também são freqüentemente utilizados em jQuery e apenas sobre cada pedaço de código JavaScript que você lê.

Um exemplo clássico de jQuery Closures:

```
$(function() {
  ​var selections = [];
  $(".niners").click(function() { // this closure has access to the selections variable​
    selections.push(this.prop("name")); // update the selections variable in the outer function scope​
  });
});
```

Bem não sabia mas costumo usar Closures :)

## Variável Global

Qualquer variável declarada fora de uma função pertence ao escopo global, e é, portanto, acessível de qualquer lugar no seu código.

```
var global = "I'm global";

function isGlobal() {
  console.log(global);  output: I'm global
}
```

No código acima, você percebe que a função isGlobal() acessa a variável `global` que está fora do escopo da função.

## Variável por parâmetro

Quando uma variável é passada por parâmetro é criada uma instância local daquela variável dentro da função, e toda modificação que esta variável sofrer internamente à função fica limitada ao escopo da função, quando retornar para a chamada da função o parametro terá o mesmo valor antes de chamar a função. Exemplo:

Funções recebem parâmetros, e dentro do contexto da função elas receberão novos nomes, mesmo que sejam variáveis mandadas para a função vindas de outros contextos. Além disso, essas variáveis podem ser acessadas apenas dentro da função. Se modifcarmos o exemplo anterior para receber uma variável por meo de parâmetro e tentarmos acessá-la de fora dará errado.

Dentro do contexto da função é criada uma instância local da variável, e toda alteração que essa variável sofrer internamente fica limitado ao escopo da função, portanto, quando passamos uma váriavel global por parametro para uma função o valor da mesma não sofrerá alterações, ao contrário de alguns exemplos visto anteriormente.

```
var x = 5;

function add(y)
{
  y++;
  console.log(y);
}

add(x); // output: 6
console.log(x); // output: 5
```

## Instanciação usando uma IIFE

**Immediately-Invoked Function Expression (IIFE)**, é uma função que é executada imediatamente no momento que é criada. Também conhecido como, “self-executing anonymous function” que significa “função anônima auto-executável” é a definição de uma função anônima que se auto-executa.

IIFE é muito útil para quando queremos criar um escopo no JavaScript para que as variáveis definidas dentro da função não poluam o escopo global (variáveis definidas no window), exemplo:

```
(function () {
  'use strict'

  var sayHi = 'oi'
  console.log(sayHi) // oi
}())

console.log(sayHi) // ReferenceError: sayHi is not defined
```

Assim mantemos as variáveis todas dentro do nosso código, evitando que o JavaScript de outras pessoas alterem nossas variáveis.

No segundo parênteses onde invocamos a função (function), podemos passar qualquer parâmetro como se fosse qualquer outra função, exemplo:

```
(function (sayHi) {
  console.log(sayHi) // output: oi
}('oi'))
```

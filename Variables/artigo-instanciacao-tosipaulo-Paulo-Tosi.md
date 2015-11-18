# MongoDB - Artigo
autor: Paulo Tosi

## Hoisting

Toda vez que uma variável é definida, sua **declaração** é hoisted, mas não sua inicialização. O que quer dizer que a declaração da variável vai para cima do escopo antes mesmo do código ser executado, mas esta variável não recebe nenhum valor e permanece como **undefined**.

####  APENAS A DECLARAÇÃODE UMA VARIÁVEL É HOISTED, NÃO SUA INICIALIZAÇÃO.

### Variavel


```
/**** Exemplo 01 ****/

console.log(a);

Resultado: Uncaught ReferenceError: a is not defined(…)


/**** Exemplo 02 ****/

console.log(a);

var a = 2;

Resultado: undefined

```

Reparem no código acima que no exemplo 01, quando tentamos imprimir o valor de a, recebemos um erro pois esta variável não existe. Já no exemplo 02, undefined será impresso, mesmo tendo a declaração de a depois do do comando console.log. Ali ocorreu um hoisting da declaração da variável a, mas não da sua inicialização.

### Função

O hosting com funções acontece de maneira diferente. Aqui, não só o nome da função é hoisted como também seu corpo.

```
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

```
function nomeCompleto (primeiroNome, ultimoNome) {
    var nomeIntro = "Meu nome é ";

    function NomeInteiro () {
        return nomeIntro + primeiroNome + " " + ultimoNome;
    }

    return makeFullName ();
}

showName ("Paulo", "Tosi");

Resultado: Meu nome é Palo Tosi

```


## Variável Global

Uma dúvida comum quando se está começando é sobre declaração de variáveis: quando e porque utilizar o var?


```
var numero = 1;

function exibe()
{
    alert(numero);
}

exibe();

```
Conforme esperado, o valor 1 é exibido, já que a variável numero é global. Para confirmar essa informação, vamos fazer:

Afinal, onde o var se encaixa nessa história? O var serve para declarar o escopo da variável. Se uma variável é declarada sem o var, ela é automaticamente global. Quer ver?

```
function define()
{
    numero = 1;
}

function exibe()
{
    alert(numero);
}

define();
exibe();

```

O número 1 é exibido. Para confirmar:

```
function define()
{
    var numero = 1;
}

function exibe()
{
    alert(numero);
}

define();
exibe();

```
Neste caso ocorre o seguinte erro: ReferenceError: numero is not defined. Isto ocorre pois a variável numero pertence ao escopo da função define e, portanto, não pode ser acessada de dentro da função exibe.

## Variável por parâmetro

Como citado anteriormente, temos que tomar um certo cuidado com espoco de variável,  principalmente dentro de funções.
Quando usamos um parâmetro em funções, esta variável se torna local para aquela função. Isso vale também para a variáveis globais.

## Instanciação usando uma IIFE

O Singleton Pattern diz que você pode ter apenas uma única instância de uma classe (ou, no caso do JavaScript, função construtora).

Mas o que tem haver com IIFE?

Sim, esse pattern também pode ser aplicado usando Singleton com o IIFE, que é usado em função de definição do ambito do JavaScript, função imediatamente, invocados pode ser usado para evitar
a elevação variável de dentro de blocos, proteger contra a poluir o meio ambiente global e,
simultaneamente, permitir o acesso do público aos métodos, mantendo a privacidade de variáveis
definidas dentro da função.



#### Fontes:

https://nandovieira.com.br/design-patterns-no-javascript-singleton

http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/

http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/

https://alextercete.wordpress.com/2010/01/15/o-problema-com-as-variaveis-globais-no-javascript/

https://brunosouza.org/functions-no-javascript.html
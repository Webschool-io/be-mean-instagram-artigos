#Artigo - Instanciação em Javascript
autor: Diego Bittencourt Silva

### Hoisting
"Hoisting" é uma palavra em inglês que significa "elevar" ou "içar". O termo é usado em Javascript, pois quando você declara uma variável usando a keyword var dentro de uma função, não importa onde você o faça, essa declaração vai "elevar-se" até o topo da função que a contém. É, por isso, que um dos padrões recomendados é que você declare logo no início da função todas as variáveis locais que usará.

```
function hoistingExample() {  
    var x = 10;    
    function hoisting() {
        x = 4;
        if(true){
            var x = 3;
        }
        console.log(x); //3
    }    
    hoisting();
    console.log(x); //10    
}

O primeiro exemplo mostra um exemplo de hoisting atuando. Se ele não ocorresse, você poderia esperar que a segunda chamada de console.log(x) fosse 4, mas o resultado é 10.
```

### Closures

"Closures" (algo como "fechamento") são funções que "capturam" variáveis que vêm de fora da função. Em inglês se diz que uma "closure" closes over (se fecha sobre) certas variáveis.
Closure tem a ver com o escopo de uma variável em JavaScript.

Para não poluir o namespace global, protegendo que as variáveis do seu script se misturem com outras variáveis de outros scripts, você pode usar um grande closure para colocar todo o código de seu plugin, app, ou library...

```
(function($){
    //  aqui vem o seu código
    var contador;
})(jQuery);

O escopo de um closure sempre vai começar com o caracter { e terminar com o caracter }. Sim, é isso mesmo: um escopo (closure) sempre tem um começo e um fim determinados pelo abre-colchete e fecha-colchete.

O que é interessante é que, ao criar uma função em um ponto determinado do código, você automaticamente cria um closure, onde as atuais variáveis locais ficam disponíveis dentro do novo escopo:

var crud = (function(){
    var modulo = {},
        contador = 0;

    modulo.update = function() {   // aqui temos o início de outro closure
        contador++;
    };  // e aqui o respectivo final

    return modulo;
})();
Repare na variável contador. Nós não temos acesso a ela "do lado de fora" do módulo crud. E ela também não foi definida dentro da função update. Quando chamamos crud.update() pela primeira vez, a variável contador vai passar a valer 1. Se chamamos crud.update() pela segunda vez, a variável contador vai passar a valer 2.
```



### Variável global

Uma variável declarada fora de uma definição de função é uma variável global, e seu valor será acessível e modificável em todo o seu programa.Uma variável declarada dentro de uma definição de função é local.Ela é criada e destruída sempre que a função é executada e não pode ser acessada por qualquer código fora da função.O JavaScript não suporta escopo de bloco (no qual um conjunto de chaves {. . .} define um novo escopo), exceto em caso especial de variáveis com escopo em bloco.

```
Script A

var contador = 0;
setTimeout(function() {
    console.log(contador++);
}, 1000);
Script B

setTimeout(function() {
    contador = 0;
}, 5000);

A cada 5 segundos o script B vai resetar o seu contador para zero – porque ele não é só seu...

Por isso a recomendação é evitar ao máximo o número de variáveis globais em seu código, reduzindo-o a zero se possível, ou criando uma única global com um objeto representando um namespace seu, e pendurando propriedades nesse objeto. Assim, aumentam as chances de seu código conviver em harmonia com o código dos outros.
```

### Variável por parâmetro

Toda função possui dois parâmetros implícitos. this, representando o contexto da função, e arguments, representando os argumentos passados para a função.

Uma variavel enviada por parametro, é atribuida a uma nova variavel que irá fazer parte do escopo da função chamada.

```
var nome = "Diego "
function f2(nome) {
  nome = "Bittencourt";
}
f1(nome)
console.info(nome)
// Diego

Mesmo passando a variavel nome, por parametro, dentro da função ela é atribuida a uma outra variavel, nesse caso chamada nome também, porém as duas são independentes.

Para mudar o nome da variável externa teriamos que passar a variavel como retorno da função

var nome = "Diego"
function f2(nome) {
  nome = "Bittencourt";
  return nome;
}

nome = f2(nome); // Nesse caso estou retornando o nome alterado para a variável externa.
console.info(nome)
// Bittencourt
```

## Instanciação usando uma IIFE

O IIFE significa “Immediately-invoked function expression”, mas podemos chamá-lo de função imediata. Como o próprio nome diz, ela executa a função imediatamente depois de criada. Mas por que usar? Encapsulamento! Tenha em mente que variáveis em Javascript têm como escopo a função pela qual elas foram definidas (podem ser acessadas somente dentro da função, jamais fora). Ao criar uma função anônima com execução imediata, podemos criar um escopo temporário para nossas funções e variáveis. Com isso, evitamos poluição no nosso escopo global e possíveis conflitos de variáveis ou funções com o mesmo nome.
```
var exemplo = ( function (exemploiife) {
	return exemploiife;
}( 'exemplo de iife' ))
console.log (exemplo)

// exemplo de iife

console.log(exemploiife);
// Uncaught ReferenceError: exemploiife nao esta definido(...)

```

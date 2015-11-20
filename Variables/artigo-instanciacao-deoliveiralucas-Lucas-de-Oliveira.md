# Instanciação de Variáveis no Javascript
autor: **Lucas de Oliveira**

## Resumo

Neste artigo veremos algumas funcionalidades fundamentais utilizadas em Javascript, essa que é uma linguagem que cada vez mais cresce no desenvolvimento web. Teremos uma introdução sobre Hoisting, Clousure, Variável Global, Variável por parâmetro e Instanciação usando uma IIFE, veremos também alguns exemplos de utilização.

## Hoisting

Quando uma variável é declarada no Javascript ela é por padrão movida para o topo do escopo atual, ou seja, independente de onde a variável seja declarada no código será o mesmo que declarar no topo, isso é chamado Hoisting. O **Exemplo A**  mostra mais claramente o funcionamento do Hoisting.

- Exemplo A:
```js
varDeclarada = 50;

alert(varDeclarada); // Será impresso 50

var varDeclarada;
```

É importante deixar claro também que **Inicializar** uma variável é diferente de **Declarar**, conforme mostrado no **Exemplo B**, onde o Hoisting é aplicado apenas para declaração de variáveis.

- Exemplo B:
```js
var varInicializada = 20; // Variável inicializada
varDeclarada = 50;

alert('Variável declarada: ' + varDeclarada + '\nVariável inicializada: ' + varInicializada);
// Será impresso "Variável declarada: 50 Variável inicializada: 20"

var varDeclarada; // Variável declarada
```

Para funções, tanto o nome quanto o corpo da função é movida para o topo, o **Exemplo C** mostra como funciona.

- Exemplo C:
```js
funcaoHoisted(); // Executa a função 'funcaoHoisted' escrita no final do script

function funcaoHoisted() {
  alert('Funciona!');
}
```

## Closure

Um Closure é basicamente uma função criada dentro de outra função (ou função interior) que tem acesso aos parâmetro da função exterior. Em geral, o Closure tem acesso a variáveis do seu próprio escopo, do escopo da função exterior e também as variáveis globais. No **Exemplo D** podemos ver como funciona uma Closure.

- Exemplo D:
```js
function funcaoExerior(paramFuncaoExterior) {
    var variavelFuncaoExterior = "Variavel da Função exterior, ";

    function funcaoInterior() {
        var variavelFuncaoInterior = "Variavel da Função interior.";

        return paramFuncaoExterior + variavelFuncaoExterior + variavelFuncaoInterior;
    }

    return funcaoInterior();
}

alert(funcaoExerior("Paramentro função exterior, "));
// será impresso "Paramentro função exterior, Variavel da Função exterior, Variavel da Função interior."
```

## Variável Global

Uma variável Global é declarada fora de um escopo, e quando criada pode ser acessada de dentro de qualquer escopo do código. Desta forma, é importante ter cuidado ao utilizar variável Global. O **Exemplo E** nos mostra o funcionamento de uma variável Global.

- Exemplo E:
```js
var variavelGlobal = 1;

function editarVariavelGlobal(){
	++variavelGlobal;
}

editarVariavelGlobal();
alert(variavelGlobal); // será impresso 2
```

## Variável por parâmetro

Quando passamos variáveis por parâmetro fazemos referência a variáveis que são passadas na assinatura de uma função. Uma variável passada por parâmetro pode ser acessada somente dentro do escopo da própria função, mesmo que a variável seja uma Global, como mostra o **Exemplo F** e **Exemplo G**.

- Exemplo F:
```js
function funcao(param1, param2){
	alert('Parametro 1: ' + param1 + ' / Parametro 2: ' + param2);
}

funcao('Argumento1', 'Argumento2'); // será impresso 'Parametro 1: Argumento1 / Parametro 2: Argumento2
```

- Exemplo G:
```js
var variavelGlobal = 'Global';

function funcao(param1){
	param1 = 'Tentando alterar variavel Global';
}

funcao(variavelGlobal);
alert(variavelGlobal); // será impresso 'Global'
```

## Instanciação usando uma IIFE

O IIFE significa *Immediately-invoked function expression*, também conhecido como função imediata. Como o nome sugere, ela é uma função que é executada imediatamente após a sua criação. Podemos utilizar IIFE para encapsular funções e variáveis, assim evitamos poluição no nosso escopo global e possíveis conflitos de variáveis ou funções com o mesmo nome.
Instanciação usando uma IIFE pode ser utilizando passando parâmetros ou não, os exemplos **Exemplo H** e **Exemplo I** nos mostram como utilizar.

- Exemplo H:
```js
(function() {
	alert('Funciona!!');
})();
```

- Exemplo I:
```js
(function(param) {
    alert('Funciona!! e como ' + param);
})('parâmetro')
```

## Referências Bibliográficas

- Mozilla <*https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures*>
- Javascript Brasil <*http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/*>
- iMasters <*http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife*>
- Wikipedia <*https://en.wikipedia.org/wiki/Immediately-invoked_function_expression*>

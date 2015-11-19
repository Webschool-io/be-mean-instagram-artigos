# MongoDB - Artigo - Instanciação
**autor**: Otaviano Pires Amancio

## Hoisting

É o ato de elevar as declarações de uma variavel (var) ou função (function) para o topo do escopo. Acontecem porque o javascript, por padrão define todas as variaveis e funções logo após ser executado.

Ex. Variavel:

    alert(foo);

    var foo = "Estudando Mean";

O código acima será executado como:

    var foo;

    alert(foo); //undefined

    var foo = "Estudando Mean";

Ex. Function:
 
    bar();

    function bar(){
        alert("Estudando Mean")
    }


## Closure

Os Closures são funções que fazem referência à variaveis livres, globais. São úteis para que uma determinada função faça uso de dados globais de um sistema.

    var a = 50;
    function soma() {
        alert(a + a);
    }

## Variável Global

É um tipo de variavel acessivel em todo o escopo de um programa. Podem ser utilizados e modificados por todos os scripts na página. Uma vez declarada no escopo global, a mesma pode ser referênciada dentro de uma função.

    var texto = "Estudando Mean";
    function exibir() {
        alert(texto);
    }

## Variável por parâmetro

Um parâmetro, quando passafo para uma função, passa a ser executado localmente, podendo, à ele, ser atribuido novos valores.
    
    function myFunction(x, y) {
        if (y === undefined) {
            y = 2;
            x = 3;
        }    
        return x * y;
    }


## Instanciação usando uma IIFE

IIFE é o nome dado a funções que são executadas no momento em que são definidas. Variáveis declaradas dentro do IIFE tornam-se privadas, não podendo ser acessadas pelo escopo global.

    (function () {
        var foo = "Estudando Mean";

        // Outputs: "Estudando Mean"
        console.log(foo);
    })();
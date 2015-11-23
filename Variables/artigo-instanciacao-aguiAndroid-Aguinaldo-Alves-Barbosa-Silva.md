# MongoDB - Artigo
autor: Aguinaldo Alves Barbosa Silva

## Hoisting
```
Hoisting do inglês levantar, hastear, significa colocar no topo.
Em javaScript todas  as declarações dentro de um determinado escopo, são movidas para o topo deste escopo.

Exemplo hoisting:

var variavel = 2;
console.log(variavel);

function testHoisting() {
    console.log(variavel);
    var variavel = 3;
    console.log(variavel);}

testHoisting();

A Saída no console será: 2, indefined, 3.
Isso porque a declaracao da variavel dento da função testHoisting() e movida para o inicio do escopo da função, mas apenas a declaração e movida e a atribuição fica na posição original.
````

## Closure
````
Um closure é uma função interior que tem acesso a variáveis de uma função exterior – cadeia de escopo. O closure tem três cadeias de escopo: ele tem acesso ao seu próprio escopo (variáveis definidas entre suas chaves), ele tem acesso as variáveis da função exterior e tem acesso as variáveis globais.

A função interior tem acesso não somente as variáveis da função exterior, mas também aos parâmetros dela. Note que a função interior não pode chamar o objeto arguments da função exterior, entretanto, pode chamar parâmetros da função externa diretamente.

Você cria um closure adicionando uma função dentro de outra função.

function showName (firstName, lastName) {
    var nameIntro = "Your name is ";
 
    //esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
    function makeFullName () {
        return nameIntro + firstName + " " + lastName;
    }
 
    return makeFullName ();
}
 
showName ("Michael", "Jackson"); //Your name is Michael Jackson

````

## Variável Global
````

A linguagem JavaScript tem dois escopos: global e local.Uma variável declarada fora de uma definição de função é uma variável global, e seu valor será acessível e modificável em todo o seu programa.

var variavel  = 0;
console.log(variavel);

function alteraVariavel(){
	variavel = variavel + 10;

}
alteraVariavel();
console.log(variavel);
````


## Variável por parâmetro
````
Quando uma variável é passada como parâmetro para uma função, é como se fosse uma nova variável ou seja as alterações feitas dentro do escopo dessa função somente serão visíveis no próprio escopo. Caso seja passada uma variável GLOBAL como parâmetro o mesmo acontece.
O valor das variáveis passadas como parâmetro somente irão receber o valor alterado dentro da função caso as mesmas sejam retornadas por referência. Assim o ponteiro da memória onde está sendo alterado os valores não é trocado para um ponteiro novo.
````

## Instanciação usando uma IIFE

````
Esta função é executada no mesmo momento em que é definida. É a definição de uma função anônima que se auto-executa.
Exemplo IIFE:

var fn = (function () { return 'oi' }())
````





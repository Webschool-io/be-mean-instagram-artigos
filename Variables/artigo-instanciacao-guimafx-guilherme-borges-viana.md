autor: Guilherme Borges Viana ( https://github.com/guimafx )

# Artigo Aula 4 - MongoDB / Javascript

## Criação de Variaveis Padrão

Ao criar a variável, é possível inicializá-la com valor. (Sem Hoisting )

```
var x = 5; // Inicializa x
var y = 7; // Inicializa y
```

Ou é possível declará-la sem que a informação de que ela é uma variável (var). Ex:

```
x = 5; // Inicializa x
y = 7; // Inicializa y
``` 

## Criação de Variaveis Padrão com Hoisting, você apenas inicializa ela, sem valor é claro, para posteriormente aplicar algum valor a mesma.

O ideal é sempre declarar todas as variáveis no escopo do código como boa prática de programação.

``` 
var x; // Declara x
x = 5; // Setta valor de 5 em algum momento.
``` 

## O que é Closure ?
Conforme o website Javascript Brasil disponível em: http://migre.me/s9wBi, 

“Um closure é uma função interior que tem acesso a variáveis de uma função exterior – cadeia de escopo.” e ainda sobre: 
“A função interior tem acesso não somente as variáveis da função exterior, mas também aos parâmetros dela. Note que a função interior não pode chamar o objeto arguments da função exterior, entretanto, pode chamar parâmetros da função externa diretamente.” 


``` 
function showName (firstName, lastName) {
	var nameIntro = “Meu Nome é ";

	//esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
	function makeFullName () {
		return nameIntro + firstName + " " + lastName;
	}

	return makeFullName ();
}

showName ("Michael", "Jackson"); // Output => Meu nome é Michael Jackson
``` 

Seria talvez o mais próximo de uma de Classe (Ao estilo Java, PHP, etc…) onde tal propriedade (ou Parametro mesmo) pode ser utilizada dentro de N funções. 
Com o que já vimos, já possibilita criar código para interação MONGODB, MAP REDUCE, ou criar coisas mais complexas.

## Sobre Variável Global

Segundo o portal msdn: A linguagem JavaScript tem dois escopos: global e local.
Uma variável declarada fora de uma definição de função é uma variável global, e seu valor será acessível e modificável em todo o seu programa.
Uma variável declarada dentro de uma definição de função é local. 
Ela é criada e destruída sempre que a função é executada e não pode ser acessada por qualquer código fora da função.
O JavaScript não suporta escopo de bloco (no qual um conjunto de chaves {. . .} define um novo escopo), exceto em caso especial de variáveis com escopo em bloco.

```
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

Como pode ser visto no método antiquities(), a variável aCentaur foi ignorada embora tenha o mesmo nome pois não afetou a variável Global.


## Sobre Variável por parâmetro

Quando uma variável Global é passado como parâmetro, o parâmetro em questão poderá ser utilizado dentro da Função como qualquer linguagem de programação.

## Instanciação usando uma IIFE

Segundo Pedro Araújo:

O IIFE significa “Immediately-invoked function expression”, mas podemos chamá-lo de função imediata. 
Como o próprio nome diz, ela executa a função imediatamente depois de criada. 
Mas por que usar? Encapsulamento! 
Tenha em mente que variáveis em Javascript têm como escopo a função pela qual elas foram definidas (podem ser acessadas somente dentro da função, jamais fora).
Ao criar uma função anônima com execução imediata, podemos criar um escopo temporário para nossas funções e variáveis. 
Com isso, evitamos poluição no nosso escopo global e possíveis conflitos de variáveis ou funções com o mesmo nome.

```
var adder = (function() 
{
 var myPhrase = "";
 return function(x) 
 { 
  return myPhrase = 
  !!myPhrase ? myPhrase.concat(" ", x) : myPhrase.concat(x);
 }
})();
 
adder("Olá"); // "Olá"
adder("Mundo!"); // "Olá Mundo!"
 
myPhrase; // myPhrase is not defined
```

## Considerações

Este artigo foi fundamental para entendimento básico de como se comporta a declaração de variáveis e funções que serão AMPLAMENTE utilizadas dentro do curso de MEAN.
Não será possível avançar na linguagem Javascript sem o entendimento dessas questões.

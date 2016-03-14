# Artigo - Instanciação
**Autor**: Joao Antonio Gardin Vieira - [gardin1992](https://github.com/gardin1992)
**Data**: 1455051025237

## Hoisting
**Tradução**: Içar, levantar
**Definição**: hoisting é o comportamento padrão do JavaScript que move a declaração para o topo [w3school](http://www.w3schools.com/js/js_hoisting.asp)

O Hoisting seria a capacidade do javascript de inicializar as varias definidas no código. Sempre pegando de onde ela está, levando para o topo do seu código e atribuindo a ela o valor 'undefined'. Tem quase a mesma definição de escopo, como na linguagem C. A declaração sobe, mas a atribuição permanesse no mesmo lugar. 

Segue abaixo um exemplo do comportamento do hoisting:
```
// variavel fora da função
var a = 1;
// imprimi 1
console.log(a)

function hoisting(){
	// imprime undefined, pois a varial 'a' só existe fora da função
	// e como foi impressa dentro do block hoisting()
	// ela passa a ser 'undefined', pois o javascript inicializou ela dentro da função
	// e como ainda não foi atribuido ela será 'undefined'
	console.log(a)
	// declarando novamente a variavel 'a'
	var a = 2
	// como fizemos uma inicialização da variavel, o console ira imprimir o valor atribuido
	console.log(a)
}
// imprime primeiro 'undefined' e depois imprimira o valor '2'
hoisting()

// mesmo tendo atribuido um novo valor a variavel 'a'
// está parte do código ira imprimir o PRIMEIRO valor atribuido.
// e se a variavel a não tivesse sido iniciado no começo do código
// aqui estária sendo impresso 'undefined' e não '2' como imaginamos em um primeiro momento
console.log(a)
```

## Closure
**Tradução**: Encerramento
**Definição**: Closures (fechamentos) são funções que se referem a variáveis livres (independentes). Em outras palavras, a função definida no closure "lembra" do ambiente em que ela foi criada. [Mozilla Foundation](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures). 

Closures são funções aninhadas, abstraindo isso, seria o comportamento de tratar uma função como referencia da função maior. Funções aninhadas têm acesso às variáveis declaradas em seu escopo externo.
 
Exemplo de Clousure:
```
// declaraçao uma função simples
// recebe o parametro 'nome'
function clousure(){
	var nome = "Clousure";
	// Essa função é aninhada
	// Esta função não tem variaveis definidas em seu escopo
	// porem ela reusa as variaveis declaradas na função pai
	function exibirNome(){
		console.log(nome)
	}
	return exibirNome();
}
// ao chamar a função, ela ira exibir o valor da variavel que foi passado na função 'clousure'
// mas o método 'exibirNome' é quem ira 'printar' o valor da varial
clousure();
```

## Variável Global
**Definição**: Variáveis Globais são aquelas declaradas no início de um algoritmo. São visíveis, ou seja, podem ser utilizadas no algoritmo principal e por todos os outros subalgoritmos. [Berriel](www.berriel.com.br/ltpi/aula15/aula15.htm)

Com esse estudo sobre variaveis notei um comportamento diferente no javascript. Quando definimos uma variavel como global ela realmente pode ser usada dentro de qualquer função filha do seu código. Porém, se você tentar usar essa variavel dentro da função e na mesma função você declarar novamente essa variavel cairemos no conceito de hoisting do javascript e você tera a variavel com o valor 'undefined' dentro do bloco de função. 

Definindo uma variavel global:
```
// declarando variavel glogal
var global = 1

function semElevacao(){
	// será impresso o valor 1
	console.log( global )	
}

function comElevacao(){
	console.log( global )	
	var global = 2;
	console.log( global )	
}

semElevacao()
comElevacao()
// usando a varivel global, com o valor definido no inicio do código [valor real]
console.log( global )

```

## Variável por parâmetro
**Definição**: São variaveis que seram passadas na chamada de uma funçao, seram os parametros desse metodo. (Minha definição)

As variaveis(parametros) de função em javascript são inicializadas dentro do bloco de código. Quando o método é chamado e nenhum valor é atribuido ao parametro o valor da variavel é igual a 'undefined', mesmo que você tenha definido uma variavel global com o mesmo nome da variavel do método. 

Exemplo de variavel por parametro:
```
// variavel global
var a = 1;
// método com parametro
function passarParametro ( a ){
	// variavel definida no método
	console.log( a )
}

// variavel tera o valor 'undefined'
// pois não foi passado nenhum valor na chamada do método
passarParametro()

// a variavel tera o valor de 15
// pois na chamada do método foi passado um valor
// que será impresso dentro do método.
passarParametro( 15 )

// variavel global que foi definido no começo do bloco
// mesmo manipulando a variavel dentro do método
// não altera o valor da variavel global
console.log(a)

```

## Instanciação usando uma IIFE
**Tradução** IIFE(Immediately-Invoked Function Expression) - Expressão Função Imediatamente Invocada
**Definição**: A finalidade de uma função anônima é exatamente a de permitir passá-la como se fosse um objeto qualquer, que você pode atribuir a uma variável, independentemente de haver um nome para a função. [stackoverflow](http://pt.stackoverflow.com/questions/9936/como-funcionam-fun%C3%A7%C3%B5es-an%C3%B4nimas)

Exemplo de função:
```
// definindo uma função IIFE
(function () {
  console.log('Olá')
}());

// retorno IIFE
// neste exemplo é possivel ver
// que será impresso "Função auto-executavel"
// porém se você tentar acessar novamente a variavel 'exemplo' 
// ela está com o valor 'undefined'
var exemplo = function (){
	return console.log("Função auto-executavel")
}();
```

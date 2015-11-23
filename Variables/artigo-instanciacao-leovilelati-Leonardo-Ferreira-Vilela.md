# Artigo
**autor**: Leonardo Ferreira Vilela

**Prazo**: **até dia 23 de Novembro de 2015 [PRAZO EXTENDIDO]**

Explique, com teoria e código, nesse artigo como o JavaScript cria e instancia as variáveis, seguindo os seguintes tópicos.

## Hoisting

Explique o que é, o porquê acontece e como acontece com variável e função.

´´´
Hoisting é a capacidade que uma linguagem pode ter quando uma variável ou função poder ser utilizada mesmo quando sua declaração só 
acontece posteriormente. Esse conceito foi adotado para que a leitura do código possa acontecer tanto de baixo para cima como
de cima para abaixo. E o compilador necessita percorrer todo código antes de validar onde cada variável ou função foi utilizado
para que assim possa procurar onde a mesma foi declarada.

Exemplo de uso com variável:

var nome = meunome;

console.log(nome);

var meunome;


Exemplos de uso com funções:

No caso de funções, só funciona a ideia de Hoisting caso a função não seja anônima.

//Maneira incorreta
console.log(multiplicaNumero(10,10));
var multiplicaNumero = function(a,b) {
  return a*b;
}
// Saída -->  TypeError: undefined is not a function

//Maneira incorreta
console.log(multiplicaNumero(10,10));
multiplicaNumero = function(a,b) {
  return a*b;
}
// Saída -->  ReferenceError: multiplicaNumero is not defined


//Maneira correta
console.log(multiplicaNumero(10,10));
function multiplicaNumero (a,b) {
  return a*b;
}
// Saída -->  100

´´´


## Closure

Explique o que é, o porquê acontece e como usar. 
Cite situações que você usaria.


´´´
Clousures são funções que armazenam dentro de si, outras funções. E através disso, é possível utilizar 
variáveis locais da função pai dentro das funções filhas. Isso acontece porque funções aninhadas
possuem acesso a variáveis declaradas em seu escopo externo.


function facaAdicionar(x) {
  return function(y) {
    return x + y;
  };
}

var add5 = facaAdicionar(5);
var add10 = facaAdicionar(10);

console.log(add5(2));  
consiole.log(add10(2)); 

// Saída --> 7
// Saída --> 12

´´´

## Variável Global

Explique como se usa uma var Global dentro de uma função.

´´´
As variáveis que são declaradas fora de uma função são consideradas globais naquele escopo:

var numero = 100;
 
function exibe()
{
    alert(numero);
}
 
exibe();
// Saída --> 100


As variáveis que não são declaradas, são consideradas globais no escopo mais externo possível.

var Pessoa = function(nome)
{
    _nome = nome;
 
    this.get_nome = function()
    {
        return _nome;
    }
}
 
var Animal = function(nome)
{
    _nome = nome;
 
    this.get_nome = function()
    {
        return _nome;
    }
}
 
person = new Pessoa('Leonardo');
macote = new Animal('Bob');

alert(person.get_nome());
alert(macote.get_nome());

// Saída --> Bob
// Saída --> Bob


Desta forma o correto é sempre utilizar a declaração de variáveis dentro de uma função para limitar o uso da mesma
dentro do escopo da função, como segue abaixo.

var Pessoa = function(nome)
{
    var _nome = nome;
 
    this.get_nome = function()
    {
        return _nome;
    }
}
 
var Animal = function(nome)
{
    var _nome = nome;
 
    this.get_nome = function()
    {
        return _nome;
    }
}
 
person = new Pessoa('Leonardo');
macote = new Animal('Bob');

alert(person.get_nome());
alert(macote.get_nome());

// Saída --> Leonardo
// Saída --> Bob

´´´

## Variável por parâmetro

Explique o que acontece dentro da função qnd um parâmetro é passado e também explique quando uma GLOBAL é passada por parâmetro.


´´´

Quando passamos para uma função um parâmetro, o mesmo é passado por valor. Isso quer dizer que fora do escopo da função, caso o valor
passado como parâmetro seja uma variável, ele continuará imutável, mesmo que seja alterado seu valor dento da função. Como no exemplo abaixo:

function passoPorValor(meuParametro){ 
   	meuParametro = 45 
   	document.write("Valor alterado para 45") 
} 

var minhaVariavel = 5 
passoPorValor(minhaVariavel) 
document.write ("O valor da variavel atualmente: " + minhaVariavel) 

//Valor alterado para 45
//O valor da variável atualmente: 5


Quando passamos uma variável global por parâmetro, a mesma é utilizada por valor, mantendo o valor original passado.

num = 1;    
    
var calculaSoma = function(x)
{
    x = 2;
    console.log(x);
};

calculaSoma(num);    
console.log(num);

// Saída -->  2
// Saída -->  1

´´´


## Instanciação usando uma IIFE

Explique como uma variável pode receber um valor de uma IIFE.
Explique como passar uma variável por parâmetro para a IIFE e acontece com ela dentro da função.


´´´
Uma IIFE significa “Immediately-invoked function expression” (função imediata), tem como idéia uma função
que ja é executada logo após sua criação, desta forma o retorno de uma IIFE deve ser armazenado em sua criação.

Para ter um melhor controle de um parâmetro passado para um IIFE é util utilizar closure como no exemplo abaixo:


var Alerta = (function() {
	
	var minhaFrase = "Meu nome é: ";

	return function(x) { 
		return minhaFrase = x;
	}
})();

Alerta("Leo");

// Saída -->  Leo

´´´

## Considerações

Quanto mais explicado melhor.

Lembre que isso fará parte do seu currículo como aluno e será disponilizado no sistema de vagas, ou seja, o contratante poderá ver todos seus projetos e trabalhos feitos nesse curso.

Boa sorte.

# Envio

1. Fork [esse repositório](https://github.com/Webschool-io/be-mean-instagram-artigos/) 
2. Nomeie seu artigo usando o seguinte padrão: artigo-instanciacao-githubuser-nome-completo.md
3. Adicione seu exercício aqui na Pasta Variables
4. Faça um Pull Requst enviando seu artigo.

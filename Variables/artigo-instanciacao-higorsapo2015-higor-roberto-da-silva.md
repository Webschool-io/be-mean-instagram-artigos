#Artigo

autor: Higor Roberto da Silva

##Hoisting

Hoisting(içar, levantar) é um procedimento do JavaScript que move declarações para o topo de seus escopos atuais, ex:

var sapo = 'sapo';

function frog(){
	sapo = 'rabit';
	return sapo;
}

console.log(frog()); // resultado 'rabit'

A função frog() içou, levantou 'rabit' para a variável global sapo


##Closure

Closure é uma função que é definida dentro de outra função e reusa as variáveis da função pai.

As vezes queremos emular médodos e variáveis privados com closures para que outras funções não a utilizem. Ex:

var calc = function(value1, value2){ 
			var resulte = 0; 
			return { 
				soma: function(){
					resulte = value1 + value2;
				},
				subtracao: function(){ 
					resulte = value1 - value2; 
				}, 
				multiplicacao: function(){ 
					resulte = value1 * value2; 
				}, 
				divisao: function(){ 
					resulte = value1 / value2; 
				}, 
				value: function(){ 
					return resulte; 
				} 
			} 
}

var total = calc(5, 5);
total.soma();
console.log(total.value()); // 10


## Variável Global

Uma variável global é visualizada glogalmente pelo codigo JS.
Ela é criada fora do escopo de uma função.
As funções podem acessar, utilizar o valor atribuido e modifica-la.

var num = 5;

function alteraNum(){
	num = 10;
	return num;
}

console.log(alteraNum());


##Variável por parâmetro

Quando um parâmetro é passado para um função a função utiliza o valor desse parâmetro.
Porém se esse parâmetro for global, a variável global não é alterada globalmente dentro da função.
Ex:

var num = 10;
function alteraNum(num){
	num = num + num;
	console.log('Variável num dentro da função: ' + num);
}

alteraNum(num); // Variável num dentro da função: 20
console.log('Variável global num: ' + num); // Variável global num: 10


##Instanciação usando uma IIFE

O IIFE significa “Immediately-invoked function expression”, mas podemos chamá-lo de função imediata.
Ela executa a função imediatamente depois de criada.

Uma variável recebe um valor de uma função imediata dependendo do comportamenta da função.
Ex:

var resposta = (function(){ return 5 + 2; }()); // 7

Passando parâmetro para uma função imediata.
Ex:

var resposta = (function(val1, val2){ return val1 + val2 })(3,2); // 7



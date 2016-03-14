# Artigo - Variáveis Javascript
**Autor**: Eliel das Virgens - hc3 - https://github.com/hc3
**Data**: Date.now()

 Iniciando o artigo vou falar sobre escopos, pense no escopo como um campo de visão para o javascript, vou exemplificar com códigos para facilitar 
o entendimento.
```
//Então temos duas variáveis com o mesmo nome so que declaradas em locais diferentes uma foi declarada fora da função e outra dentro.
var global = 200;

function mostraVariavel() {
	global = 201;
	console.log(global);
}
//essa função vai chamar a variável que foi declarada fora da função e vai retornar o valor 200
console.log(global);
//essa função vai chamar a variável que está dentro dela e vai retornar o valor 201
mostraVariavel();
```

esse exemplo é do livro Head First Javascript programmimg 

```
// essas são as variáveis globais [ avatar , skill , pointsPerLevel , usersPoints ]
var avatar = "generic";
var skill = 1.0;
var pointsPerLevel = 1000;
var usersPoints = 2008;

function getAvatar(points) {
	// level é uma variável local acessível apenas na função getAvatar
	// pointsPerLevel é uma variável global que está sendo usada pela função mas seu valor não sera alterado
	var level = points / pointsPerLevel;
	
	if(level == 0) {
		return "Teddy bear";	
	} else if (level == 1) {
		return "Cat";
	} else if (level >= 2) {
		return "Gorilla";
	}
}

function updatePoints(bonus,newPoints) {
	
	var i = 0;

	while (i < bonus) {
		newPoints = newPoints + skill * bonus;
		i = i + 1;	
	}
	return newPoints + userPoints;
}
// aqui está a sacada , como as variáveis são globais elas são alteradas aqui e o valor irá permanecer alterado.
userPoints = updatePoints(2,100);
avatar = getAvatar(2112);
```

#HOISTING

como já foi explicado o escopo, agora vamos para o hoisting que é um comportamento que serve tanto para funções como para variáveis, quando se declara uma variável em javascript ele vai para o topo do escopo, mas para as variáveis apenas sua declaração vai para o topo do escopo, sua
inicialização não exemplo:
```
// quando essa função for chamada o valor da segunda variávei vai estar undefined pois apenas sua declaração foi hoisted e não seu valor.
//console.log está sendo chamado antes do valor da variável segunda ser inicializado.
function hoistingExemplo() {
	var primeira = "1 var declarada";
	console.log(exemplo + "" +segunda);
	var segunda = "2 var declarada";
}

hoistingExemplo();

```  
outro exemplo de hoisting agora com funções, que já não segue a mesma regra das variáveis pois tanto o valor como a declaração são içadas

```
//mesmo a função sendo chamada antes de sua declaração ele está hoisted.
//vale lembrar que uma das formas de declarar funções é por meio de variável então vamos ao um exemplo logo abaixo:
functionExemplo();

function functionExemplo() {
	var a = "Hoisted";
	console.log(a);
}

//amazenando função em variável
a();// aqui teriamos um erro.
var a = function() {
	var a = Hoisted?";
	console.log(a)
}
```

#CLOSURE

 o closure é um comportamento no javascript que pode ser descrito quando temos uma função dentro da outra a função interna fica presa a função 
externa de uma forma que para acessar a função interna, o chamado e feito na função externa segue o código com o exemplo:
```
//essa é a função externa que sera chamada 
function enrolarBaseado(nome,tipo,seda) {
	
	//essa é a função interna 
	function bolar() {
		var texto = "quem ta bolando e:" + nome;
			texto += "\n barro é :" +tipo;
			texto += "\n tipo da seda :"+seda;
			console.log(texto)
	}
	//aqui ela está sendo executada é onde o paranauê acontece
	bolar();
}

//quando a função externa for chamada ela chama a função interna ai é só soltar a fumaça pro ar!
enrolarBaseado("Lampião","Cabrobó","Smoking original");

```
só que o JS é tão foda que podemos driblar isso :

```
// Função externa que dessa vez ao invez de chamar a função interna retorna a mesma
function enrolarBaseado(nome,tipo,seda) {
		
		//função interna que agora é o retorno da função externa
		function bolar() {
			var text = "quem a bolando é: "+nome;
				text += "\n barro é: "+tipo;
				text += "\n tipo da seda é: "+seda;
	}
	//aqui ela está sendo retornada pra quem chamou a função fazer o paranauê acontecer
	return bolar;
}
// olha ai os baseados sendo errolados e assim é feita a mágica do JS
var primeiroBaseado = enrolarBaseado("Marcelo d2","White Window","D2 Hemp");
var segundoBaseado = enrolarBaseado("Hc3","Brenfa do Sertão PE","Somking");

primeiroBaseado();
segundoBaseado(); 
```
o Return faz com que função interna seja retornada a partir da função externa sendo assim a função externa pode ser inserida em uma 
variável que terá como conteúdo a função interna que é retornada a partir da externa.


#VARIÁVEL GLOBAL

uma variável global é aquela que foi declarada fora da função sendo assim pode ser acessada em qualquer função

```
var global = "variavel global";

function alteraVariavel() {
	global = "está na função";
	console.log(global);
}
// a partir da aqui a variável foi alterada para "está na função"
alteraVariavel();

console.log(global);
```

#VARIÀVEL POR PARÂMETRO

as variáveis estão presentes nas funções como parâmetro, imagine que temos uma função que soma dois números

```
// num1 e num2 são duas variáveis que foram declaradas como parâmetro e como o proprio nome já diz parametrizam a função 
// que quando for chamada deve ter dois argumentos que serão num1 e num2
// quando usamos variáveis globais nos parâmetros elas não são alteradas.
function soma(num1 , num2) {
	console.log(num1 + num2);
}
//função com os dois argumentos
soma(2,2);
```
#INSTANCIAÇÃO USANDO UMA IIFE

Imediately inveked function expression , a função vai ser executada no mesmo momento em que é lida ( interpretada ) , a sintaxe e :
```
var tell = (function(tell){
	console.log("telefone: "+telefone);
}("99872210"))
```

#CONSIDERAÇÕES
esse artigo foi básicamente feito por base do http://tableless.com.br de onde tirei minhas proprias conclusões sobre o artigo aqui elas estão
expressas , a instânciação de variáveis é um ótimo assunto para entendermos o Javascript , com isso consegui entender melhor o que fazer no mongodb
para criar um documento ou percorrer uma coleção gostei muito de clousers , da forma como uma função pode desencadear outra e como burlar, na hora 
do código isso pode ser muito eficiente.

#Artigo - Instanciação em Javascript
autor: Cassius Sanches Gachido de Souza

### Hoisting
Hoisting é o procedimento do JavaScript que move as declarações das variáveis para o topo de seus escopos atuais, porém ao ser movida isso não significa que ela está instanciada.
Exemplo:
```
var numero = 5;
function teste() {
	console.log(numero);
	var numero = 8;
}
teste();
//undefined

O que ocorreu é que ele jogou a declaração da variavél 'numero' no topo do escopo, como se tivesse sido feito 'var numero;' mas não atribui seu valor nesse momento.
Seria o equivalente a esse trecho de código:
function teste() {
	var numero;
	console.log(numero);
	numero = 8;
	console.log(numero);
}
teste();
//undefined
//8
```

### Closures

Closure é uma função interior que tem acesso a variáveis de uma função exterior, ou seja, uma função dentro da outra. Closures tem acesso as variáveis do seu próprio escopo, da função exterior e globais.
```
function exibeNome(nome, sobrenome) {
	var complemento = 'Meu nome é ';	
	function montaNome() {
		console.log(complemento + nome + ' ' + sobrenome);
	}	
	return montaNome();
}
exibeNome('Cassius', 'Souza')
//Meu nome é Cassius Souza
```
O que ocorreu é que a função interior montaNome teve acesso a variável 'complemento' que pertence a função exterior exibeNome.
As Closures tem acesso a variável das funções exteriores mesmo após o retorno da função. Veja abaixo:
```
function exibeNome(nome, sobrenome) {
	var complemento = 'Meu nome é ';	
	function montaNome(idade) {
		console.log(complemento + nome + ' ' + sobrenome + ', tenho ' + idade + ' anos');
	}	
	return montaNome;
}

var pessoa = exibeNome('Cassius', 'Souza');
pessoa(26);
//Meu nome é Cassius Souza, tenho 26 anos
```
Note que as variáveis 'nome' e 'sobrenome' se mantiveram quando a variável pessoa recebeu a função exibeNome.


### Variável global

Variáveis globais são variáveis definidas fora do escopo de uma função. Esse tipo de variável pode ser acessada de modificada de qualquer parte do programa.
Veja o exemplo abaixo?

```
var hello = 'Olá';
console.log(hello);
//Olá

function teste() {
	hello = 'Oi';
	console.log(hello);
}
teste();
//Oi
console.log(hello);
//Oi
hello = 'Olá novamente';
console.log(hello);
//Olá novamente
```
Isso signifca que essas variáveis podem ser acessadas e modificadas de qualquer parte do programa. Esse tipo de variável é utilizada quando você precisa que mais de uma função altere seu valor.


### Variável por parâmetro

Quando uma variável é passado por parâmetro de uma função essa variável será utilizada somente dentro daquela função, seu escopo é local, porém se uma variável global for definida na assinatura da função ela deverá ser passada na 
chamada da função, ou seu valor será undefined. Parâmetros definidos na assinatura da função terão seu valor em escopo local somente. Caso seja passada por parâmetro as modificações que com ela ocorrem ficaram restritas aquele escopo,
não alterando seu valor global.
```
var hello = 'Oi';
function teste(hello) {
	var bye = 'Adeus';
	console.log(hello);	
	hello = 'Olá';
	console.log(hello);	
	console.log(bye);
}

teste('Passando valor');
//Passando valor
//Olá
//Adeus

console.log(hello);
//Oi

teste();
//Undefined
//Olá
//Adeus

console.log(bye);
//Uncaught ReferenceError: bye is not defined(…)
```

## Instanciação usando uma IIFE

IIFE signifca  “Immediately-Invoked Function Expression", de maneira geral função imediata. São funções que são executadas logo após sua instanciação.
Seu uso é comum para quando queremos isolar as variáveis definidas dentro de uma função do escopo global, evitando a poluição do escopo global.
```
var texto = (function(meuTexto) {
	return meuTexto;
}('Meu texto'))
console.log(texto)
//Meu Texto

console.log(meuTexto);
//Uncaught ReferenceError: meuTexto is not defined(…)
```

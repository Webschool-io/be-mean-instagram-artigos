# Artigo
**Autor**: Diego Vieira

## Hoisting
Hoisting é um procedimento executado pelo *Javascript* de "elevar" a declaração de uma váriavel para o topo de seu escopo.

```
var amigo = NomeDoAmigo;
console.log(NomeDoAmigo);
```

No exemplo acima será retornado um erro de referência *Uncaught ReferenceError: NomeDoAmigo is not defined*

Agora veja como o Hoisting funciona

```
var amigo = NomeDoAmigo;
console.log(NomeDoAmigo);
var NomeDoAmigo;
```

O código executado no console irá retornar *undefined* pois o Javascript "moveu" a variavel para o topo do escopo, evitando assim um erro de referência.


## Closure
É uma prática que basicamente se cria uma função dentro de outra função, veja o exemplo abaixo.

```
function contaCorrente(saldoInicial) {
	var Saldo = saldoInicial;
	return {
		GetSaldo : function(){
			return Saldo;
		},

		AtualizarSaldo : function(valor) {
			Saldo += valor;
		}
	}
}

var DiegoConta = contaCorrente(500); 
DiegoConta.GetSaldo(); // Retorna o Saldo atual
DiegoConta.AtualizarSaldo(300); // Adiciona mais 300 ao saldo
DiegoConta.GetSaldo(); // Retorna o saldo atualizado em 800

```

Detalhes importantes em *Closure*:
- As funções internas tem acesso as variaveis externas da função, como no exemplo acima;
- As funções continuam com o acesso as variaveis externas mesmo após o retorno da função principal;

## Variável Global
Este tipo de variaveis são declaradas fora do escopo de uma função. Essa variável pode ser acessada de qualquer função e inclusive ter seu valor alterado por elas. Veja um exemplo abaixo.

```
var contador = 0;

function AddContador() {
	console.log('Valor Anterior:' +contador);
	contador++;
	console.log('Valor Atualizado:' +contador);
}

AddContador();
contador;
AddContador();
AddContador();
contador;
```

O código acima irá retornar no console como o quadro abaixo

```
Valor Anterior:0
Valor Atualizado:1
Valor Anterior:1
Valor Atualizado:2
Valor Anterior:2
Valor Atualizado:3
3
```

## Variável por parâmetro
Quando criamos um função e a mesma necessita de parametros para ser executadas, esses parametros serão variaveis declaradas em seu escopo.

```
function Multiplicar(a, b) {
	return a * b;
}
```

## Instanciação usando uma IIFE
É uma função que é iniciada ao mesmo tempo que ela é definida, veja no exemplo abaixo 

```
(function(a) {
	console.log(a);
}("Ela será executada no momento que for definida"))
```
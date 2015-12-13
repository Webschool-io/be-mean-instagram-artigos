# Artigo - Instanciação
**Autor**: Fabio Rodrigo Magalhães - [fabiomagalhaes890](https://github.com/fabiomagalhaes890)
**Data**: 1449955633715

# Instanciação de Variáveis em JS

## Hoisting
	
	Uma variável é declarada em javascript, mesmo que seja no meio do código ela é movida para o início do escopo que foi definido.

	Variable Hoisting

		Quando uma variável é definida a sua declaração é hoisted, mas somente sua declaração e não sua inicialização ou seja essa declaração vai para o topo do escopo antes mesmo do código ser executado mas neste momento essa variável tem o valor 'undefined'.


		Exemplo de Hoisting

		function teste(){}
			console.log(meuvalor);
			var meuvalor = 10;
		}

		No caso acima ele irá retornar 'undefined' pois o que foi movido para o topo foi a declaração da variável e nao sua incialização.

	Function Hoisting

		Para funções tanto o nome como o corpo da função são hoisted.

		function meuteste(){
			console.log('YO');
		}

		essa declaração será elevada para o topo!

		uma função pode tambem ser declarada como expressão porem dessa forma ela segue a regra de hosting de variável onde somente seu nome sera hoisted.

			var minhafunction = function(){
				console.log('teste');
			};

			dessa forma a declaracao 'minhafunction' será elevada para o topo mas a atribuição ou incialização dela permanecerá no mesmo local.

## Closure
	
	Quando uma variável é declarada localmente dentro de uma função e ainda nesse escopo declaramos uma nova função onde essa pode acessar a variável declarada anteriormente.

	Exemplo:

		function meuteste(){
			var name = "Fabio";

			function apresentaNome(){
				console.log(name);
			};

			return apresentaNome();
		};

		meuteste()();

		a variavel name foi declarada dentro da function 'meuteste' e a outra function 'apresentaNome' utiliza essa variavel quando é chamada.

## Variável Global

	Variáveis globais devem ser declaradas fora do escopo de uma função.
	Exemplo:

		//Global
		var nome;

		function atribuiNome(a){
			nome = a;
		};

		atribuiNome('Fabio');

## Variável por parâmetro

	Essas variáveis são utilizadas somente dentro do escopo da função como podemos chamar de Escopo Local.

	Exemplo:

		teste(1);

		function teste(x){

			console.log(x);
		};

		// aqui será apresentado o valor 1 que foi passado na chamada da função 'teste' onde o x está no escopo local.

		Se uma variável global é passada por parametro e dentro dessa função esse valor é alterado ele não será alterado no escopo global, somente no escopo local dentro da função mantendo assim o valor original da variavel global.

## Instanciação usando uma IIFE
	
	IIFE - Immediately Invoked Function Expression
	Define uma função quue é executada no momento da sua definição. É muito utilizado para criar escopos onde as variaveis definidas dentro dessas funções não saiam para o escopo global.

	Exemplo:

		(function (v){
			console.log(v);
		}('parametro passado para v neste momento, esse parenteses está invocando a function'));

# Considerações

	Muito interessante a ideia de fazer o artigo, só assim podemos crescer e saber o que estamos fazendo. Muito bom!!!

	Valeeeu!!!!
# Artigo - Garbage Collector
**Autor**: Fabio Magalhaes - [fabiomagalhaes890](https://github.com/fabiomagalhaes890)
**Data**: 1450114016705

# Garbage Collector em Javascript

	No javascript as variáveis, objetos entre outros são criados e quando não utilizados são automaticamente liberados da memória mas isso não quer dizer que você não deve se preocupar com o consumo de memória! Se estiver pensando dessa forma tome cuidado pois está pensando de forma errada!

	O ciclo de vida da memória será sempre o mesmo independente da linguagem que você estiver utilizando:
		1 - Alocar memória que precisa;
		2 - Utilizar (Ler, Escrever);
		3 - Liberar a memória quando não precisar mais;

	Em javascript a primeira e a segunda são explicitas mas a terceira é implicita!

	A utilização de valores é basicamente a leitura e escrita de informações na memória alocada ou passar um argumento para uma função entre outros, mas a maioria dos problemas está relacionado em como definir quando o espaço alocado não é mais utilizado para que assim seja liberado o espaço da memória?
	É aí que entra o garbage collector que tem a responsabilidade de monitorar a alocação de memória e descobrir se o código ainda é necessário ou não e caso não seja libere-o automaticamente da memória mas saber se esse código é necessário ou não é algo que não pode ser resolvido através de um algoritmo por isso é feito de maneira aproximada.

	Quando um objeto criado em javascript é referenciado por outro objeto ele não pode ser coletado pelo GC, somente quando não houver mais referencias para ele conforme o exemplo abaixo:

		//escopo

		var objeto = {
			atributo: {
				nome: 'JS';
			}
		};					// neste momento criamos 2 objetos referenciando um ao outro como uma de suas propriedades não podendo ser coletado.

		var outroobj = objeto; // aqui criamos outra variável referenciando o objeto.

		objeto = 1;	// aqui tiramos a referencia de 'objeto' que tinha anteriormente, apenas a variável 'outroobj' tem a referencia.

		var atributoobj = outroobj.atributo; // agora esse objeto tem duas referencias, uma para o objeto em si e outra para sua variavel.

		var outroobj = 'MALUCAO'; // agora temos apenas uma referencia para a variável que existia dentro do objeto fazendo com que o mesmo ainda não 							  //possa ser coletado da memoria.

		atributoobj = null   // nesse momento não existe mais referencias para o objeto que foi criado no inicio, agora sim ele pode ser coletado da 						 //memória.
		Deve-se tomar muito cuidado para não criar objetos onde referenciam um ao outro formando um ciclo onde os mesmos nao sejam mais necessários e ainda assim permaneçam alocados na memória mesmo sendo declarados dentro de uma função conforme o exemplo abaixo:

			function teste(){
				var objeto = {};
				var outroobjeto = {};
				
				objeto.dado = outroobjeto; // referenciando a variavel outroobjeto
				outroobjeto.dado = objeto; // referenciando a variavel objeto
			};

			teste();

			// neste caso os objetos se referenciam formando um ciclo onde permanecem vinculados mesmo depois de a funcao ser executada e os mesmos não podem ser limpos pelo garbage collector pois ainda tem referencias para cada uma delas.
			Deve-se tomar cuidado com situações assim.

		Algoritmo de Varredura e rotulação

			É feita uma varredura buscando objetos que não tem referencias  onde esses objetos sao definidos como roots, objetos globais. O Coletor vai iniciar sua busca por esses roots econtrando todos os objetos referenciados por esses roots e todos os objetos referenciados por eles. O coletor vai encontrar todos os objetos acessíveis e coletará os inacessíveis. 

			Os ciclos citados anteriormente não são mais um problema depois desse algoritmo por que a partir do momento que os objetos não estão mais referenciados por algo que seja acessível no objeto global ou seja, tornem-se inacessiveis pelos roots então já podem ser coletados pelo Garbage collector.

# Considerações
	
	Não tinha idéia de como funcionava o garbage collector no js, nunca pensei em procurar mas sempre me preocupei com o consumo de memória.. Mas quanto mais da teoria soubermos melhor faremos nosso trabalho e saberemos aplicar muito mais a prática. Eu penso dessa forma.
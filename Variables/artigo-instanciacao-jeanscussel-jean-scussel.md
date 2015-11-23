# Artigo
**By**: *Jean Scussel*

#Como o javaScript instância uma variável

###Escopo

Para trabalhar com `JavaScript` de maneira eficiente, uma das primeiras coisas que precisamos entender é o conceito de escopo de variável. O escopo da varíavel é controlado pela localização da declaração da variável, e define a parte do programa onde uma variável em particular é acessada.

Em JavaScript temos dois escopos, `global`, que são as varíaveis declaradas fora de uma função, e podem ser acessadas em qualquer parte do código, e `local` que são as avariáveis criadas dentro de uma função, portanto não podem ser acesadas por qualquer código fora da função. Além disso o JavaScript não suporta escopo de nível de blocos assim como `Java`, `C` e `C++`, ou seja, em `JavaScript` só temos escopo por nível de função, sendo assim, as variáveis criadas dentro de blocos de código, como por exemplo um for, não são restritas a este bloco.

Variáveis locais podem ter os mesmos nomes que variáveis globais e uma alteração feita em uma, não altera a autra. As variáveis são avaliadas como se fossem declaradas no início do escopo no qual existam. Às vezes, isso resulta em comportamentos inesperados, como mostrado aqui.

```
var numero = 100;
mostrar();

function mostrar(){
	// Isto imprimirá, "undefined", porque numero está declarado localmente;
    document.write(numero);

    if (false)
    {
        var numero = 123;
    };
};

```

Quando o JavaScript executa uma função, ele primeiro procura todas as declarações de variáveis, criando as variáveis com um valor inicial de undefined mesmo se atribuirmos inicialmente algum valor, ela assumirá o valor declarado somente quando a linha contendo a declaração for executada. O JavaScript executa o código na função, e se uma variável estiver declarada dentro de uma função, mas não tiver sido declarada com var, ela será criada como uma variável global.

No exemplo a seguir, a segunda chamada para a função interna gera a mesma mensagem `(“Olá Jean”)` assim como a primeira chamada, porque o parâmetro de entrada para a função externa, nome, é uma variável local que é armazenada no fechamento da função interna.

```
	function enviar(nome) {
    	return function () {
        	enviar2(nome);
   	 	}
	}

	function enviar2(msg) {
  	  	console.log('Olá ' + msg);
	}
	var func = enviar('Jean');
	func();
	// Saída
	// Olá Jean
	enviar2('Ana');
	// Saída:
	// Olá Ana
	func();
	// Saída:
	// Olá Jean

```

## Hoisting

Da forma mais simples, podemos definir `hoisting` como um procedimento que move as declarações de variáveis do JavaScript para o topo de seus escopos atuais.

Observando o código a seguir, responda rápido, qual será a saída no Console?

```
	var num = 1;
	function teste(){
    	console.log(num);
    	var num = 5;
	}
	init();

```

Por causa da forma como são tratadas as declarações e escopos no JavaScript, a saída para o script acima será `undefined`.

Hoist significa levantar ou suspender um objeto. E é isto o que acontece em JavaScript quando declaramos uma variável ou função. Sua declaração é `elevada` para o topo do escopo.

Quando uma variável é definida, sua declaração é `"levantada" (hoisted)`, mas não inicializada. O que quer dizer que a declaração da variável vai para o topo do escopo antes do código ser executado, mas esta variável não recebe nenhum valor e permanece como undefined.

Vejamos este texemplo:

```
	// Irá imprimir o erro dentro do `catch`
	try {
  		console.log(exemp)
	} catch (e) {
  		console.error('A variável `exemp` não foi definida.')
	}

	// Irá imprimir `undefined`
	try {
  		console.log(exemp)
  		var exemp = 2
	} catch (e) {
  		console.error('A variável `exemp` não foi definida.')
	}

```
Na primeira parte do código acima quando tentamos imprimir o valor de `exemp`, recebemos um erro de variável inexistente. Na segunda parte `undefined` será impresso, mesmo tendo a declaração de `exemp` depois do comando console.log. Nesse momento ocorreu um hoisting da declaração da variável `exemp`, mas não sua inicialização, ou seja apenas a declaração da variável vai para o topo, mas não sua inicialização, que continua no mesmo lugar em que definimos no nosso código. Por isso recebemos um undefined quando tentamos acessar seu valor. Isso ocorre porque o código é executado de forma linear.

***"Apenas a declaração de uma variável é hoisted, não sua inicialização."***

Exemplificando melhor:

Vamos considerar o seguinte código:

```
	var exemplo = "Valor de exemplo";
	alert(exemplo);
```
Certo! Sabemos com certeza que o alert vai mostrar o valor `"Valor de exemplo"`. Vamos então criar uma função imediatamente após a declaração da variável com o mesmo valor.

```
	var exemplo = "Valor de exemplo";

	(function() {
  		alert(exemplo);
	})();

```
SIM! Dessa forma obviamente o resultado será o mesmo. Porém, vamos atentar para o próximo exemplo, onde criamos uma função com uma variável local, com o mesmo nome da variável global.

```
	var exemplo = 'Valor de exemplo';

	(function() {
  		alert(exemplo);
  		var exemplo = 'Valor diferente';
	})();

```
COMO? Porque meu alert ta mostrando udefined? Nós criamos a variavel logo abaixo do alert, então isto deveria fazer algum efeito, certo? ERRADO!

*"Declarações de variáveis são levantadas(Hoisted)."*

Neste escopo, independentemente de onde a variável é declarada apenas a declaração será levantada, se a variável for inicializada com o valor atual, ela será setada como undefined. Tudo bem, mas qual a diferença entre declaração e inicialização, vejamos:

**Declaration**

```
	var nome; // declaração

```

**Initialization**

```
	nome = 'Jean'; // inicialização

```

Agora que entendemos as terminologias podemos facilmente compreender oque acontece por tráz desse código:

```
	(function() {
  		var a = 'a';
  		var b = 'b';
  		var c = 'c';
	})();

```

*Declare todas as variáveis no topo!*

Note que o exemplo acima é conciderado um prática ruim,  no entanto, todas essas declarações de variáveis independentemente de onde elas ocorrem no escopo da função serão hasteadas no topo, assim:
```
	(function() {
  		var a, b, c
  		a = 'a';
  		b = 'b';
  		c= 'c';
	})();

```

Vejamos agora o script onde originou-se a confusão com o `undefined`:

```
	var exemp = "Valor de exemplo";

	(function() {
  		alert(exemp);
  		var exemp = "Valor local de exemplo";
	})();

```
Agora podemos ter um entendimento perfeito de porque a variável `exemp` está retornando `undefined`. Sabemos agor, que , quando a variável `exemp` é declarada, ela é automaticamente `"hasteada"` para o topo do escopo da função anteriormente ao `alert`, ou seja, a varável já tinha sido declarada no momento do alert, no entanto, como a inicialização não é `hasteada` antes, o valor da variável é, `undefined`.


##Closure

Closures são funções que se referem a variáveis livres, ou seja, a função definida no closure "lembra" do ambiente em que ela foi criada.

No código abaixo, a função `externa()` cria uma variável local chamada nome, e depois define uma função chamada `mostrarName()`, que é uma função aninhada (um closure), ela é definida dentro da função `externa()`, e está disponivel apenas dentro do corpo daquela função. A função `displayName()` não tem variáveis locais próprias, e ao invés disso reusa a variável `nome` declarada na função pai

```
	function externa() {
 		var nome = "Zé Colméia";
  		function mostrarNome() {
    		alert(nome);
  		}
  		mostrarNome();
	}
	externa();
```

Closure é uma função interna que tem acesso a variáveis de uma função externa. O closure tem três cadeias de escopo: acesso ao seu próprio escopo, acesso as variáveis da função externa e acesso as variáveis globais.

A função interna tem acesso não somente as variáveis da função externa, mas também aos parâmetros dela. Note que a função interna não pode chamar o objeto arguments da função externa, entretanto, pode chamar parâmetros da função externa diretamente.

Você cria um closure adicionando uma função dentro de outra função.

Vejamos um exemplo básico de closures no JavaScript, no script abaixo, a função interior tem acesso as variáveis da função exterior, incluindo os parâmetros:

```
	function mostraNome(nome, sobrenome) {
		var nomeIntro = "Seu nome é ";

		function juntaNome() {
			return nomeIntro + nome + " " + sobrenome;
		}
		return juntaNome();
	}
	mostraNome("Jean", "Scussel");
```

Uma das mais importantes e delicada característica dos closures é que a função interior continua tendo acesso as variáveis da função exterior mesmo após ela ter retornado. Mesmo depois da função exterior retornar, a função interior continua tendo acesso as variáveis da função exterior. Portanto, você pode chamar a função interior depois em seu programa. Vejamos no exemplo:

```
	function nomeJogador (primeiroNome) {
		var nome = "Este jogador é ";

		function sobrenomeJogador (sobrenome) {
			return nome + primeiroName + " " + sobrenome;
		}
		return sobrenome;
	}

	var nome2 = nomeJogador ("Jean");
	nome2 ("Scussel");
```

No código acima a função exterior `nomeJogador` foi retornada, o closure `(sobrenome)` é chamado depois da função exterior ter sido retornada, observe que, o closure continua tendo acesso as variáveis da função exterior e os parâmetros.

## Variável Global


São variáveis definidas fora de uma função e acessíveis em qualquer lugar, depois de serem declaradas.

```
	var variavelGlobal = "Jean";

	var teste = function() {
    	console.log(variavelGlobal);
	}
```

A variável global declarada no inicio do código acima pode ser acessada em qualquer lugar, mesmo dentro da função `teste`.

Observer agora o código abaixo:

```
	function soma(valor){
  		resultado = valor + 5;
	}
	function mostraResultado(){
  		console.log(resultado);
	}
	soma(10);
	mostraResultado();
```

A variável `resultado` utilizada dentro da função `soma` não foi declarada, ela foi simplesmente inicializada, desta forma ela se torna um variável global, e pôde ser chamada em uma outra função.

## Variável por parâmetro

Parâmetros são usados para mandar valores à função. Por exemplo, uma função que realiza uma soma de dois valores recebe como parâmetros as respectivas variáveis e consequentemente seus valores. Veja um exemplo:

```
	function media(val1, val2){
    	var resultado = (val1 + val2) / 2;
        alert(resultado);
    }
	media(5, 8);

```
Na funçãa cim, de forma simples a função é criada com duas variáveis como entrada, quando invocamos a função passamos para ela os valores que correspondem aos seus parâmetros.

Vejamos um exemplo diferente:

```
	function escrever(nome){
    	document.write("<H1> Meu nome é " + nome + " ! </H1>");
	}
    escrever("Jean");
```

Como podemos ver no exemplo, para definir um parâmetro temos que por o nome da variável que vai armazenar o valor que vamos passar. Essa variável, que neste caso se chama nome, terá como valor o dado que passarmos a ela. A vida da variável é durante a execução da função e deixa de existir quando a função terminar sua execução.

** *"Parâmetros passam-se por valor"* **

Temos sempre que indicar que os parâmetros das funções se passam por valor. Isto é, mesmo que modifiquemos um parâmetro em uma função a variável original que havíamos passado não mudará seu valor. Vejamos um exemplo abaixo:

```
	function exemplo(parametro){
    	parametro = 25;
	}
	var variavel = 20;
	exemplo(variavel);
	document.write (variavel);
```

Temos no exemplo acima, uma função que recebe um parâmetro, e que modifica o valor do parâmetro. Fora da função inicializamos uma variável e atribuimos o valor 20, posteriormente chamamos à função passando esta variável como parâmetro e dentro da função modificamos o valor do parâmetro, normalmente variável original mudaria seu valor, mas, os parâmetros não modificam o valor original das variáveis, portanto, ela não muda de valor.

** *"Parâmetro é uma variável local!"* **


## Instanciação usando uma IIFE

IIFE(Immediately-invoked function expression), conhecido também como `função imediata`. Como o próprio nome diz, ela executa a função imediatamente depois de criada. O motivo pelo qual devemos usar é o `encapsulamento`. As variáveis em Javascript têm como escopo a função pela qual elas foram definidas. Ao criar uma função anônima dessa forma, criamos um escopo temporário para nossas funções e variáveis. Evitamos assim a poluição no nosso escopo global e possíveis conflitos de variáveis ou funções com o mesmo nome.

```
	var exemplo = (function() {
 		var texto = "";
 		return function(x) {
 			return texto =
 			!!texto ? texto.concat(" ", x) : texto.concat(x);
 		}
	})();

	exemplo("Olá"); // "Olá"
	exemplo("Mundo!"); // "Olá Mundo!"

	texto; // texto is not defined
```

No exemplo, criamos uma função anônima imediata que retorna uma outra função que concatena uma string na variável chamada `texto`, esta por sua vez está no escopo da IIFE e não na função retornada. Portanto, `texto` não é definido toda vez que invocamos a função `exemplo`, e o mais, `texto` está limitado ao escopo da função anônima imediata, não permitindo o seu acesso direto.

A Sintaxe da IIFE é `(...)();`, fica muito confuso tentar entender a sintaxe de uma IIFE que possui parênteses com comportamentos tão diferentes. Então vamos lá. 

Sabemos que uma função pode ser definida da seguinte forma: `function nomeDaFuncao() { /* codigo */ }`, e até esta parte é fácil entender, mas, e se colocarmos o conjunto de parênteses () para invocar a função enquanto definimos? O resultado será uma **IIFE**, certo?

`function nomeDaFuncao() { /* codigo */ }();`

... e **"tcharaaam"** `// SyntaxError: Unexpected token ).`

... e desta forma agora com função anônima:

`function() { /* codigo */ }();` 

Novamente o mesmo erro `// SyntaxError: Unexpected token (`

Então nossa suposição anterior estava errada! Como podemso observar, se tentamos invocar uma função enquanto definimos, temos um erro.

Mas porque? Não podemos começar com a keyword function, pois quando usada em escopo global ou dentro de outra função, temos um estado de declaração de uma função (Function Declaration). Neste caso, se tentarmos definir uma IIFE da maneira mostrada acima, estaremos lidando com declaração e o que precisamos é de uma expressão.

Se transformarmos uma função em Function Expression e, em seguida, um conjunto de parênteses (), esses mesmos parênteses se tornam uma invocação de uma função. Mas então como fazer uma function se tornar uma expressão? Apenas a inserimos dentro de um grupo de operadores.

```
	(function minhaFuncao() { /* codigo */ })(); // undefined

	// Função anônima
	(function() { /* codigo */ })(); // undefined

	//Função imediata com parâmetro
	(function minhaFuncao(parametro) { console.log(parametro); })(666); // 666

	// Função anônima
	(function(parametro) { console.log(parametro) })(666); // 666

```

## Considerações

Espero ter conseguido explicar da forma mais simples possível, para que os, que assim como estejam procurando um texto introdutório. Busquei da melhor forma organizar as idéias principais e pegar os melhores exemplos que achei.

##Referências

https://msdn.microsoft.com/pt-br/library/bzt2dkta%28v=vs.94%29.aspx
http://www.sitepoint.com/demystifying-javascript-variable-scope-hoisting/
http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html
http://adripofjavascript.com/blog/drips/variable-and-function-hoisting
http://jamesallardice.com/explaining-function-and-variable-hoisting-in-javascript/
http://www.w3schools.com/js/js_function_closures.asp
http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/
http://snook.ca/archives/javascript/global_variable


#Artigo
autor: João Paulo S de Araújo

#Instanciação de variáveis em JavaScript

Neste artigo explicarei de forma mais didática e exemplificada possível como funcionam instanciações de variáveis em Javascript.

#Hoisting (Hasteamento/Elevação/Içamento)

É o comportamento do interpretador JavaScript processar as declarações de variáveis e funções antes de processar qualquer outra linha de código do escopo (contexto) em que foi declarada. Em Javascript existem dois tipos de escopo, local (declaração dentro de funções) e global (declaração fora de funções).

##Variable Hoisting(Elevação de variável)

O escopo de declaração pode ser global ou local, conforme exemplos abaixo:

Escopo global, as variáveis são declaradas fora de qualquer função e ficarão disponíveis em todo código, inclusive dentro de funções:

```
var nome;
```

Obs.: Caso seja atribuído valor a uma variável sem que ela tenha sido préviamente declarada (com o prefixo `var`), ela será considerada no escopo global, mesmo que essa atribuição seja feita dentro de uma função, veja mais sobre variáveis globais no parágrafo `Variável Global`. 
Em strict mode (modo estrito) essa atribuição direta gerará uma exceção.

Escopo local, as variáveis só ficarão disponíveis dentro da função que foi declarada e de suas subfunções:

```
1. function Funcao1() {
2.     var variavel1;
3.     function Funcao2(){   //Está sendo declarada dentro da Funcao1
4.          //variavel1 estará disponível aqui também
5.     }
6. }
```

Mesmo que seja atribuído um valor já na declaração da variável só será feito `Hoisting` da declaração, ou seja, caso tente usar a variável antes da linha que tenha sua declaração com atribuição ela existirá por causa do `hoisting` mas seu valor será undefined. Veja o exemplo abaixo:

```
1. console.log(a); //undefined
2. var a = 2;
```

Antes de processar as intruções é feito o hoisting da declaração da variável a da linha 2, mas sua atribuição não é feita, então até que chegue na linha 2 a variável a tem o valor `undefined`, como a impressão do valor dela está na linha 1 irá ser impresso `undefined`.


##Function Hoisting(Elevação de função)

No caso de função não é feito hoisting apenas da declaração dela mas também do corpo, veja exemplo abaixo:

```
1. funcTeste(); //Chamada da função
2. function funcTeste(){ // declaração da função
3.	console.log('Hoisting de função'); //Irá exibir mensagem sem problemas
4. }
```

Mesmo com a função sendo chamada antes de sua declaração ela é processada sem erros, porque é feito hoisting dela toda, caso contrário daria exceção.

Existe uma exceção ao hoisting ser feito em toda função, quando a função é declarada como expressão, neste caso o hoisting se comporta como de uma variável, apenas a declaração da função é hoisted. Veja exemplo abaixo:

```
1. funcaoA() //Chamada dará erro => TypeError
2. var funcaoA = function() {}
```

Por ser expressão a função é apenas declarada, até que chegue no seu corpo seu tipo será `undefined` e qualquer chamada a ela dará exceção.

Declaração de função sobrescreve declaração de variável, ou seja, se forem declaradas uma função e uma variável com o mesmo nome a declaração da função irá sobrepor a declaração da variável, veja exemplo abaixo:

```
1. //variável e função possuem o nome "endereco"
2. var endereco;
3. function endereco () {
4. 	  console.log ("Variável e função com mesmo nome");
5. }
6. //Impressão do tipo de endereco
7. console.log (typeof endereco); //Será impresso => function
```

Mas atribuição de variável sobrescreve declaração de função, veja exemplo abaixo:

```
1. var teste = "teste";
2. function teste () {
3. 	  console.log("Exceção de sobrescrita de função");
4. }
5. console.log(typeof teste); //Será impresso => String
```

No exemplo acima o hoisting da função sobrescreve a variável, mas há uma atribuição de uma valor à variável na linha 1, tornando a variável tipo String.


#Closures 

É uma técnica em linguagens de programação onde, é compartilhado o escopo das variáveis de uma função com suas subfunções (funções criadas dentro de outras funções), aplica-se a variáveis declaradas apenas na função principal, inclusive variáveis passadas via parâmetro. 
É feito compartilhamento da referência das variáveis da função principal, assim qualquer alteração na variável reflete no restante que tiver essa referência.
A cada chamada de função é criado um execution context (contexto de execução) para ela, nesse momento da chamada de uma função que possui subfunções é criada uma closure e são armazenadas as referências às variáveis da função principal, por essas referências que as variáveis são acessadas.

Veja exemplo:

```
1.  function msgParaUsuario() {
2.      var texto = "Olá usuário";    
3.      return function exibirMsg() {
4.          console.log(texto);
5.      };
6.  }
7.  var mensagem = msgParaUsuario();
8.  mensagem();
```

A subfunção "exibirMsg" utiliza diretamente a variável "texto"  que foi declarada e inicializada na função principal "msgParaUsuario".

Outra característica de uma closure é que a variável da função principal continua em memória mesmo depois de sua chamada. Veja o exemplo abaixo:

```
1.   var CriacaoDeProdutos = function( ) {
2.   var cod = 0;
3.   var criaProduto = function( descr ) {
4.       cod++;
5.       return {
6.           cod: cod,
7.           descr: descr
8.       };
9.   };
10.  return criaProduto;
11.  };
12.  var novoProduto = CriacaoDeProdutos();
13.  var substitutoQueijo = novoUsuario( "Mandiokejo" );
14.  var proteinaDeVerdade = novoUsuario( "Espirulina" );
15.  alert(substitutoQueijo.cod);    // 1
16.  alert(proteinaDeVerdade.cod);    // 2
```

Mesmo depois da função principal "CriacaoDeProdutos" retornar, a variável "cod" continua em memória com seu valor, possibilitando que a subfunção "criaProduto" itere a variável corretamente.



##Algumas indicações de uso de closures são:

*Necessita ter uma variável com acesso compartilhado, mas que não se misture entre scripts, assim evita declaração desnecessária de variáveis de escopo global.

*Utilização de variáveis privadas, assim como numa classe em linguagens orientadas a objeto, podem ser obtidos os valores mas não ser manipuladas diretamente, dando mais segurança no código, veja exemplo:

```
function conta () {
	var saldo = 100;
	return {
		getSaldo: function () {
			return saldo;
		},
		setSaldo: function (saldoNovo) {
			saldo = saldoNovo;
		}
	}
}

var contaBancaria = conta ();
contaBancaria.getSaldo(); //será impresso => 100
contaBancaria.setSaldo(900); //Atualização de saldo
contaBancaria.getSaldo(); // retorna o saldo atualizado
```

Só é possível alterar o valor do saldo chamando a função "setSaldo", nele seria possível ter quaisquer consistências necessárias para evitar uma alteração indevida na variável, garantindo que de nenhuma forma o saldo seja alterado diretamente sem passar pelas consistências.

*Casos em que uma função será reusada e de alguma forma precisa manter a última posição do seu uso.

#Variável Global

Variáveis globais podem ser acessadas e modificadas de qualquer parte do código, sua utilização deve ser feita com cuidado pois além de muitas vezes ficar o tempo todo na memória desnecessariamente pode haver muita confusão na execução do código e problemas de dificil detecção, pois a mesma variável pode ter sido alterada em qualquer parte de todo o código, logo o problema pode ter surgido de qualque lugar.

##Formas de declarar uma variável global

Explicitamente, com o prefixo "var":

```
1.  var nome = "João";
2.  
3.  function exibirNome(){} 
4.      alert(nome);
5.  }
6.  
7.  function alterarNome(){
8.      nome = "Nome alterado"    	
9.  }
10. exibirNome(); //Será exibido => "João"
11. alterarNome();
12. exibirNome(): //Será exibido => "Nome alterado"
```

Como mostrado no exemplo qualquer alteração que éfeito na variável global reflete em todo lugar que ela é utilizada.


Implicitamente, sem o prefixo "var", atribuindo diretamente um valor a variável sem declará-la:

```
1.  function funcao1(){
2.		texto = "Variável global";
3.  }
4.  
5.  alert(texto); //Será exibido => "Variável Global"
```

Como é possível ver no exemplo, quando uma variável é declarada implicitamente, não importa onde ela é declarada, dentro ou fora de qualquer função, ela será global.
É a forma menos indicada, pois, fica muito dificil identificar o escopo da variável nesse caso, só efetuando uma busca para saber se ela foi declarada explicitamente em algum lugar do código.
Esse tipo de declaração retorna exceção no strict mode (modo estrito).

Uma forma de reduzir a probabilidade de problemas com variáveis globais é passar seu valor via parâmetro para as funções, assim, mesmo que a função altere esse parâmetro a variável global não será alterada e não afetará o restante do código.



#Variável por parâmetro

Ao chamar funções é possível passar valores de variáveis diretamente para a função na forma de parâmetros.
No momento que a função é chamada são criadas as variáveis que estão como argumentos na criação da função, com os valores das variáveis passados na chamada da função, assim como qualquer variável criada na função é de escopo local, independente se a variável que foi passada na chamada da função tinha escopo global ou local, ou seja, qualquer alteração feita na variável criada via parâmetro só refletirá na própria função.

Veja exemplo:

```
1.   var souGlobal = "Global intacta"; //Criaçao da variável global
2.   function exibirValorVariavel(variavel){ //Criação da função 
3.   	variavel = "alterada"	;
4.	 	alert(variavel); // Será mostrado => "alterada"
5.		alert(souGlobal); // A variável global permanece com seu valor original e será mostrado => "Global intacta"
6.   }
```


#Instanciação usando uma IIFE (Immediately-invoked function expression)

IIFE é uma sigla para "Immediately-invoked function expression" popularmente conhecida como: "funções autoexecutáveis" ou "funções imediatas", é o recurso de executar a função imediatamente após sua criação.
Variáveis criadas nela, seja explicitamente ou via parâmetro são de escopo local, ou seja, não podem ser acessadas de fora da função diretamente.

Estrutura de uma IIFE:

```
1.  (function () 	
2.  	//Corpo da função
3.  })()	
```

Para funcionar a IIFE, a função tem que ser declarada na forma de expressão, que é feito envolvendo-a em parênteses (linha 1), no final tem abertura e fechamento de parênteses (linha 3) para fazer a invocação da função.

Pode-se usar operadores unários no lugar de parênteses para transformar a função em expressão:

```
!function(){ /* codigo */ }();
~function(){ /* codigo */ }();
-function(){ /* codigo */ }();
+function(){ /* codigo */ }();
```

Para receber um valor de uma IIFE basta usar o "return" como usaria em uma função comum, veja exemplo:

```
var teste = (function(x){
	return x;
}
)("Teste");
console.log(teste); //Será mostrado => "Teste"
```

Assim como uma função comum, uma IIFE pode receber parâmetros, veja exemplos:

Eviando texto como parâmetro:

```
1.  (function (saudacao) 	
2.  	alert(saudacao); //Será mostrado => Olá, bons estudos
3.  })("Olá, bons estudos!")	
```

Enviando variável como parâmetro

```
1.  var msg = "Mensagem";
2.  var teste = (function(){
3.  	return function(x){
4.  		console.log(x);
5.  	}
6.  }
7.  )();
8.  teste(msg); //Será impresso => "Mensagem"
```

Neste exemplo a IIFE está um pouco diferente, foi criada uma IIFE na variável `teste` (linha 2) e dado `return` direto em uma função anônima interna onde é impresso valor do parâmetro enviado para a IIFE.
Obs.: Neste caso o uso do parênteses para envolver a IIFE se torna facultativo.

##Qual a utilidade?


Umas das utilidades da IIFE é criação de módulos encapsulados em sua aplicação, a IIFE retorna um objeto com valores e funções, veja exemplo:

```
1. var counter = (function () {
2.	 var current = 0;
3. 	 return {
4.		name: "counter",
5.		next: function () {
6.			return current + 1;
7.		},
8.		isFirst: function () {
9.			return current == 0;
10.		}
11.	 };
12.})();
```



#Fontes

##Hoisting
http://javascriptbrasil.com/2013/10/11/escopo-de-variavel-e-hoisting-no-javascript-explicado/
https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/var
http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/
http://tableless.com.br/elevacao-ou-javascript-hoisting/

##Closures
http://pt.stackoverflow.com/questions/1859/como-funcionam-closures-em-javascript
https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures
http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/
http://klauslaube.com.br/2011/05/29/afinal-o-que-sao-closures.html
http://www.profissionaisti.com.br/2011/06/afinal-o-que-sao-closures/

##Variável Global
http://pt.stackoverflow.com/questions/32251/vari%C3%A1vel-global-em-javascript
http://pt.stackoverflow.com/questions/32251/vari%C3%A1vel-global-em-javascript/32255#32255
http://javascriptbrasil.com/2013/10/11/escopo-de-variavel-e-hoisting-no-javascript-explicado/
https://alextercete.wordpress.com/2010/01/15/o-problema-com-as-variaveis-globais-no-javascript/

##IIFE
http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/
http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/
http://blog.da2k.com.br/2015/02/20/os-segredos-da-iife/
http://www.devmedia.com.br/padronizacao-com-iife-amd-e-requirejs/31031
http://desenvolvimentoparaweb.com/javascript/javascript-iife-conteiner-de-codigos/
http://benalman.com/news/2010/11/immediately-invoked-function-expression/#iife
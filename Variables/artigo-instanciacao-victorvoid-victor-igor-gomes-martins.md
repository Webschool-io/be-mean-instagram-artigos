#Instanciação com JavaScript  
#####Victor Igor

Primeiro de tudo, você deve está se perguntando, *"qual o objetivo desse artigo ?
Ensinar declarar uma variável no JavaScript ? Vou aprender alguma coisa ? Vai ser importante para mim ?*"
##Resumo
O objetivo não é ensinar a declarar uma variável ou lhe encher de teorias (também não tem como fugir totalmente delas).
Se você realmente quer saber como funciona **JavaScript**, e quer ter uma opinião para defender-la, pra isso não há nada
mais importante que ficar sabendo a fundo o assunto, não é ? Portanto veremos alguns conceitos que muitas 
vezes não sabemos, por acontecer *'escondido'*. 

1. O que é ?
2. Como usa ?
3. Como funciona ?

Hoisting, Closures, Variáveis globais, Variável por parâmetro e instanciação usando uma IIFE, com JavaScript.

Palavras chaves:
--------------
> hosting, closures, variáveis, javascript, instancia, escopo

#Hoisting
Não precisa ser um gênio para entender. Muitos que não sabem seu significado pode está pensando 
*"Nossa, deve ser algo difícil!"*, mas na verdade precisa mais de atenção, a linguagem usa e você não tá percebendo, porém pode causar bastante confusão se não souber da sua existência, por ter comportamentos *'ocultos'*. Então o que significa **Hoisting** ? Traduzindo para português, seria *'elevação'*, e isso tem todo sentido no JavaScript. 
Antes de mostrar um exemplo com hoisting, é importante entender como funciona a declaração de uma variável no 
javascript.

```javascript
function getNome(){
	var nome = "victor";//declarando e atribuindo
	return nome;
}//victor
function getNomeCachorro(){
	var nome;
	nome = "Calango JR";//atribuindo
	return nome;
}//Calango JR
```
> A palavra reservada **var** especifica que a variável seja do escopo atual, com isso, usando a variável *nome* em 
outras funcoes não irá atrapalhar, pois ela apenas faz parte do seu escopo.

```javascript
var cachorro;//declarando uma variável global
function setNomeCachorro(nome){
	cachorro = nome;
}
function modificaNome(){
	cachorro = 'spike';
}
setNomeCachorro("calango");
modificaNome();
console.log(cachorro);//spike
```
> Repare que damos um nome de calango, mas a funcao *'modificaNome()'* altera o nome da variável,
isso acontece por ela ser uma variável global, um conceito importante que ainda veremos melhor.

```javascript
function imprimeCachorro(){
	console.log(cachorro);
}
imprimeCachorro();//ReferenceError: a is not defined
```
> Isso porque não declaramos nenhum cachorro.

```javascript
var cachorro = 'calango';
function imprimeCachorro(){
  console.log(cachorro);
  var cachorro = 'spike';
}
imprimeCachorro();//underfine.
```

> Retorna **underfine** por ter uma variável no seu escopo *mostraCachorro()* declarada, porem não instanciada.
Você pode tá se perguntando *'Como não foi instanciada se logo a cima o cachorro recebe calango ?'* 

A lógica de muitas linguagens seria mostrar *calango*, pois já foi instanciada no topo. Taí um dos porquês que você 
precisa estudar a linguagem como funciona e não apenas sua sintaxe, **JavaScript** diferente de algumas linguagens, 
tem seus comportamentos diferenciado, e se você já estudou C, vai perceber que a sua ação é diferente.
E é aí amiguinho que a mágica acontece, a variávei foi *'hoisteada'*, elevada para o topo.
Isso acontece pois o compilador na verdade, declara todas as variáveis, logo que seu código começa a ser compilado. 

####Como acontece?

```javascript
var cachorro; //underfine - compilador elevando
cachorro = 'calango';//repare que aqui ficou apenas a atribuição
function getNomeCachorro(){
	//var cachorro; underfine - compilador elevando
	console.log(cachorro);
	cachorro = 'spike';//repare, apenas atribuição
}
getNomeCachorro();
```
Logo depois ele executará a funcao `getNomeCachorro()`, e entao ela pula as declarações e começa a executar o 
`console.log(cachorro)`, porém não houve nenhuma atribuição ao cachorro dentro do `getNomeCachorro`, já que foi 
declarada e hoisteada, e com isso ele continuou com o valor `underfine` e imprimiu.

####A mesma coisa pode ser aplicada para uma função.

```javascript
function getQualquerValor(){
	function pegaValor(){
		return 0;
	}
	return pegaValor();
	function pegaValor(){
		return 1;
	}
}
var valor = getQualquerValor();
console.log(valor);//--> 1
```
> Acontece que as duas funções *pegaValor()* foram jogadas para o topo, e em seguida é retornada a última *pegaValor()*, 
pois ela sobrescreve a primeira.

####Como acontece?

```javascript
var valor; //underfine - elevada para o topo
function getQualquerValor(){
	function pegaValor(){ //continua no topo
		return 0;
	}
	function pegaValor(){ //elevada para o topo
		return 1;
	}
	return pegaValor();//1 - prevalece a última
}
valor = getQualquerValor(); //--> 1
```
___
####E agora ? Vai ser o mesmo resultado ?

```javascript
function getQualquerValor(){
	var pegaValor = function (){
		return 0;
	}
	return pegaValor();
	var pegaValor = functino (){
		return 1;
	}
}
var valor = getQualquerValor();//--> 0
```
Agora **cuidado**, estamos usando uma funções anônima, com isso a declaração sobe e as funções *(atribuição)* 
continuam no mesmo lugar, e assim prevalecendo sempre a de cima.

```javascript
var valor; //underfine - elevada para o topo
function getQualquerValor(){
	var pegaValor; //underfine - elevada pelo compilador
	var pegaValor; //underfine - segunda variavel elevada
	pegaValor(){
		return 0;
	}
	return pegaValor();
	pegaValor(){
		return 1;
	}
}
valor = getQualquerValor();//--> 0
```
#Closures

Traduzindo closure, podemos dizer que é algo de encerramento, no sentido de guardar, pôr em um lugar fechado.
JavaScript não é a única a usar essa poderosa técnica, ela veio das linguagens funcionais, mas que acabou difundindo e implementado em outras linguagens como C#.

Um exemplo comum seria:
```javascript
function somador(n1) {
  return function(n2) {
    return n1 + n2;//v1 da funcao de fora
  }
}
```
Mas cuidado, o `somador` não é uma closure, e sim o seu valor que retorna.

```javascript
var closureSoma2 = somador(2);
var total = closureSoma2(5); //--> 7 
```
####Outro exemplo

```javascript
function mostraMensagem() {
    var messagem = "Cuidado, clojure";
    function tela() {
        console.log(messagem);
    }
    tela();//considerado uma closure
}
```

> Note que ao chamar a *tela*, ele usa a variável mensagem, que está no escopo *mostrarMensagem()*,
a função de dentro (inner), vai ter o acesso as variaveis da funcao de fora (outer).

---

```javascript
function conta(n) {
    var cont = n;
    return {
        incremente: function() {
            cont++; // apenas incrementa o valor passado por parametro, e lembre está no escopo de fora
        },
        get: function() {
            return cont; //pega o valor atual
        }
    }
}
var contador = conta(0);
contador.incremente();
contador.incremente();
contador.get();//-->2 
```
###Na prática
Primeiro vamos imaginar que precisamos saber quantas vezes o usuário está clicando no botão. Se você estudou mesmo,
não vai querer usar uma variável global pra sair contando, isso só vai te trazer prejuízo na frente.

###Agora você sabe CLOSURE, use-a

```javascript
var butao = document.getElementById('button');
butao.onclick = (function() { // uso de funcao anônima
    var count = 0;
    return function(e) {
        cont++;          //contando
        if (cont === 1) {
          	alert("Aguarde um momento");
        }
        else if (cont === 2) {
            alert("Eu falei pra esperar!");
        }
        else if (cont === 3) {
            alert("Fica na sua quieto e aguarde");
        }
        else{
            alert("Chato!"); 
        }
    };
})();//cria e executa
```

>"Todos os seres humanos têm três vidas: a pública, a privada, e a secreta." Gabriel García Márquez

Não podemos ficar criando variáveis globais, pois elas ficam livre ao longo do código e provavelmente pode-se
modificar e trazer dor de cabeça. Através de closure, podemos deixar ela escondida, ou seja, **privada**.
Precisamos usar encapsulamento, um conceito das linguagens orientada a objetos, ora mas **JavaScript** é orientada
a objetos ? **NÃO**.
Porém, todavia, JS é linda, podemos criar um mecanismo de privar uma variável usando closure, e se 
você prestar bem atenção, não vai ter nenhuma dúvida.
Vamos criar um exemplo clássico, um banco onde posso depositar e sacar, claro que quero meu dinheiro privado.

```javascript
function banco(){
  var saldo = 0;
  var cofre = {}; 
  function sacar(e){//vem por parâmetro quando você chamar
    if (saldo == 0){
      console.log("Saldo: 0");
      return 0;
    }
    if (saldo - e < 0){
      console.log("Saldo insuficiente para o saque");
      return 0;
    }
    saldo = saldo - e;
    console.log("Novo saldo: "+saldo);
    return e;
  }
  function depositar(e){
    saldo += e;
    console.log("Depositado com sucesso!");
  }
    cofre.sacar = sacar;
    cofre.depositar = depositar;
    return cofre;
}
var p = banco();
p.depositar(100);//Depositado com sucesso!
p.sacar(20);//Novo saldo: 80
```

##Boas práticas
Em projetos grande um dos problemas que acontecem muito é o de comportamento, alguns por está conseguindo acessar 
variáveis que não deviam, vazando escopo, sempre feche o escopo, se organize e não fique criando variáveis globais.

#Variáveis Globais
Falamos bastante sobre não criar variáveis globais, e que isso pode lhe trazer dores de cabeça lá na frente, 
mas por quê ?
Elas podem ser acessadas por qualquer escopo, e possibilitando as chances de você modificar.

1.
```javascript
var mensagem = 'sou global';
function alteraMensagem(){
  var mensagem = 'não sou global';
}
alteraMensagem();
console.log(mensagem);//--> sou global
```
2.
```javascript
var mensagem = 'sou global';
function alteraMensagem(){
  mensagem = 'modifiquei';
}
alteraMensagem();
console.log(mensagem);//--> modifiquei
```
> Na exemplo 2 a função **alteraMensagem**, não foi criado nenhuma variável, apenas atribuição, por isso ela modificou, diferente
do exemplo 1, onde tem uma palavra reservada **var**, onde indica uma declaração no escopo atual.

#Variáveis por parâmetro
É comum demais passarmos parâmetros pela nossas funções, porém no JavaScript não precisamos especificar que tipo
de dados vamos receber.
```javascript
var canguru = 1;
function pular (e){
  canguru += e;
}
pular(4);
console.log(canguru);//-->5
```
Um das coisas curiosas é que ele não verifica nossos argumentos. Como por exemplo:
```javascript
function somar (n1,n2){
  return n1+n2;
}
var resultado = somar(1);
console.log(resultado);//--> NaN 
```
###Mas por quê ?
Simples, como não foi passado por parâmetro, ela é setada como underfined, e você não pode querer somar um número
com underfined.

##E quando existe mais parâmetros do que o esperado ?
```javascript
  function multiplica (n1,n2){
    return n1*n2;
  }
  var num1 = 5;
  var num2 = 10;
  var num3 = 6;
  var total = multiplica(num1,num2,num3);
  console.log(total); //-->50
```
#Instanciação usando uma IIFE
IIFE é comumente chamado de "fechamento" ou envoltório.
###Por que usar isso ? 
Variáveis globais é um enorme risco, onde pode ocorrer colisões de nomes e trazendo grandes consequências e 
possívelmente um dos problemas mais difícieis de detectar.
Com JavaScript é apenas uma questão de adequação, você declara dentro de um escopo local para que ela não 
fique jogada no escopo global, e claro, nunca esquecer da palavra-chave `var`, pois sem ela, a variável é global.

```javascript
function qualquer(){
  var cavalo = 'marrom';
}
```
```javascript
var n = 3;
getValor = (function(e){
  return function(){return e;};
}(n));//perceba que já é chamada, logo não sofre alteração da linha de baixo que muda o n
n = 4;
var valor = getValue();
console.log(valor);//-->3
```
Cuidado ao pensar que seja 4, ele chama logo preservando o contexto e retornando o argumento.

Nesse exemplo sim, ele é chamado apenas depois da atribuição no n.
```javascript
var n, getValor;  
n = 1;  
getValor = function() { 
  return v; 
};  
n = 2;  
var valor = getValor(); //perceba que já houve mudança
console.log(valor); //-->2
```

Dessa forma aprendemos vários conceitos, todos eles são importantes, e alguns possuem até livros específicos como o de [Closure: The Definitive Guide](http://www.amazon.com/Closure-Definitive-Guide-Michael-Bolin/dp/1449381871), então nunca descarte de sempre estudar.

#Bibliografia

- [Mozilla - Closures](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures)
- [W3School - Function Invocation](http://www.w3schools.com/js/js_function_invocation.asp)
- [Eloquente - JavaScript](https://github.com/braziljs/eloquente-javascript)
- [Learning Advanced JS](http://ejohn.org/apps/learn/)

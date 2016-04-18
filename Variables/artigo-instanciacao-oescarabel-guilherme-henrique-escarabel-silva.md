# Artigo - Instanciação
**Autor**: Guilherme Henrique Escarabel Silva - [oescarabel](https://www.github.com/oescarabel)
**Data**: 1461010677163


## Hoisting
Em tradução livre, a palavra hoisting significa: ['içar', 'levantar', 'elevar'], vamos a um exemplo prático:

```
console.log(escola);
var escola = 'Webschool';
```

Você deve se perguntar: "Porque não deu erro, sendo que só declarei a variável escola abaixo da referência?", é porque o javascript iça(hoisting) a *declaração* no topo do escopo, vejamos como o javascript entende:
```
var escola;  //passo3: declaração içada
console.log(escola);  // passo1: opa, encontrei uma variável sem estar declarada
var escola = 'Webschool'; // passo2: encontrei a declaração vou iça-lá
```
**Atenção:** *somente a declaração é içada(hoisted), e não sua inicialização*

*Ou seja:*
```
var escola = 'webschool'; <- inicialização
----------
    |
    v 
declaração
```
Com funções é a quase a mesma coisa, entretanto a sua implementação também vai junto com sua declaração:
```
escola();
function escola() {
    return "Webschool";
}
```
Até mesmo se usarmos uma variável como uma função
```
escola();
var escola = function() {
    return "Webschool";
}
```

Como vimos nos exemplos acima, hoisted é simples(por incrível que parece a definição é mais complicado do que entender sua implementação), mas que confundem muitos programadores(até mesmo os mais experientes se esbarram nisso), por mais que tenhamos esse auxilio do hoisting, é importante sempre declarar antes de usá-la, mas porque? Simples, imagine em um sistema em que existam inúmeras linhas de código, e que você faça parte de uma equipe, todos saberão o que vai ocorrer ali? Além de que você terá que caçar pelo seu código a declaração, e ainda pior, essa variável pode acabar sendo utilizada em outro escopo, sem que você tenha percebido!!. Mas como fazemos para evitar um possível transtorno?

Javascript está em constante evolução e já estamos em fase de implementação do [EcmaScript6](https://github.com/lukehoban/es6features#readme)(a evolução do javascript). Pincelando, com ele surgem 2 novas *keywords* para nos auxiliarem a [let e const](https://github.com/lukehoban/es6features#let--const), ou seja, sendo curto e grosso: uma variável/função sendo let ela não sairá do escopo onde foi declarada.


## Closure

Closure nada mais é do que encapsulamento. Não é de hoje que precisamos esconder informações do usuário e até mesmo da nossa própria aplicação, assim entendermos o conceito de closure é essencial, vamos a um exemplo básico:
```
(function() {
    var curso = 'MEAN';
})();
var professor = 'Suissa'; 
console.log('Você é aluno do curso '+curso+', seu professor é'+professor);
//"curso is not defined"
```
Como visto, não conseguimos acessar a variável curso, pois ela é uma closure, e o interessante é que closures podem acontecer também em funções.

## Variável Global

Variáveis globais, nada mais é que o escopo onde se encontra essa variável, vamos à um exemplo:
```
var global = 0;
function inc(){
   global++;
   return global;
}
console.log(global);
console.log(inc());
console.log(global);
//0
//1
//1
```
De dentro da função alteramos o valor da variável global, e fica nítido que realmente mudamos o valor quando pedimos novamente o valor da variável global, se não quiséssemos utilizar a variável global, teríamos de ter declarado o var dentro da função, ficaria assim: 

```
var global = 0;
function inc(){
   var global = 69;
   global++;
   return global;
}
console.log(global);
console.log(inc());
console.log(global);
//0
//70
//0
```

Desta forma o escopo deixou de ser global e ficou preso ao escopo onde foi declarado, sendo assim, ela só ficará dentro da função **inc()**, vamos à mais um exemplo para fixarmos o conceito de variável global:
```
var escola = 'Nenhuma inserida até o momento';  
function setEscola(nome) {   
    escola = nome; 
};
function getEscola() {
    return escola; 
};
console.log(escola); //Variavel global sem modificarmos
setEscola('Webschool'); //Mudando valor da variavel global
console.log(getEscola()); //Mostrando o valor da variavel global 
//"Nenhuma inserida até o momento"
//"Webschool"
```
Isto que é uma variável global, podemos utilizá-la ao longo de nossa aplicação(atentando ao escopo,não quer dizer que é global que vai funcional em todo e em qualquer lugar de seu sistema[quem usa [AngularJs](https://angularjs.org/) entende muito bem]).

**Bônus:**Normalmente precisamos usar variáveis globais em javascript para poder integrar módulos, funcionalidades, serviços... Mas alguma hora iremos chegar na derradeira pergunta: "E se quisermos não usar variáveis globais tem como?" Hmmmm, não! Alguns riem outros ficam surpresos, entretanto há como amenizar o problema de ficarmos com uma bola de neve descendo uma montanha, que quanto mais desce mais pesada sua aplicação fica, a solução podem ser por meio de:

**Closures Anônimas** 
```
(function(){
        // Tudo o que for declarado(funções e/ou variáveis), fica aqui dentro. 
       // É o nosso mini escopo global desta forma, nós podemos fazer nossas  
      //aplicações ficarem mais rápidas, pois ao invés de procurar no escopo 
     //global gigantesco, trabalhamos em um ambiente bem controlado. 
})();
```

## Variável por parâmetro

São formas de nos comunicarmos com as funções, mas como assim? Vamos supor que nós queremos criar uma função para simplificar nosso processo de multiplicação:
```
function multiplica(num1, num2) {
  return num1 * num2;
}
console.log(multiplica(5,2));
//10
```
Na primeira linha do código, informamos variáveis para que possamos nos comunicar com a função, de modo que, poderemos decidir que dados entrarão naquela função para serem processados, e o mais incrível é que podemos passar qualquer coisa por parâmetro(sério, qualquer coisa mesmo), até funções podemos passar como parâmetro, imagine o leque de possibilidades que podemos fazer com os parâmetros.

## Instanciação usando uma IIFE

IIFE significa Expressão de Função Invocada Imediatamente, ou seja, ela será executada ao mesmo tempo em que ela estará sendo interpretada.Esta é a sua face:
```
(function(){
})()
```
ou
```
(function(){
}())
```
É questão de gosto, Batman ou Superman?, há quem discuta(e morre alegando que é Batman e ponto final[concordo ;3]), mas não está errado nenhuma das formas acima.

Os parênteses que envolvem diz que está é uma expressão:
```
*(*function(){
}*)*()
```
E os parênteses ao final executam a função:
```
(function(){
})*()*
```
Mas para que serve esses últimos parentes?, boa pergunta, mas para respondê-la precisamos pensar em algo: 'Já que ela é invocada imediatamente na interpretação(vulgo, tempo de execução), como poderemos passar parâmetros para ela'?:
```
(function(escola){
    console.log("Seja bem vindo à "+escola);
})("Webschool");
```
Respondida a pergunta? Os últimos parênteses servem para iniciar a expressão e é possível dentro destes parênteses saciarmos alguns parâmetros de dentro da expressão, que foi o caso do parâmetro escola, passamos o valor pela inicialização da expressão com "Webschool"; 




### Artigo - Instanciação de Variáveis em Javascript


Autor: Leonam Carlos da S Souza



## Hoisting


Hoist em inglês significa levantar ou suspender algo através de um aparato mecânico. Em bom português, significa usar o guindaste para elevar um objeto. E é isto o que acontece em JavaScript quando declaramos uma variável ou função. Sua declaração é “elevada” para o topo do escopo.

Apesar de não ser óbvio, o modo como o hoisting funciona é fácil de ser entendido. 

Em JavaScript , uma variável pode ser declarada depois de ter sido usada.
Em outras palavras; uma variável pode ser utilizada antes de ter sido declarada .


```
x = 5; //  Atribuição do número 5 ao x.

elem = document.getElementById("demo"); // Encontra um elemento
elem.innerHTML = x;                     // Mostra x no elemento

var x; // Declara x

```



```
var a = 1

function foo() {
  console.log(a)
  var a = 2
  console.log(a)
}

foo()

console.log(a);

```


Saída: 

```
undefined, 2 e 1.

```

## Closure


Um closure é uma função interior que tem acesso a variáveis de uma função exterior – cadeia de escopo. O closure tem três cadeias de escopo: ele tem acesso ao seu próprio escopo (variáveis definidas entre suas chaves), ele tem acesso as variáveis da função exterior e tem acesso as variáveis globais.

A função interior tem acesso não somente as variáveis da função exterior, mas também aos parâmetros dela. Note que a função interior não pode chamar o objeto arguments da função exterior, entretanto, pode chamar parâmetros da função externa diretamente.

Você cria um closure adicionando uma função dentro de outra função.



```
function showName (firstName, lastName) {
    var nameIntro = "Your name is ";
 
    //esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
    function makeFullName () {
        return nameIntro + firstName + " " + lastName;
    }
 
    return makeFullName ();
}
 
showName ("Michael", "Jackson"); //Your name is Michael Jackson

```
Situações onde você poderia utilizar isto são comuns em ambientes web. Muitos códigos escrito em JavaScript para web são baseados em eventos - nós definimos algum comportamento e então, o atribuimos a um evento que será disparado pelo usuário (quando uma tecla for pressionada, por exemplo). Nosso código normalmente é utilizado como callback: uma função que será executada como resposta ao evento.

## Variável Global

O uso de variáveis globais é geralmente considerado inadequado pois seu conteúdo pode ser potencialmente modificado de qualquer local, e qualquer parte de um código pode depender dela. A técnica possui o potencial de criar dependências mútuas, o que aumenta a complexidade e dificuldade de leitura de um código. Entretanto, para alguns casos seu uso pode ser adequado; um exemplo é a passagem frequente de variáveis continuamente por diversas funções.

Exemplo de uso de variável global em função:

```
var contador = 0;
setTimeout(function() {
    console.log(contador++);
}, 1000);

```
## Variável por parâmetro

Os parâmetros se usam para mandar valores à função, com os quais ela trabalhará para realizar as ações. São os valores de entrada que recebem uma função. Por exemplo, uma função que realizasse uma soma de dois números teria como parâmetros a esses dois números.
Na passagem de parâmetros para uma função é criado um objeto argumentos semelhante a um array com os parâmetros passados, é feita uma cópia dos valores e/ou referências no caso de objetos. 
Quando uma variável é passada por parâmetro de uma função essa variável será utilizada somente dentro daquela função, seu escopo é local, porém se uma variável global for definida na assinatura da função ela deverá ser passada na chamada da função, ou seu valor será undefined. 


```
function somar(a, b){
    return a  + b;
}

somar(8,9);

```

```
var c=7;
var d=8;

function somar(x,y){
    return x + y;
}

somar(c,d);

```

## Instanciação usando uma IIFE

O Javascript tem um problema bastante grave, que é a questão do escopo. Tudo o que você declara fora de uma função, faz parte do escopo global. Algumas linguagens colocam escopo a partir de blocos (if, while, for, etc). Mas em Javascript, se você declarar uma variável dentro de um if, você ainda será capaz de consultar o seu valor, pois o escopo é definido por um bloco de função.
Então, o motivo de usar uma IIFE é para que o seu código não fique sujando o escopo global, e evitar que suas variáveis possam colidir com outras de mesmo nome, que estão no mesmo escopo.
Criando uma função, a função ou variável criada ali dentro (utilizando o var), fará parte do escopo local, e não poderá ser acessada de fora, a não ser que, explicitamente, você a exporte.

Exemplo de uma IIFE:

```
var cool = 'pizza';
(function() {
    var cool = 'pizzaria';
    console.log('Vamos para ' + cool + '!');
})();
console.log('Valor de "cool" é:', cool);

```
Além de ser executada imediatamente, a IIFE mantém o valor da variável global cool inalterado, apesar de redefini-la dentro de seu escopo estático.

Resultando impresso no console:

```
Vamos para pizzaria!
Valor de "cool" é: pizza

```
Uma variável pode receber o valor da uma IIFE quando a IIFE retorna um valor:

```
var cool = 'pizzaria';
var vamos = (function(){ return 'Vamos para ' + cool; })();
console.log(vamos);

```
Resultando  impresso no console:

```
Vamos para pizzaria

```
Uma coisa bacana é que podemos passar argumentos para as funções imediatas. Podemos então ter algo assim:

```
(function(name, hobby) {
  console.log('Hi, my name is ' + name + ' and I like ' + hobby );
}('Fabeni', 'to travel'));

// "Hi, my name is Fabeni and I like to travel"

```
Uma grande vantagem de passar parâmetros para uma função imediata (IIFE), é que esse valor é passado como uma cópia, e não como uma referência. Isto significa que se alterarmos o valor desse parâmetro dentro da IIFE, esse valor não vai persistir fora dela. por exemplo:

```
var myVar = true;

(function(myVar) {
  console.log(myVar); // true
  myVar = false;
  console.log(myVar); // false
}(myVar));

console.log(myVar); // true


```

Isso é bom para criarmos cópias de variaveis globais, e garantirmos que se alguem sobreescrever essa variável, isso não vai influenciar o módulo que criamos. Esse comportamento também é conhecido como Closure.



## Referências Bibliográficas

* http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/
* http://www.w3schools.com/js/js_hoisting.asp
* http://blog.da2k.com.br/2015/02/20/os-segredos-da-iife/
* http://www.raphaelfabeni.com.br/dicas-rapidas-javascript-2/




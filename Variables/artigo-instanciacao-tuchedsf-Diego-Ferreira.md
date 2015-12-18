# Instanciação com Javascript
autor: DIEGO FERREIRA

## Hoisting
Hoist em inglês significa levantar ou suspender algo através de um aparato mecânico. Em bom português, significa usar o guindaste para elevar um objeto. E é isto o que acontece em JavaScript quando declaramos uma variável ou função. Sua declaração é “elevada” para o topo do escopo.

Como em Javascript uma varável/função pode ser declarada depois de ter sido utilizada o javascript utiliza o conceito acima, e eleva a declaração desta variável ou função para o topo do escopo corrente.

Variável Hosting

Ao definir uma variável sua declaração se torna hoisted, fazendo que ela seja declarada antes mesmo da exexucão do método, porém a mesma não é inicializada ou seja, caso uma parte do código faça referência a mesma ela será undefined, pois existirá no escopo porém ainda não foi incializada com um valor válido. Conforme exemplo abaixo:

```
var x = 5;  // Initialize x

elem = document.getElementById("demo");            // Find an element
elem.innerHTML = "x is " + x + " and y is " + y;    // Display x and y

var y = 7;  // Initialize y
```

No exemplo acima o retorno do script será : x is 5 and y is undefined. A variável y se tornou hoisted, sendo sua declaração levada para o inicio do script, porém sua inicialização (var y = 7;) somente é feita após sua utilização (elem.innerHTML = "x is " + x + " and y is " + y), ou seja, no momento da utilização a mesma ainda não foi inicializada.

Abaixo outro exemplo com o mesmo resultado acima:

```
var x = 5; // Initialize x
var y;     // Declare y

elem = document.getElementById("demo"); // Find an element
elem.innerHTML = x + " " + y;           // Display x and y

y = 7;    // Assign 7 to y
```

No exemplo acima ocorre o mesmo do exemplo anterior x e y são declaradas ocorre um hoisting de sua declaração porém "y" não é inicializada, retornando o mesmo resultado:  5 undefined

Uma dica/boa prática para se evitar bugs no seu script é sempre declarar as varáveis no início do script, que assim você organiza seu código e tem um controle maior em casos de scripts complexos, evitando assim muitos bugs.

Função Hosting

Em uma função tanto sua declaração quanto seu ficam hoisted, fazendo com que a mesma possa ser declarada/utilizada em qualquer local do script que não ocorrerá erros "undefined".

Segue exemplo abaixo:

```
foo()
function foo() {
  console.log('bar')
}
```

No exemplo acima será exibido "bar" como resultado conforme esperado, pois foi realizado um hoisting tanto da declaração quando do corpo da função, não importando então o local que a mesma for chamada seja antes ou depois de sua declaração.

Segundo exemplo:

```
function foo(){
  function bar() {
    return 3
  }

  return bar()

  function bar() {
    return 8
  }
}

console.log(foo())
```

No exemplo acima o resuntado será 8, pois como há um hoisted das funções, como a fução bar que retorna 8, posui o mesmo nome "bar" da função que retorna 3, no momento do hoisted que eles forem elevadas ao inicio do script, a segunda função sobrescreve as funções anteriores de mesmo nome. Ou seja,dentro do mesmo script caso existam mais de uma função com o mesmo nome, sempre a ultima função declarada será a função considerada.

***Cuidado***
Como o Javascript permite que uma função seja declarada como expressão ( Ex.: var foo = function(){} ), neste caso o Javascript trata a expressão como uma declaração de variável fazendo o hoisted apenas da declaração e consequentemente retornando erro conforme exemplo abaixo:

```
foo() //=> TypeError
var foo = function() {}
```
No exemplo acima será disparado um erro do tipo TypeError, nos avisando que undefined não pode ser usado como uma função, ou seja, ocorreu o hoisting apenas da declaração.

## Closure

Clousure é uma função declarada dentro de uma outra função e que pode utilizar variáveis e parâmetros da função exterior como fazer o uso de variáveis globais.

Segue exemplo abaixo:

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

Muitos podem estar perguntando em quais situações e para que usar o clousures, então seguem exemplos de situações onde justifica o uso de clousures:

1) Callbacks - Muitas vezes é necessário que uma ação seja realizada sempre após outra ação, por exemplo após determinado tempo que uma função foi chamada exibir uma mensagem e/ou quando o clique do botão deve ser startado evento de mudar de tela. Ou seja, ah uma clousure do evento de mudar tela dentro da função click do botão.

Exemplo:
```
function alertMsg(message) {
    var alerta = function () {
        alert(message);
    };
    return alerta;
};

var informar = alertMsg("Estudando Javascript Closures");

setTimeout(informar, 1000); //alerta "Estudando Javascript Closures” após 1 segundo
```


2) Emular métodos privados - São utéis para encapsular objetos e protejer o acesso a dados privados deste objeto.

Exemplo:

```
          function Estudante(nome) {
    var name = nome;
    var getName = function () {
        return name;
    };

    var setName = function (nome) {
        name = nome;
    };

    return {
        get: getName,
        set: setName
    };
}

var aluno = new Estudante('John');
aluno.nome; //undefined
aluno.get(); //retorna "John"
```

## Variável Global

No Javascript uma variável declarada fora de uma definição de função é uma variável global, e a mesma estará disponível em todo o script.

Segue exemplos de como declarar uma variável local, a mesma pode ser declarada de 3 formas distintas conforme abaixo:

```
1º) var myName = "Richard";

2º) firstName = "Richard";

3º)
var name;
name;
```

Para se utilizar uma variável global dentro de uma função basta realizar a declaração da mesma sem a utilização da palavra var.

Segue exemplo:

```
function showAge() {
    //age é uma variável global porque ela não foi declarada com a palavra chave var dentro da função
    age = 90;
    console.log(age);
}

//age está no contexto global, então está disponível aqui, também
console.log(age); //90
```

## Variável por parâmetro

Os parâmetros de uma função são chamados de argumentos de função. Argumentos são passados para função por valores.

Seguem 3 regras básicas a respeito dos parâmetros:

- Definições das funções não especifica o tipo de dado do parâmetro.

     Ex:

```
function teste (param1, param2){}
```

- Não são realizadas verificações do tipo de parâmetro passado.

Ex.:

```
var idade = “nome”;
function calculaidade(idade) {return idade + 10 }
```

- Não são verificados o número de argumentos recebidos.

Ex:

```
x = sumAll(1, 123, 500, 115, 44, 88);

function sumAll() {
    var i, sum = 0;
    for (i = 0; i < arguments.length; i++) {
        sum += arguments[i];
    }
    return sum;
}
```

Outro detalhe importante é que se uma função muda o valor de um argumento, esta mudança não é refletida globalmente ou na chamda da função.

Exemplo:

```
function square(number) {
  return number * number;
}
```

Contudo, o objeto de referência são os valores, também, e eles são especiais: se a função muda as propriedades de objetos de referências, estas mudanças são visíveis fora da função, como é mostrado no seguinte exemplo:

Exemplo:
```
function myFunc(theObject) {
  theObject.make = "Toyota";
}

var mycar = {make: "Honda", model: "Accord", year: 1998};
var x, y;

x = mycar.make; // x reebe o valor "Honda"

myFunc(mycar);
y = mycar.make; // y recebe valor “Toyota”, ou seja, o valor da propriedade foi alterado pela função.
```

## Instanciação usando uma IIFE

Uma IIFE é uma função que é executada no momento que é definida.

A convenção comum estabelece que para identificar uma IIFE basta verificar se há a presença de um parêntese adicional esquerda e à direita no final da declaração da função.

As IIFE'S foram criadas com o intuito de manter as variáveis dentro de um escopo definido evitando assim que o escopo sofra interferência de variáveis globais e que javascript de outras pessoas possam atrapalhar o código, devido a declaracao de variáveis e funções com o mesmo nome.

Exemplo Declaração Padrão de uma IIFE
```
(Function () {
   // O código aqui é executado uma vez em seu próprio escopo
 }) ();
```

Em uma IIFE a passagem por parâmetro ocorre conforme exemplo abaixo:

```
(Função (a, b) {
   // A == 'Olá'
   // B == 'mundo'
 }) ('Olá', 'mundo');
```

Foram passados os valores "Ola" para variável "a" e "mundo" que foi recebido pela variável "b".

## Referências Bibliográficas
http://www.w3schools.com/js/js_hoisting.asp
http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/
https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures
http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/
https://msdn.microsoft.com/pt-br/library/bzt2dkta(v=vs.94).aspx
http://javascriptbrasil.com/2013/10/11/escopo-de-variavel-e-hoisting-no-javascript-explicado/
http://www.w3schools.com/js/js_function_parameters.asp
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions
https://en.wikipedia.org/wiki/Immediately-invoked_function_expression
http://tutsmais.com.br/blog/javascript-2/o-que-e-iife-no-javascript/
http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/
http://www.abequar.net/posts/javascript-closure

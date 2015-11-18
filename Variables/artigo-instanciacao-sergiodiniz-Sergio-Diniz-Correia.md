# Instanciação com Javascript


Autor: **Sergio Diniz Correia**

----------
# Resumo
Você já parou para pensar o que seria Hoisting? Ou o que seria o escopo da variável? Ou ainda como instanciar uma variável utilizando uma função IIFE? 
Pois bem, vamos da uma olhada do que é e como elas se comportam em JavaScript.


# Hoisting
Em Javascript, variáveis podem ser utilizadas antes de terem sido declaradas. Isto é possível porque o comportamento padrão do Javascript é elevar todas as variáveis para o topo do escopo atual.

Mais o que é escopo? 
Escopo é o ponto onde o “Alvo” se encontra, isto é, o contexto onde os valores e as expressões estão associado. Ele é utilizado para definir o grau de ocultação, visibilidade e acessibilidade das variáveis em diferentes níveis da aplicação.

Então o que é Hoisting? 
Hoisting, elevar ou suspender algo, é o termo utilizado para dizer que quando declaramos uma variável em Javascript, ela é “elevada” para o topo do escopo ou da função.

Vejamos um exemplo:
```javascript
var a = 1 // Inicializando a variável
function be-mean() {
  console.log(a); // variável ainda não declarada dentro deste escopo
  var a = 2; // inicializando
  console.log(a);
}

be-mean();

console.log(a);
```

O resultado deste código é que a variável no escopo principal vai imprimir o valor 1, enquanto a primeira impressão da variável “a” na função be-mean vai imprimir undefined e a segunda vai imprimir o valor 2.
Analisando o código a cima, podemos ver que isto acontece porque existe uma variável “a” sendo declarada no escopo principal, entretanto, dentro da função be-mean(), apesar da variável ter o mesmo nome, “a”, ela se encontra dentro de outro escopo, em um escopo de nível de função, sendo assim, elas possuem referencias de acesso diferentes, e consequentemente, valores diferente. 


# Closure
Closures, fechamento, são funções que conseguem “lembrar” do escopo em que foram criadas, ficando assim, disponível para que possa ser usada dentro de um escopo que ela não pertence. Em outras palavras, é uma função mais interior que pode ter acesso a variáveis de funções mais exteriores. 

```javascript
function wUrName(myName) {  
   var name = “Seu nome é: “;
    return function() {  
    alert(name + myName);  
  };  
} 
```


Neste exemplo, podemos ver que uma função mais internar consegue ter acesso a variáveis mais externas. Um detalhe importante é que o retorno dessa função interna é por referencia, e não por valor, isso quer dizer que se o valor da função mais externa muda antes da função interna retorna o valor, o valor retornado será o mais novo da função externa.


# Variável Global


Variáveis Globais são variáveis que estão disponíveis para acesso em todos os escopos de uma aplicação. 

Exemplo:
```javascript
var x = "Oi eu sou o dollynho"; // variável global  
function vLocal () {  
 console.log(x);
}  
console.log(x);
```
Aqui a variável global está sendo impressa dentro de uma função e também fora da função.

# Variável por parâmetro
A característica de passagem de valor por parâmetros, presente em varias linguagens, também foi herdade pelo Javascript.

Exemplo:
```javascript
functionName(nome, sobrenome) {
     alert(“Seu nome é: “ + nome + “ “ + sobrenome);
}
```
Essa simples função recebe dois valores, um nome e um sobrenome, e exibe uma mensagem concatenando os 2 valores com a mensagem: Seu nome é: .

Podemos passar também uma variável global, dessa forma, a função pega uma referencia por valor dela, o que quer dizer que caso a função altere o valor da variável, a variável global não vai sofrer alteração no seu valor.

# Instanciação usando uma IIFE

Expressão de Função Imediatamente Invocada (IIFE – Immediately Invoked Function Expression) é um design pattern bem popular em Javascript, isso graças a seu uso no padrão de módulos.

O padrão de módulos permiti usar variáveis normais como propriedades de objetos que não são expostos publicamente, e isso é feito através da criação de funções de closure, visto anteriormente, como métodos de objeto, um exemplo disso são os métodos get e set de uma classe, uma vez que não tenho acesso as variáveis mas tenho aos métodos que podem retornar o seu valor.

As expressões IIFE são utilizadas para evitar o Hoisting das variáveis que se encontro dentro do escopo da função, além de permitir o acesso a método públicos protegendo a privacidade das variáveis definidas dentro da função.

Apesar de poderem ser escritas de varias formas, existe uma conversão de declara-las dentro de parêntese, exemplo:
```javascript
var i = 26;  
getI = (function(x) {  
  return function() {
  return x; 
};   
})(i);  
  
getI(); // 26
```

Essa simples função tem uma variável i que tem um valor e para podermos acessar ela, utilizamos a função getI, ela passa i como parâmetro e preserva o seu contexto, fazendo uma referencia de valor.

Tha’s all folks!



# Bibliografia
https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures
http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/
http://www.w3schools.com/js/js_function_parameters.asp
http://www.w3schools.com/js/js_hoisting.asp
Princípios de Orientação a Objetos em Javascript, Novatec - Zakas, Nicholas C, 2014

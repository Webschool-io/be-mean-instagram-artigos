# Artigo - Instanciação no Javascript #

**Autor**: Marcelo Alves

#### Hoisting ####

Antes de falarmos sobre Hoisting, precisamos entender o seu conceito, então vamos lá:

A tradução de HOISTING é == "Levantar"

Seguindo uma linha de pesquisa relacionada ao tema, podemos afirmar que o conceito de hoisting seria o poder do Javascript em colocar as declarações para o topo do escopo.

Através desse comportamento conseguimos declarar as funções após seu uso. Vejamos este exemplo:

Exercicio 01

```js
// o erro é exibido no console
try {
  console.log(x)
} catch (e) {
  console.error('A variável `x` não foi definida')
}
```

Exercicio 02
```js
// UNDEFINED é exibido no console
try {
  console.log(x);
  var x = 5;
} catch (e) {
  console.error('A variável `x` não foi definida')
}
```

Após a execução destes exercícios conseguimos perceber duas coisas. Primeiramente podemos declarar variáveis em qualquer parte de nosso escopo que esta sempre será colocada em primeiro plano. Podemos ver também que apenas a declaração sofre "hoisting" e não a inicialização.

Para podermos exemplificar que "hoisting" também pode ser aplicados as funções, qual será o retorno que será exibido ao usuário nesta função?


alert(MinhaFuncao());

<script>
function MinhaFuncao() {
    function teste() {
        return "oi"
    }
    
    return teste()
    
    function teste() {
        return "olá"
    }
}
</script>

A mensagem que será apresentada no console será "olá". Devido a function "teste()" ser declarada acima da linha de código. Devido ter sida declarada novamente, sendo sobreescrita devido ao hoisting, mesmo que esta nova declaração fosse aplicada após o "return"


#### Closure ####

Podemos pensar em "CLOSURED" como sendo uma funcionalidade muito utilizada na programação orientada a objetos. Isso permite ao Javascript que uma função "memorize" as variáveis e funções utilizadas no escopo. Podemos observar nesse exercício:

<script>
function BemVindo() {
    var mensagem = "Seja bem vindo programador JAVASCRIPT!"
    function f() {
        alert(mensagem)
    }
    f()
}
</script>

BemVindo();   

Nesse exemplo usando "CLOSURE" temos a function "BemVindo()", que dentro dela possuímos outra function "f()" que acessa a variável declarada "mensagem", mesmo não estando inserida no escopo. 


Vamos perceber isso em um exemplo um pouco mais elaborado.

<script>
function adicionar(n) {
    return function(m) {
        return n + m;
    }
}
</script>

adiciona1 = adicionar(1)
adiciona2 = adicionar(2)

var x = adiciona1(10); // 11
var y = adiciona2(10); // 12

Observando esse exercício, percebemos que uma function retorna outra function e esta altera a variável que já havia sido declarada alterando seus valores.

#### Variável Global ####

As variáveis globais são vistas por todas as funções dentro da tag <script>

<script>
var numero= 0;
function teste(){
alert(numero);
}
</script>

Ora neste caso se chamarmos o teste vamos receber o alerta com 0, numero foi definido como uma variável global.

<script>
function teste(){
var numero= 0;
alert(numero);
}
</script>

Neste exemplo só vamos ver o resultado da variável numero dentro da função teste, pois numero foi definido como uma variável local, só esta disponível dentro do scope do teste. 
```

#### Instanciação usando uma IIFE #### 

*Immediately-Invoked Function Expression* (IIFE) = também pode ser chamada de função imediata.
Mais qual seria a finalidade de à usarmos? Vejamos: 

	-Como o próprio nome diz, ela executa a função imediatamente depois de criada.

Utilizamos bastante quando precisamos isolar as variáveis do contexto global.


<script>
var count = (function () {
  var n = 0;
  return function () {
    n += 1
    return n
  }
}());
</script>

console.log(count()) // 1
console.log(n) // ReferenceError: n is not defined
console.log(count.n) // undefined
console.log(count()) // 2

Dessa forma, também conseguimos criar objetos com variáveis encapsuladas no Javascript.

Também podemos enviar variáveis do contexto global para dentro da função utilizando-as como parâmetro. Antigamente, antes do $ ser adotado pelo plugin [jQuery](https://jquery.com/), em projetos reais de páginas web isso era muito comum usar o IIFE com o objeto do jQuery. No exemplo abaixo, nós passamos a deixar de ter que escrever `j Q u e r y` para escrever apenas `$`.

<script>
(function($) {
  $('body').html('JS is the best!');
})(jQuery)
</script>

#### Referências ####

http://tecnologia-simples.blogspot.com.br/2012/04/variaveis-globais-e-locais-javascript.html
https://developer.mozilla.org/en-US/docs/Glossary/Hoisting
http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/

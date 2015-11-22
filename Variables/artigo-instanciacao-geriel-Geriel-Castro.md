##Instanciação com Javascript  
#####Autor: Geriel Castro



##Introdução

Irei abordar um pouco sobre as funcionalidades do javascript, basicamente falar um pouco sobre como o Javascript cria e instancia uma varivel seguindo algums metodos.


###Hoisting

> *Hoist* em inglês significa levantar ou suspender algo através de um aparato mecânico. Em bom português, significa usar o guindaste para elevar um objeto.

É basicamento isso que acontece, toda vez que você declara uma variável ela se "eleva" para o topo do escopo.

**Entendeu?**

Não?!
Pense o seguinte, toda vez que voce declara uma variável ela não é inicializada e recebe um valor undefined.

Vamos para o **10** que fica mais facil.

**Exemplo de Hasteamento de variável:**

```javascript

// EXEMPLO 1

function showName () {
    console.log (name);

    var name = "José beije meu pé";
    console.log (name);
}

showName ();
// Result: Undefined
// Result: José beije meu pé

```

NOTA EXEMPLO 1: A razão para a primeira impressão ser undefined é que a variável local name foi hasteada ao topo da função

Deixa eu explicar com codigo o que realmente aconteceu.

```javascript

// EXPLICANDO O EXEMPLO 1

function showName () {
    var name; // name é hasteada e vem para o topo do escopo: Estatus Underfined
    console.log (name); // Result name: Underfined

    var name = "José beije meu pé"; // Atribui valor a name, name agora é José beije meu pé
    console.log (name); // Result name: José beije meu pé
 }

```
**"Por que você não falou antes Geriel, agora entendi a bagaça**

Simples, né?


**Exemplo de Hasteamento de funções:**

```javascript

// EXEMPLO 2

console.log(showName ("Jose", "beije meu pé"));

var showName = function(name, lastname) {
    return name+' '+lastname;
}
// Result: TypeError: showName is not a FUNCTION

```

Se mudarmos para:

```javascript

// EXEMPLO 2

console.log(showName ("Jose", "beije meu pé"));

showName = function(name, lastname) {
    return name+' '+lastname;
}
// Result: showName is not defined

```

Viu? Agora esta dizendo que a função **showName** não foi declarado.

Vamos mudar novamente para:

```javascript

// EXEMPLO 2

console.log(showName ("Jose", "beije meu pé"));

function showName (name, lastname) {
    return name+' '+lastname;
}
// Result: Jose beije meu pé

```
DUCA NÉ?


###Closure

Você ja deve ter ouvido falar bastante, em algum momento ja usou, mas se não usou com certeza vai usar.

> *"Um closure é uma função interior que tem acesso a variáveis de uma função exterior."*

Trocando em miudos: Uma função filho acessando uma variavel da função pai.

Para fazer isso funcionar, basta criar uma função dentro de outra função. Dessa forma para ter acesso a variavel pai não é necessario passar valores por parâmetro.

**Deixa eu exemplificar:**

```javascript

// EXEMPLO 3

function showNamePai (name, lastname) {
    var namepai = "José beije meu pé, ";

    // Esta função showNameFilho tem acesso as variáveis da função exterior(showNamePai)
    function showNameFilho () {
        return namepai + name + " " + lastname;
    }
    return showNameFilho ();
}

console.log(showNamePai ("João", "beije minha mão."));
// Result: José beije meu pé, João beije minha mão.

```

Pense o seguinte: A variavel ```namepai``` apesar de ser um "escopo" pertence a função showNamePai e não a função showNameFilho. Mas a showNameFilho esta acessando ela tranquilamente.

Closures torna os códigos mais compactos e fáceis de ler, além de reaproveitáveis. Saber como utilizá-los elimina muito tempo e retrabalho.


###Variável Global

A linguagem JavaScript tem dois escopos: global e local.

Uma variável declarada fora de uma definição de função é uma variável global, e seu valor será acessível e modificável em todo o seu programa.
Uma variável declarada dentro de uma definição de função é local. Ela é criada e destruída sempre que a função é executada e não pode ser acessada por qualquer código fora da função.

```javascript

// EXEMPLO 4

var namepai = "José beije meu pé, João beije minha mão."; // Global

function showNamePai () {
    var namepai = "Nicolau, por que foges?"; // Local
    return namepai;
}
console.log(namepai); // Result: José beije meu pé, João beije minha mão.

console.log(showNamePai ()); // Nicolau, por que foges?

```

###Variável por parâmetro

Na passagem de parâmetros para uma função é criado uma cópia dos valores e/ou referências no caso de objetos.
Essa variável será tratada como local, mesmo que a mesma seja uma variável global.


**Vamos exemplificar isso como dentro de uma função**

```javascript

// EXEMPLO 5

function showCode () {
    var num = 10;
    function showPar (resultPar) {
        resultPar += 1;
        console.log(resultPar);
    }
    showPar(num); // Result: 11 por que ele entrar na função primeiro

    console.log(num); // Result: 10, por que ele pega a variavel global
}
showCode();

```

**Vamos ver a mesma função passando como variável global**

```javascript

// EXEMPLO 5

var num = 10; // Variavel Global

function showPar (resultPar) {
    resultPar += 1;
    console.log(resultPar);
}
showPar(num); // Result: 11 por que ele entrar na função por parametro
console.log(num); // Result: 10, por que ele pega a variavel global

```


###Instanciação usando uma IIFE

O IIFE significa “Immediately-invoked function expression”, ou podemos chamar de função imediata. Ela executa a função imediatamente depois de criada.
Ela é bem úttil para manter a limpesa do codigo tambem.

**Vamos para um pequeno exemplo**

```javascript

// EXEMPLO 6

//Função imediata com parâmetro
(function imediata(num) {
    console.log(num);
})(5);

// Agora com função anônima
(function(num) {
    console.log(num);
})(5);

```

##Bibliografia

- [Pedro Araújo] (http://pedrotcaraujo.github.io/2014/12/01/funcoes-imediatas-IIFE/)
- [Tableless] (http://tableless.com.br/elevacao-ou-javascript-hoisting/)
- [Loopinfinito] (http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/)
- [Javascriptbrasil] (http://javascriptbrasil.com/2013/10/11/escopo-de-variavel-e-hoisting-no-javascript-explicado/)
- [Sitepoint] (http://www.sitepoint.com/demystifying-javascript-variable-scope-hoisting/)
- [Stackoverflow] (http://pt.stackoverflow.com/questions/1859/como-funcionam-closures-em-javascript)
- [Hugobessa] (https://www.hugobessa.com.br/posts/entendendo-escopo-e-hoisting-no-javascript/)
- [Tutsmais] (http://tutsmais.com.br/blog/javascript-2/o-que-e-iife-no-javascript/)

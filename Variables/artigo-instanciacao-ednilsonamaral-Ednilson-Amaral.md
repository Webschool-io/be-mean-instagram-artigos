# Instancias em Javascript  
Autor: Ednilson Amaral


Salve salve nerds! Apresentarei a seguir como o Javascript cria e instancia as variáveis, através de hoisting, closure, variável global e por parâmetro, usando uma IIFE.


A cada dia que passa, o Javascript nos surpreende com alguma coisa nova ou mesmo algo que não conheciamos. Isso ocorre desde para _noob_ iniciante tipo eu (:P) ou mesmo para experts.


Antes de iniciar diretamento nos tópicos citados acima, gostaria de dar uma atenção ao **escopo em Javascript**. Sabemos que o escopo em Javascript é diferente do que em outras linguagens de programação, como Java, C/C++. Enquanto essas linguagens possuem escopos baseados em blocos, o Javascript tem escopo baseado em funções.


Sempre devemos lembrar que **escopo em Javascript é apenas com uso de funções**, e isso é _AWESOME_! Mas por que? Simples, parça!! ;)


Com o uso de **closures**, que falarei mais a frente, podemos criar escopo em qualquer parte do nosso códigozin. Legal, né? Peraí, melhora! :)


Hoisting
---------  

Traduzindo do inglês, _hoist_ significa levantar ou suspender algo, em miudos, usar um guindaste para elevar alguma coisa pesada ou não.  

No _hoisting_ em Javascript é isso que ocorre quando uma varaível ou função é declarada, ela é "elevada" para o topo do escopo.  

Existem dois tipos de _hoisting_, no qual veremos a seguir cada um deles.


### Variable Hoisting  

Assim que definimos uma variável, ela é _hoisted_, porém não sua inicialização. Ou seja, a declaração da variável vai para cima do escopo, antes mesmo do código em si ser executado.  

Mesmo essa variável sofrendo uma elevação e indo para cima do escopo, ela não receberá nenhum valor, a principio. Irá permanecer como `undefined`.  

Vejamos o seguinte trecho:  

```  
console.log(x);  
```

Rodando no console, receberemos um erro, pois a variável `x` não foi declarada para utilizarmos o `console.log()`.


```  
console.log(x);  
var x = 5;  
```

Já nesse código, ainda teremos `undefined`. Como assim? Mesmo depois que fizermos a declaração a variável `x`? Pois é, isso mesmo tio! Mas o que de fato ocorreu aí que ainda continuou como `undefined`? A resposta é simples, ocorreu um **hoisting** da declaração da variável `x`, porém ela não foi inicializada.


Mas tio, e se eu fizer assim:  

```
var x;  
console.log(x);  
x = 5;  
```

Ele ainda vai continuar recebendo `undefined`. Pelo mesmo motivo, ocorreu um **hoisting** da declaração da variável, mas **não** da sua inicialização.


> Apenas a **declaração** de uma variável é _hoisted_, a sua inicialização não!


### Function Hoisting  

Com funções, o _hoisting_ ocorre de uma maneira um pouco diferente. Ele não dá um _hoisted_ apenas no nome da função, mas também em todo o seu corpinho!  

```  
bebedeira();  
function bebedeira(){  
   console.log('Cerveja, cerveja, cervejaaaa!');  
}  
```

O que será impresso com esse código? **Cerveja, cerveja, cervejaaaa!**. Mas por que? Ao contrário da _variable hoisting_, na _function hoisting_ ele irá dar um _hoisted_ tanto no nome da função como no seu corpinho!

Vejamos mais um exemplo:  

```  
function bebedeira(){  
   function cerveja(){  
      return "Skol";  
   }  

   return cerveja();  

   function cerveja(){  
      return "Brahma";  
   }  
}  
console.log(bebedeira());  
```

Como você pode perceber, o resultado impresso na tela é **Brahma**. Mas não imprimiu isso porque ela é mais gostosa que a Skol, deixemos isso claro. HEHEEHHEHE'  

Explicando o código e o porque de imprimir **Brahma**, foram declaradas duas funções `cerveja()`, uma já no topo, e um no final do escopo. E independente de ela estar no fim, também é _hoisted_. E, como já temos uma função com o mesmo nome, ela apenas **sobreescreve a primeira função**. Legal, né? Aprendi isso hoje! rsrs'


Closures
--------

Basicamente, uma _closure_ é uma função interior que tem permissão para acessar as variáveis e parâmetros de uma função exterior. E para criar uma _closure_ basta criar uma função e dentro dela mais uma função, como veremos no código a seguir:  

```  
function cerveja(melhorCerveja, piorCerveja){  
   var intro = "Na minha humilde opinião, a melhor cerveja é ";  
  
   function melhorPior(){  
      return intro + melhorCerveja + ", enquanto a pior é " + piorCerveja;  
   }  
  
   return melhorPior();  
}  
  
cerveja("Brahma", "Bavaria");  
```  

Nesse código será impresso **"Na minha humilde opinião, a melhor cerveja é Brahma, enquanto a pior é Bavaria"**, certo?! E nele, podemos entender o conceito básico de _closures_. Onde a `function melhorPior()` utilizou a variável `intro` da `function cerveja()`, e, também, seus parâmetros `melhorCerveja` e `piorCerveja`.  

Na prática, as _closures_ são ligadas diretamente com programação orientado a objetos, já que os objetos permitem associar dados utilizando um ou vários métodos.  

Assim, é possível utilizar um _closure_ de qualquer lugar do seu código, já que habitualmente seria utilizado um objeto de um único método.


Mas não vá criando funções dentro de outras funções por aí se os _closures_ não forem necessários para uma determinada tarefa. Se fizer isso, a performance do script será afetada de uma forma bem **negativa**, seja na velocidade de processamento ou no consumo de memória.


Alguns chegam a considerar os _closures_ como grande sofisticação, tornando os códigos mais compactos e fáceis de ler, além, é claro, de serem reaproveitáveis. Porém, de nada adianta ter sofisticação se não saber como utilizá-las, com o intuito de eliminar tempo e retrabalho.


Variável Global
----------------


Se definirmos uma variável `var x = 5;` fora de qualquer função, no topo do escopo, ela será definida como **global**, por exemplo:  

```  
var x = 5;  
  
function num(){  
	return x;  
}  
  
num();  
```  

Agora, se definirmos `var x = 10;` dentro da função, ela passa a ser local. Ou seja, só poderá ser acessada dentro dessa função. Isso devido ao uso do comando `var` para declararmos a variável, que a tornará local dentro a função especifica:  

```  
function num(){
	var x = 10;  
	return x;  
}  
  
num();  
```  

Vejamos mais um exemplo:  

```  
function num(){
	x = 10;  
	return x;  
}  
  
num();  
```  

Nesse exemplo, simplesmente por não utilizarmos `var`, a variável `x = 10;` tornou-se global, podendo ser acessada de qualquer lugar de nosso códigozin.


Variável por Parâmetro
-----------------------

Como bem sabemos, em determindas funções passamos alguns parâmetros. Esses parâmetros são acessados através de variáveis globais ou locais de outra função. Podendo assim, passar os parâmetros e acessar de outras variáveis, sem modificar as variáveis ou as modificando. Vamos ao código, fica melhor.. :p

```  
function soma(num1, num2){  
   x = num1 + num2;  
   console.log(x);  
}  
  
soma(25, 10);  
```  

No código acima, é possível notar que passamos os números `25` e `10` como parâmetros da função `soma(num1, num2)`, e, então definimos a variável `x` para receber os valores passados por parâmetro.


Instanciação usando uma IIFE
-----------------------------

IIFE ou _Immediately-invoked function expression_, também conhecida como **função imediata** ou **anônima** nada mais é do que uma função executada imediatamente após ter sido criada. Mas pra que diabos isso maluco? Já ouviu falar de **encapsulamento**? Então, tá aí parça.  


```  
(function hello(){  
   console.log("Salve trua véi!");  
}())  
```

A sintaxe básica para criar uma IIFE é:  

```
(function nome(){  
	//aqui vai o código  
}())  
```

Onde colocando toda a função dentro de **()** irá a tornar em anônima e os **()** antes de encerrar a função fará com que ela seja invocada e executada imediatamente.  

Como sabemos, em programação orientada a objetos tradicional encapsulamento nada mais é que esconder uma ideia, não expondo detalhes internos, tornando assim algumas partes do sistema mais independente.  

Quando criamos uma função anônima com execução imediata, é possível criar um escopo temporário para as nossas funções e variáveis. Assim, acabamos evitando uma poluição chata em nosso escopo global, além de possívels conflitos de variáveis ou funções que tenham o mesmo nome.


Fontes
-------


[Hoisting e escopo em JavaScript - Loop infinito](http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/)  
[Javascript Scoping and Hoisting](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html)  
[Hoisting - Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)  
[Entenda Closures no JavaScript com Facilidade](http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/)  
[Closures - Mozilla Developer Network](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures)  
[Função por Parâmetro - Códigofonte.net](https://www.codigofonte.net/dicas/javascript/709_funcao-com-parametro)  
[Passando função como parâmetro no JavaScript com Node.js - NodeBR](http://nodebr.com/passando-funcao-como-parametro-no-javascript-com-node-js/)  
[O que é IIFE no JavaScript? - Tutsmais](http://tutsmais.com.br/blog/javascript-2/o-que-e-iife-no-javascript/)  
[Sobre funções imediatas JavaScript (IIFE) - iMasters](http://imasters.com.br/front-end/javascript/sobre-funcoes-imediatas-javascript-iife/)  
[Modularização em JavaScript - Tableless](http://tableless.com.br/modularizacao-em-javascript/)  
[Immediately-Invoked Function Expression (IIFE)](http://benalman.com/news/2010/11/immediately-invoked-function-expression/)  
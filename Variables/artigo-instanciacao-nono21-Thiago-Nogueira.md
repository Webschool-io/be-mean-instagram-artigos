# Artigo
**autor**: Thiago Nogueira

Explique, com teoria e código, nesse artigo como o JavaScript cria e instancia as variáveis, seguindo os seguintes tópicos.

## Hoisting

Explique o que é, o porquê acontece e como acontece com variável e função.
```
É o comportamento padrão do JavaScript, fazendo com que as declarações sejam movidas para o topo.
Em outras palavras, significa que uma variável pode ser usada antes mesmo de ser declarada. Porém isso só funciona com declarações e não com inicializações.

Exemplo de Hoisting(Declaração):
x = 2;
test = function (a) {
  return a * 2;
};
for(i = 0; i < 2; i++) {
 console.log(test(x));
}
var x;
var test;

Exemplo onde não tem Hoisting(Inicialização):
var x = 2;
var test = function (a) {
  return a * 2;
};
for(i = 0; i < 2; i++) {
 console.log(test(y));
}
var y = 3;

```



## Closure

Explique o que é, o porquê acontece e como usar.
Cite situações que você usaria.

```
Closure nada mais é que qualquer função que mantém referência a variáveis do escopo da função "pai"(exterior), mesmo que essa função "pai"(exterior) já tenha sido retornada.

Motivos para usar:
- Usar como função construtora para criar funções que são relacionadas de alguma maneira.
- Usar closures como funções construtoras é uma boa maneira de manter o seu código JavaScript DRY.
- Muitas linguagens orientadas a objetos lhe dão a possibilidade de declarar métodos como públicos ou privados. O JavaScript não tem     essa funcionalidade por padrão, porém temos a possibilidade de emular essa funcionalidade através do uso de closures.
- Melhorar o namespacing de funções privadas.

Closure para função construtora:

  function myJob(title) {
      return function(prefix) {
          return prefix + ' ' + title;
      };
  }

  var dev = myJob('Software Developer');
  var manager = myJob('IT Manager');

  alert(dev('Top'));  // Top Developer
  alert(manager('Assistant to the Regional')); // Assistant to the Regional IT Manager
  alert(manager('Regional')); // Regional IT Manager

  Closure para namespacing funções privadas:

    var mySalary = (function() {
      var salary = 60000;
      function changeBy(amount) {
          salary += amount;
      }
      return {
          raise: function() {
              changeBy(5000);
          },
          lower: function() {
              changeBy(-5000);
          },
          currentAmount: function() {
              return salary;
          }
      };
  })();

  alert(mySalary.currentAmount()); // $60,000
  mySalary.raise();
  alert(mySalary.currentAmount()); // $65,000
  mySalary.lower();
  mySalary.lower();
  alert(mySalary.currentAmount()); // $55,000

  mySalary.changeBy(10000) // TypeError: undefined is not a function
  ** A variável salary e a função changeBy não são acessíveis fora de mySalary.

```
## Variável Global

Explique como se usa uma var Global dentro de uma função.

```
Todas as variáveis declaradas fora de uma função são globais e estão acessíveis num escopo global, ou seja, podem ser usadas dentro das funções. Se uma variável for inicializada antes de ser declarada com a palavra "var" essa variável é adicionada ao contexto global, obviamente se tornando uma variável global.

Exemplos de variáveis globais fora de uma função:
​
  ​var myName = "Richard";
  firstName = "Richard";​​
  ​var name;

Exemplo de variável global dentro de uma função:

  function showAge () {
  	// age é uma variável global porque não foi declarada com "var" na função.
  	age = 90;
  	console.log(age);// ​
  }
  ​
  showAge (); // 90​
  ​
  ​// age está no contexto global, então está acessível aqui também.​
  console.log(age); // 90



```
## Variável por parâmetro

Explique o que acontece dentro da função qnd um parâmetro é passado e também explique quando uma GLOBAL é passada por parâmetro.

```
Quando passamos um parâmetro para uma função estamos na verdade enviando apenas o valor desse parâmetro e não a sua referência, por isso quando passamos uma variável global por parâmetro para uma função, mesmo que essa função manipule esse parâmetro não irá afetar seu valor original no escopo global.

Exemplo de como uma variável global passada por parâmetro não muda seu valor:
  var number = 10;

  function test(n) {
    number = number + 2;
  }

  test(number);
  console.log(number);

```


## Instanciação usando uma IIFE

Explique como uma variável pode receber um valor de uma IIFE.
Explique como passar uma variável por parâmetro para a IIFE e acontece com ela dentro da função.

```
Isolar suas variáveis em uma IIFE é uma boa prática em JavaScript. Isso evita muitos problemas em termos de sobrescrever variáveis globais.

O uso comum de IIFE em variáveis é para criar objetos com várias funções agregadas, onde pode também conter informações privadas que são compartilhadas apenas com a IIFE.
Exemplo:
  var MyApp = (function() {
      var privateModuleInformation;
      var morePrivateModuleInformation;
      // ...

      function doSomething() {
          // ...
      }

      function doAnother() {
          // ...
      }

      function doTheLast() {
          // ...
      }

      return {
          doSomething: doSomething,
          doAnother: doAnother,
          doTheLast: doTheLast
      };
  })();

Como passar uma variável por parâmetro para uma IIFE:
  (function(window, $) {
      // window e $ vão ser variáveis encapsuladas dentro da IIFEE, e assim podemos usar essas variáveis fora da IIFEE para outros fins.
  })(window, jQuery);


```

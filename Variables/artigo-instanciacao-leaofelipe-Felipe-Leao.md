## Instânciamento de Variáveis em Javascript (Iniciante e Intermediário)

Este artigo é indicado para quem quer se aprofundar um pouco mais em Javascript e gostaria de receber algumas dicas e boas práticas sobre instanciamento e tratamento de variáveis.  

De início, devemos saber como declarar uma variável:
```javascript
var nome;
nome = 'Felipe';
console.log(nome);
```

Simples não?
No exemplo acima podemos ver:

- A Variável _nome_ é declarada;  
- Nome recebe o valor do tipo String `Felipe`;  
- Mostramos o valor da variável _nome_ no console;  

Se quisermos reduzir ainda mais o nosso código, podemos atribuir um valor a variável diretamente em sua declaração, veja:  
```javascript
var nome = 'Felipe';
console.log(nome);
```

Nada muito difícil certo? Porém, existem algumas peculiaridades que devemos levar em consideração quando estamos desenvolvendo em Javascript e que são de extrema importância para o processo de desenvolvimento e boas práticas. Separei essas características da linguagem em tópicos, para que sejam melhor compreendidas.  

### Hoisting:
Hoisting é uma palavra em inglês cujo significado é "Levantar", "Subir".
A partir dessa tradução podemos explicar de forma compreensível o que acontece por trás dos panos quando nosso código é executado.  

Veja o seguinte trecho de código:
```javascript
var nome = 'Felipe';

function foo () {
     console.log(nome);
    var nome = 'Igor';
    console.log(nome);
}

foo();
```

Se você entendeu o primeiro exemplo desse artigo você irá pensar: “O código acima irá imprimir Felipe e Igor”, certo? Eu também achava até executar o código e ver que nós dois *estávamos errados o tempo todo*.  

Ao contrário de algumas linguagens de programação como o C e Java, o Javascript não trata as variáveis a nível de Bloco, mas a nível de Função.
Isso implica que a cada variável que criarmos dentro de uma função (não sendo uma variável global, que será explicada logo abaixo), sua declaração será movida para o INÍCIO do bloco, fazendo com que, no nosso exemplo, os dados sejam sobrepostos devido ao mesmo nome de variável. Por isso, na hora da execução do nosso Script, ele será interpretado assim:
```javascript
var nome = 'Felipe';

function foo () {
     var nome; // A Declaração é movida para o início da função, então nossa variável não tem valor algum agora.
     console.log(nome); // Irá imprimir nossa variável, ou seja, UNDEFINED;
     nome = 'Igor'; // A Declaração “Var” foi transferida para cima, agora temos apenas uma atribuição de valor
     console.log(nome) // Nossa variável agora tem um valor, Igor
}

foo();
```

### Closure:
Como havia explicado no tópico anterior, no Javascript as variáveis são tratadas a nível de Função. Assim, tudo o que é criado dentro de determinada Função, pode ser acessado por todas as suas variáveis ou funções aninhadas.  

Vejamos o seguinte código:
```javascript
function foo () {
     var nome = 'Felipe';

     function mostrarNome () {
          console.log(nome);
     }
     mostrarNome();
}

foo();
```

Podemos ver que a função `mostrarNome` é filha da função `foo`. Isso implica que tudo o que é criado no escopo de `foo` pode ser acessado por suas funções filhas, que no nosso exemplo é `mostrarNome`. A função `foo`, por ser uma função pai, não consegue ter acesso às variáveis de `mostrarNome`, devido ao tratamento funcional de escopo em Javascript. Vamos ver outro exemplo para compreender melhor:
```javascript
function foo () {
     var nome = 'Felipe'; // A Variável nome é definida
     function bar () {
          console.log(nome); // A Função bar consegue acessar as variáveis da função Pai, foo()
          var visitas = 10; // A Variável visitas é criada e definida como 10, porém fica disponível apenas dentro de bar.
     }
     bar();
     console.log(visitas); // A Função foo não consegue acessar a variável visitas de sua função filha bar() devido ao escopo da variável;
}
foo();
```

### Variável Global:
Variáveis globais em Javascript podem ser acessadas dentro de qualquer função devido ao seu escopo global.
Para definirmos uma variável global, basta declará-la fora de qualquer escopo de função. 
Veja o exemplo a seguir:
```javascript
var minhaGlobal = 'Olá!';
function foo () {
     console.log(minhaGlobal);
}
foo();
```

Como a variável `minhaGlobal` está fora de qualquer escopo, ela pode ser acessada em qualquer local do nosso código.

Podemos também declarar uma variável global dentro de uma função ou fora dela, omitindo a declaração `var` no momento da declaração.
Veja o exemplo:
```javascript
function foo () {
     minhaGlobal = 'Olá!';
}
foo();
console.log(minhaGlobal);
```

Uma boa prática é evitar variáveis globais. Tente limitá-las apenas para `Namespaces` e tome muito cuidado ao utilizá-las, pois a medida que nosso código cresce, há o risco de sobreescrevê-la.

### Variáveis como parâmetro de função:
Assim como em qualquer linguagem de programação, é possível definir variáveis como parâmetro para serem utilizadas dentro de uma função. Devemos compreender que as variáveis definidas como parâmetro recebem sempre o valor `UNDEFINED`. Veja:
```javascript
function foo(minhaVar) {
     console.log(minhaVar);
}
foo();
```

Para alterar o valor pré-definido da variável declarada como parâmetro podemos, podemos estabelecer um valor.
Vamos tentar novamente:
```javascript
function foo(minhaVar = 1) {
     console.log(minhaVar);
}
foo();
```

Mesmo se definirmos uma variável global como parâmetro de uma função, esta é tratada como variável local. Isso faz com que a variável global continue intacta, mesmo sendo alterada dentro da função.
Veja o exemplo:
```javascript
minhaGlobal = 1;
function foo(minhaGlobal) {
     minhaGlobal = minhaGlobal + 1;
     console.log(minhaGlobal);
}
foo(minhaGlobal);
console.log(minhaGlobal);
```

### IIFE (Immediately-invoked function expression)
As `IIFE`, mais conhecidas como Funções Imediatas, são funções que se auto-executam assim que são criadas.  
Veja o exemplo:
```javascript
(function () {
     var nome = 'Felipe';
     console.log(nome);
})();
```

No código acima, além da função anônima se auto-executar, tudo o que está dentro dela é encapsulado. Isso faz com que todo o conteúdo fique protegido de fatores externos, a não ser que sejam passados como parâmetro da função.
Os parâmetros de uma `IIFE` funcionam como os de uma função padrão, porém os parâmetros devem ser passados no momento de sua auto-invocação, como no exemplo a seguir:
```javascript
(function (nome) {
     console.log(nome);
})('felipe');
```

Como as `IIFE` encapsulam seus atributos, é preciso criar um retorno para uma variável externa no caso de utilizarmos algum atributo fora da função.
```javascript
var foo = (function () {
     return 'Felipe';
})();
console.log(foo);
```

---

Espero que este artigo possa ajudar em sua jornada para conhecer melhor o Javascript. Caso tenha alguma dúvida, encontre um erro ou algo que possa ser melhorado, fique à vontade para enviar seu feedback para [felipe@felipeleao.com](mailto:felipe@felipeleao.com)

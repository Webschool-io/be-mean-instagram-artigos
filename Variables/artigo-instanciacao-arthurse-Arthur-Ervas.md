# Instanciação de variáveis no JavaScript
**autor**: Arthur Ervas

## Hoisting

No JavaScript acontece um processo de reorganização do código assim que ele é lido, fazendo com que as declarações de variáveis e funções sejam sempre movidas para o topo do documento (ou escopo) no qual essas declarações estejam.

**Exemplo - variável:**

```
var foo = 'var global';
(function() { 
    alert(foo); // undefined
    var foo = 'var local'; 
})();
```

O undefined ocorre por que a var **foo** foi declarada acima do **alert**, já que o JS reorganizou o código:

```
var foo = 'var global';
(function() { 
    var foo;
    alert(foo); // undefined
    foo = 'var local'; 
    alert(foo); // 'var local'
})();
```

**Exemplo - funções**

```
pokeFunction(); // 'Cynamon'
function pokeFunction() {
    console.log('Cynamon');
} 

var pokeFunction = function() {
    console.log("Secondmon");
};
```

O código acima é reorganizado para

```
function pokeFunction() { // movido para o topo do código (declaração de função)
    console.log("Cynamon");
} 
var pokeFunction; // movido para o topo (declaração de variável)
pokeFunction();  
pokeFunction = function() { // ficou na mesma posição pois é uma entrada de valor na variável
    console.log("Secondmon");
};
```

## Closure

Closure é quando temos uma função interna definida dentro de uma função externa. A função interna pode utilizar as variáveis definidas na função externa, pois elas são acessíveis, o contrário não pode ser feito.

**Exemplo:**

```
function showPoke( pokename, pokeattack, pokeheight ) {
   var pokeIntro = 'O pokemon chama-se ';
    
    function makeIntro() { // Função interna que utiliza variáveis da função externa
       return pokeIntro + pokename + ', tem attack de ' + pokeattack + ' e altura de ' + pokeheight;     
    }

   return makeIntro();
}

showPoke("Pikachu", "80", "1.2"); // "O pokemon chama-se Pikachu, tem attack de 80 e altura de 1.2"
```

Pode ser usado para criar métodos públicos e privados, exemplo: 

```
foo = (function () {
    var privateFunction = function() {
        console.log('BE MEAN!');
    }

    return {
        publicFunction : function() {
            privateFunction();
        }
    }
})();
foo.publicFunction(); // 'BE MEAN!'
foo.privateFunction(); // foo.privateFunction is not a function
```

## Variável Global

As variáveis declaradas externas, ou seja, fora de uma função, são pertencentes ao escopo global do documento, e sendo assim essa variável pode ser acessada por toda a aplicação.
Se uma variável for declarada sem a palavra **var** ela automaticamente é adicionada ao escopo global ou substitui aquela já definida no escopo global.

**Exemplo:**
```
var pokeName = 'Pikachu';

function pokeFunction() {
    
    // Trocamos a var pokeName pelo valor abaixo, assim a variável global será substituída já que não usamos a palavra var antes
    pokeName = 'Cynamon';

    console.log(pokeName); // 'Cynamon'
}
```

## Variável por parâmetro

Funções no JavaScript podem receber parâmetros, eles são definidos um a um separados por vírgula. Esses parâmetros serão atribuídos como variáveis de mesmo nome dentro da função.
**Exemplo:**

```
function pokeFunction(a,b) {
    console.log(a+b);
}
pokeFunction(10,20); // 30
```
**Exemplo usando variável global**

```
var pokeName = "Pikachu";
function changeMon(poke) {
    poke = "Cynamon";
    console.log(poke);
}

changeMon(pokeName); // Cynamon
console.log(pokeName); // Pikachu
```

## Instanciação usando uma IIFE

IIFE são *Imediately Invoked Function Expression*, ou seja, são funções criadas dentro de parênteses onde tudo que é definido ali não faz parte do escopo global, nada existirá fora do escopo, sendo que essas funções são executadas imediatamente após sua criação.

**Exemplo**

```
(function hello() {
  console.log('Olá!');
})(); // Retorna 'Olá' imediatamente, sem que a função precise ser invocada pelo método tradicional hello()
hello(); // Retorna hello is not defined pois a função está entre os parênteses, ou seja, fora do escopo global
```
**Quando passamos uma variável para a IIFE**

```
(function(pokeName) { // Função anônima
  console.log(pokeName);
})('Pikachu'); // Retorna 'Pikachu', porém o parâmetro é apenas válido dentro do escopo da função, e não globalmente
```
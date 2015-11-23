#Artigo

**autor:** Renan Mello Rangel de Oliveira

##Hoisting

**Hoisting** é a forma com que a linguagem JavaScript lida por padrao com as variáveis declaradas no meio do escopo, levando-as diretamente ao inicio do mesmo. O hoisting acontence através do interpretador da linguagem que antes da execução do codigo procura por todas as variáveis no condigo, isso permite o uso da variavel antes de sua instaciaçãos.

**exemplo:**
```
menssagem = "Olá a todos do be-mean";
var menssagem;

funcaoHello();

function funcaoHello(){
   alert(menssagem);
}
```

##Closure

**Closure** é uma função que após ser encerrada ainda pode ser chamada pois ela está salvada na memória.

**exemplo:**
```
function gerarMensagem() {
  menssagem = "Olá a todos do be-mean";

  return function exibir() {
    console.log(messagem);
  };
}
var exibeMensagem = gerarMensagem();

exibeMensagem();

O closure pode ser usado para encapsular funções deixando-as pricadas aquela funcao, ou seja, fazendo com que ela nao fique acessível de qualquer parte do codigo

```

##Variável Global

Variável global é uma variável que está diponível e acessível e todo o código. 

**exemplo:**
```

var numero = 10;

mostraNumero();

function mostraNumero(){
   console.log(numero);
}
```

##Variável por parâmetro

Quando o valor de uma variavel é passada por parametro, fazendo com que a assinatura da função tenha um parametro que recebe o valor da que foi referenciada, esse valor recebido é chamado de argumento. Caso a funcao chamada tenha um argumento, mas em sua chamada o argumento nao foi passado o valor recebido será undefined.Uma variavel global pode ser usada para passagem de parametro por uma funcao, porem quando isso acontece, o argumento recebido é apenas uma copia da mesma, inalterando seu valor inicial.

**exemplo:**
```
function monstraMenssagem(menssagem){
   alert(menssagem);
}

monstraMenssagem("Olá a todos do be-mean");
```

##Instanciação usando uma IIFE

IIFE é sigla para “Immediately-Invoked Function Expression", ela funciona como uma função normal porém é executada de forma imediata, ela é bastante usada quando é preciso trabalhar com variaveis auxiliates ou temporarias. Uma variavel pode receber o valor de uma IIFE caso a mesma retorne um valor.Para passar uma variável como parâmetro para uma IIFE basta coloca-la nos parenteses após a declaração da IIFE.A variavel passada por parametro pra IIFE só é alterada dentro do escopo da mesma função.

**exemplo:**
```

var numero = (function(num) {
    return num;
}(10));
console.log(numero);
```

##Considerações

Através da pesquisa realizada sobre instanciações pude aprender e aprofundar o meu conhecimentro sobre os recursos de instanciações de variaveis 
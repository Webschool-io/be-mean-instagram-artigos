# Artigo
**autor**: Elvisley Souza Pereira

## Hoisting

Sempre que uma variavel é definida, sua declaracao é **hoisted**. Isso quer dizer que a declaracao da variavel vai para cima do escopo antes mesmo do codigo ser executado, porem essa variavel nao recebe nenhum valor e permanece como undefined.

```
//Exemplo 01:

//Criando uma funcao com variaveis hoisting
function hoisting(){
    //declaracao da variavel e adicionada aqui(no topo), por causa do hosting
    console.log(variable); // imprimi undefined
    var variable = "bemean";
}

//Exemplo 02:

//Como a funcao é compilada pela javascript
function hoisting(){
    var variable; //A declaracao da variavel vai para o topo
    console.log(variable); // imprimi undefined
    variable = "bemean";
}

```

## Closure

Sao funçoes que se referem a variáveis independentes. Em outras palavras quer dizer que ela nao tem variaveis locais proprias e com isso utiliza as variaveis que foram definidas na funçao pai.

```
closure();

function closure(){
	var name = "bemean";

    function displayName(){
    	console.log(name)
    }

    return displayName();
}

//Exemplo de uso

var clo = closure("be"); // Executa a primeira vez apenas instanciando a funcao
clo("mean"); //Chama a funcao dentro da outra funcao concatenando o parametro enviado na instaciacao com o parametro passado neste funcao

function closure(x){
    console.log(x);
    return function(y){
    	console.log(x+y)
    }
}

```

## Variável Global

No javascript qualquer variavel declarada fora do escopo de uma funçao é GLOBAL , sendo assim ela pode ser acessada e modificada em qualquer parte.Para criar-mos uma variavel global nao utilizamos VAR na declaracao da variavel.


```

variavel = "bemean"; //Variavel global

console.log(variavel); // Imprimi bemean

variavelGlobal("FullStack");

function variavelGlobal(texto){
	 variavel += " - "+texto;
}

console.log(variavel); // Imprimir bemean - FullStack

```

## Variável por parâmetro

Qualquer variavel passada como parametro para um funçao  sendo ela GLOBAL ou nao é considerada uma variavel LOCAL, ou seja , as alteraçoes feitas nela só poderáo ser vistas dentro do escopo da daquela funçao, as variaves GLOBAIS só seráo modificadas se a funçao retornar o novo valor como referencia.


```

var variavel = "bemean";

console.log(variavel); // Imprimi bemean

variavelGlobal(variavel , "FullStack");

//Recebendo variavel global como parametro
function variavelGlobal(variavel , texto){
	 variavel += " - "+texto;
   console.log(variavel); // Imprimir bemean - FullStack
}

console.log(variavel); // Imprimir bemean

```


## Instanciação usando uma IIFE

IIFE é uma funçao que é executada no momento que é definida. Utilizamos IIFE quando queremos criar um escopo no javascript matendo assim as variaveis declaradas dentro da funcao como variaveis locais.

```
//Exemplo de uma funcao IIFE

(function () {
  var mensagem = 'bemean'
  console.log(mensagem) // Imprimi bemean
}())

console.log(mensagem) // ReferenceError: mensagem is not defined

//Recebendo valor de uma funcao IIFE
(function (string) { // recebendo valor da funcao IIFE
  console.log(string) // Imprimi bemean
}("bemean")) //Parametro sendo passado

```

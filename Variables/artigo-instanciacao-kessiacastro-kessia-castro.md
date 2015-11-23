# Artigo
**autor(a)**: Késsia Castro


Linguagens de programação mais antigas como C/C++/Java e outras, possuem escopo baseado em bloco. JavaScript, por outro lado, é uma linguagem que pode confundir programadores iniciantes por possuir um escopo baseado em função. Então, variáveis que estão fora de uma função são consideradas variáveis globais e variáveis declaradas dentro de uma função são consideradas locais.

##Exemplo:

```
var numero = 5;
console.log(numero) //=> O resultado será 5.

if (true) {
	var numero = 10;
	console.log(numero); //=> O resultado será 10.
}

console.log(numero); //=> O resultado será 10, pois o valor de "numero" foi alterado dentro do if.
```

Nesse exemplo, tanto a declaração da variável "numero" inicial, quanto a declaração de "numero" dentro do bloco if pertencem ao mesmo escopo, então a variável tem o seu valor alterado de 5 para 10 e, quando chamada na última linha, o valor alterado será mostrado.

## Hoisting

Quando se trata de variáveis, o interpretador de JavaScript pode realizar duas funções que nem todo mundo está ciente: declarar e definir. Quando uma variável é criada o interpretador coloca a declaração de todas as variáveis no TOPO de seus respectivos escopos, como se você tivesse declarado a variável no início mesmo que você a tenha declarado no meio do código. Se nenhum valor foi atribuído, o interpretador a coloca com o estado padrão de "undefined".

```
var a;
console.log(a) //=> Será mostrado "undefinied" no console, pois a variável "a" foi declarada, mas nenhum valor foi atribuído a ela.
var b = 1; //=> Nos bastidores, para o interpretador primeiro "var b;" será declarado como "undefined" e depois "b = 1;" será definido.
console.log(b); //=> Será mostrado o valor 1, atribuído a b.
```

Em uma função, mesmo que uma variável não seja definida no início, o console não mostrará um erro, pois o interpretador considera que aquela variável é undefined por padrão e foi declarada no início.

Explique o que é, o porquê acontece e como acontece com variável e função.

## Closure

Explique o que é, o porquê acontece e como usar. 
Cite situações que você usaria.

## Variável Global

Explique como se usa uma var Global dentro de uma função.

## Variável por parâmetro

Explique o que acontece dentro da função qnd um parâmetro é passado e também explique quando uma GLOBAL é passada por parâmetro.


## Instanciação usando uma IIFE

Explique como uma variável pode receber um valor de uma IIFE.
Explique como passar uma variável por parâmetro para a IIFE e acontece com ela dentro da função.


## Considerações

Quanto mais explicado melhor.

Lembre que isso fará parte do seu currículo como aluno e será disponilizado no sistema de vagas, ou seja, o contratante poderá ver todos seus projetos e trabalhos feitos nesse curso.

Boa sorte.

# Envio

1. Fork [esse repositório](https://github.com/Webschool-io/be-mean-instagram-artigos/) 
2. Nomeie seu artigo usando o seguinte padrão: artigo-instanciacao-githubuser-nome-completo.md
3. Adicione seu exercício aqui na Pasta Variables
4. Faça um Pull Requst enviando seu artigo.
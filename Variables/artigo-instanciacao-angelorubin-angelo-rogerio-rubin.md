# Artigo
**Autor**: Angelo Rogério Rubin

**Prazo**: até dia 18 de Novembro de 2015

Explique, com teoria e código, nesse artigo como o JavaScript cria e instancia as variáveis, seguindo os seguintes tópicos.

Explique o que é, o porquê acontece e como acontece com variável e função.

# HOISTING

## O que é Hoisting

Hoisting é o ato do interpretador do javascript mover as declarações feitas (variáveis ou funções) para o topo do seu escopo (global ou local).

## Hoisting de variáveis

No caso do hoisting de variáveis o comportamento é o seguinte, apenas as declarações de variáveis são movidas até o topo, elas não serão inicializadas e nada do que é atribuído a elas é movido (obviamente quando for atribuído algo a variável), somente a sua declaração.

Exemplo de Código - Hoisting de Variável

    console.log(myVar);

	var myVar = 'I am a invisible assign, i hate you hoisting !!!';

	// undefined

Como podemos observar no código acima, o console imprimirá undefined, isso ocorre porque quando a variável sofre o hoisting, ela não é inicializada e nem suas atribuições são movidas ao topo do seu escopo, que neste exemplo é um escopo global.

## Hoisting de Funções

No caso das funções

# Closure

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
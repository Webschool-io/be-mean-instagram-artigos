# Artigo - Garbage Collector
**Autor**: Felipe Lopes Rita - [felipelopesrita](https://github.com/felipelopesrita)
**Data**: 1455843757700

##Era uma vez, uma variável...
Ok, se você não sabe o que é uma variável, não se desespere. Existe um site chamado Google. Nele você pode tirar todas suas dúvidas sem problemas e complicações. Mas essa história que irei contar, não se trata do Google e seu mecanismo de busca.Essa é uma história de um pequeno sujeito chamado `timotio`. Acredite você ou não, mas `timotio` era uma pequena variável quando nasceu. Eu mesmo vi seu nascimento: `var timotio;`. Foi lindo, foi mágico, e foi simples.

Obviamente, nunca tratei `timotio` de maneira diferente do que tratei meus outros filhos um tanto quanto variáveis, e com o tempo, passei a me preocupar com isso, já que nesse mundo da memória Ram, espaços não são infinitos. E se eu sem querer acabasse ficando sem espaço de tantos `"timotios"` que eu coloquei lá? Acontece que papo vai, papo vem, e esqueci que `timotio` existia. Onde ele teria ido parar? O que aconteceu com ele? Ele simplesmente morreu? Bem, pra você que se emocionou com a história de `timotio`, digo que a resposta é sim e não. Que ele morreu, isso é uma certeza, porém, ele não "simplesmente" morreu. Alguém atuou na morte dele. E pasme, não foi por mágica!
![Imagem](http://i.huffpost.com/gen/3325038/images/n-DUMBLEDORE-large570.jpg)

##Garbage Collector - A "mágica" que você talvez não conheça
Se você também é pai de filhos variáveis, e os criou à moda C, sabe que existe maneiras de "dispensá-los" de sua memória Ram, com funções do tipo `free()` da vida, que simplesmente limpam da memória a variável que você deseja e informa como parâmetro. Poréeeeeem, por que você não faz isso em javascript? Teriam todos os programadores javascript sofrido de alzheimer e simplesmente esqueceram-se de usar a bendita função? Acho improvável, não?

O que acontece é que pra melhor ou pior, a função de limpeza da memória deixou de ser explicita em javascript e outras linguagens de auto nível, para se tornar algo que função em segundo plano, implicitamente, limpando toda a "sujeira" que você deixa pra trás, como um coletor de lixo (sim, como você já deve ter percebido, o termo garbage collector vem daí).

Porém, o desafio do Garbage Collector é saber o momento exato em que a variável pode ser coletada. Alguns métodos para a coleta são o por Referência de contagem do garbage collection e o Algoritmo de varredura e rotulação.

###Referência de contagem do garbage collection
Verdade seja dita, esse não é o melhor coletor, e já vemos o porquê. Na verdade, esse algoritmo simplifica a definição de `não preciso mais de timotio` para algo no sentido de `outros objetos não referenciam mais o timotio`. Na prática, um exemplo:
```js
//Aviso prévio: Não sou criativo com nomes
var x = { y: "abc" } //Como vemos, y está referenciado a x
z = x; //E agora está referenciado a z também (cool)
x = 0; // y ainda não pode ser coletado, porque está referenciado a z
z = 0; // Ok, agora y não tem mais referência e y pode ser coletado :)
```

Tudo muito bonito, tudo muito lindo. Agora mais um exemplo:
```js
function cicloDoMal(){
  var x = {};
  var x2 = {};
  x.a = x2; // x referencia x2
  x2.a = x; // x2 referencia x

  return "Parece que o jogo virou, não é mesmo?";
}

cicloDoMal();
```
Nesse caso, nosso querido coletor nunca conseguirá coletar nossas variáveis `x's`, já que elas estão presas em um ciclo. Essa falha é conhecida nesse tipo de coletor.

###Algoritmo de varredura e rotulação
Nesse algoritmo, tratamos a definição de `"não preciso mais de timotio"` para `"timotio esta acessível"`. De forma sucinta e sem comparações infames, esse algoritmo toma conhecimento de uma lista de objetos chamados de roots ou raízes e periodicamente encontra todos os objetos referenciados por esses roots, e então os que são referenciados por ele, etc (recursividade).Començando pelas raízes, o coletor encontra todos os objetos acessíveis e coleta os inacessíveis.

##E afinal, qual é a moral da história mesmo?
Apesar de termos visto sobre ciclos e tudo mais, uma situação desse gênero é atípica na prática, e é por isso que ninguém se importa muito sobre o coletor ou sobre como terminou a vida de nosso querido `timotio`

##Referências
[Gerenciamento de Memória](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Memory_Management)
[Garbage Collector no JavaScript](http://education.codeshare.com.br/garbage-collector-no-javascript/)
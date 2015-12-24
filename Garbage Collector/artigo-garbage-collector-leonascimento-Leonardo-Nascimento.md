# Artigos - Garbage Collector

**Autor**: Leonardo Nascimento de Oliveira- [leonascimento](https://github.com/leonascimento)

**Data**: 1450918820838

![](http://i.imgsafe.org/4c35a6b.jpg)

### Definição

> Garbage Collection é uma forma de gerenciamento de memória. É onde temos um coletor que tenta recuperar a memória ocupada por objetos que não estão mais sendo usados.

A Garbage Collection acontece com mais freqüência quando mais objetos estão sendo criados com mais freqüência.

O algoritmo varia de navegador para navegador, mas o conceito é consistente. Enquanto Javascript vê novos objetos que estão sendo criados e destruídos, emitirá rotinas de Garbage Collection intermitentes que irá diminuir o desempenho.

Há muitas técnicas que você pode aplicar para superar falhas causadas pela coleta de lixo. Vamos a um exemplo real, caso você tenha escrito um jogo em javaScript e a cada segundo você
está criando um novo objeto,quando é feito o gerenciamento da  da coleta de lixo será deixado mais espaço para a sua aplicação.Falando em Aplicações em tempo real é muito importante a reutilização da mesma mémoria.

Uma característica fundamental de coleta de lixo é que o coletor de lixo deve ser capaz 
de determinar quando é seguro para recuperar memória. Ele nunca deve recuperar 
valores que ainda estão sendo usados e deveria recolher apenas os valores que não são mais acessíveis; ou seja, valores que não podem ser referidas através de qualquer das variáveis, propriedades de objetos ou elementos de matriz no programa.

Usando procedimentos simples:  Isto é bem conhecido que a nova palavra-chave indica alocação. Sempre que possível, você pode tentar reutilizar o mesmo objeto de cada vez, adicionando ou modificando as propriedades. 

Isso também é chamado de reciclagem de objeto. 

**Em caso de Arrays:** atribuindo [] é muitas vezes usado para limpar matriz, mas você deve ter em mente que ele também cria uma nova matriz. Para reutilizar o mesmo bloco que você deve usar  arr.length = 0  Isto tem o mesmo efeito mas reutiliza o mesmo objeto de matriz em vez de criar uma nova. 

**Em caso de funções:** Às vezes o nosso programa precisava chamar uma função específica 
mais tempo ou em determinados intervalos usando setInterval ou setTimeout.

Exemplo: 

```JS
setTimeout(function() { doSomething() }, 10);

// Você pode otimizar o código acima, atribuindo a função a uma variável permanente:
var myfunc = function() { doSomething() }
setTimeout(myfunc, 10);
```    

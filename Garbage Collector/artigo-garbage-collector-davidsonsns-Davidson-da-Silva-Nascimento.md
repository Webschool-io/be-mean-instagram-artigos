# Artigo ­ Garbage Collector

**Autor**: Davidson da Silva Nascimento ­ [davidsonsns](https://github.com/davidsonsns)
**Data**:  1451435379956

## Afinal, o que é esse tal "Garbage Collector"?

![Imagem](http://front.saiadolugar.com.br/arquivos/2013/04/captain-planet.jpg)

Vai planeta! Errou feio, não tem nada haver com o Captão Planeta e sua turma de anéis.

**Garbage Collector**Collector**(GC) significa ***"Coletor de Lixo"***, e ele realmente cumpre o que o nome já descreve. Esse cara procura por regiões de memória que não são mais utilizadas pela aplicação e devolve os recursos ao sistema operacional. É um processo usado para a automação do [gerenciamento de memória](https://goo.gl/9pla0h).

Permitindo assim que outros programas possam usar a memória disponível, ou mesmo a aplicação em que foi coletado o **"lixo"**.

> Seu funcionamento pode evitar problemas de [vazamento de memória](https://goo.gl/ZfjZEE), que resulta no esgotamento da memória livre para alocação.

## **"Coletando"** no JavaScript

> O conceito de acessibilidade é a base do gerenciamento de memória no JavaScript 

O interpretador JavaScript possui, através do **"Coletor"**, a capacidade de detectar quando um objeto nunca mais será utilizado pelo programa. Quando se determina que um objeto é inalcançável (ou seja, já não há qualquer maneira de se referir a ele usando as variáveis ​​do programa), ele sabe que o objeto não é mais necessário e sua memória pode ser recuperada. Considere as seguintes linhas de código, por exemplo:

No JS este procedimento é automático e invisível para o programador. Você pode criar todos os objetos que seja necessário em sua aplicação, e o sistema irá ser o responsável pela limpeza, caso as referências não sejam mais acessíveis.

### Mark-and-Sweep

Se baseia na definição de que um objeto não é mais necessário quando é inacessível.

Este tipo de coleta atravessa periodicamente todas as variáveis no ambiente e marca todos os valores referidos por estas. Sendo estas variáveis objetos ou arrays, ele realiza a marcação de forma recursiva, afim de coletar todas as informações. Por recursivamente atravessar esta árvore ou gráfico de valores, o coletor de lixo é capaz de encontrar (e marca) a cada valor único que ainda está acessível. Ele então interpreta que os valores não marcados são inacessíveis e, portanto, são lixo.

Uma vez que um coletor de lixo mark-and-sweep terminou marcando todos os valores alcançáveis, ele começa sua fase de varredura. Durante esta fase, parece a lista de todos os valores no ambiente e remove da memória qualquer que não estejam marcados.

Sua marcação e varredura é realizada de uma só vez, o que provoca uma desaceleração no sistema durante a coleta.

### Reference Counting
Se baseia na definição de que um objeto não é mais necessário quando não possui outro objeto referenciando ele.

Quando um objeto é criado, uma referência a ele é armazenado em uma variável com a contagem de referência do objeto. Quando a referência para o objeto é copiado e armazenado em outra variável, a contagem de referência é incrementado para dois. Quando uma das duas variáveis ​​que detém essas referências é substituído com algum novo valor, a contagem de referência do objeto é diminuída de volta a um. 

Se a contagem de referência chega a zero, não há mais referências ao objeto. Uma vez que não há referências para copiar, não pode nunca mais ser uma referência para o objeto no programa. Portanto, o JavaScript sabe que é segura para destruir o objeto e recolher o lixo da memória associada a ele.

### Exemplos
```js
var variavel = "beMean"; // (1)	
var temp = variavel.toUpperCase(); // (2)
variavel = temp; // (3)
```
(1) - aloca memória para uma `string`

(2) - cria uma nova string

(3) - sobrescreve a referência da string original

Após esse código ser executado, a sequência original "beMean" não é mais alcançável. O sistema detecta esse fato e libera seu espaço de armazenamento na memória para reutilização.

----------

```js
function Menu(title) {
	this.title = title
	this.elem = document.getElementById('id')
}
var menu = new Menu('My Menu')
document.body.innerHTML = '' // (1)
menu = new Menu('His menu') // (2)
```
(1) - `body.innerHTML` é limpo. Então, tecnicamente, seus **"filhos"** são removidos, porque eles não são acessíveis mais. Mas o elemento `#id` é uma exceção. É acessível como `menu.elem`, assim que permanecer na memória. Se verificado, utilizando `parentNode`, ele é `null`.

(2) - `window.menu` é reatribuído, e o `menu` antigo torna-se inacessível e é automaticamente removido pelo **Garbage Collector **.
> Elementos **DOM** individuais podem permanecer na memória mesmo quando o pai é limpo

#### Ciclos
A falha básica com Reference Counting tem a ver com referências cíclicas. 

O ciclo inteiro pode ser lixo, se não há nenhuma maneira para se referir a qualquer um desses objetos de um programa, mas porque todos eles se referem um ao outro, um coletor de lixo de contagem de referência não pode detectar e livre essa memória não utilizado.

Este problema com ciclos é o preço que deve ser pago para um esquema de coleta de lixo simples. A única maneira de evitar esse problema é através de uma intervenção manual. Se você criar um ciclo de objetos, você deve reconhecer esse fato e tomar medidas para assegurar que os objetos são lixo coletado quando eles não são mais necessários.Para permitir um ciclo de objetos a serem lixo coletado, você deve quebrar o ciclo.
```js
function foo(){
  var o = {};
  var o2 = {};
  o.a = o2; // o referencia o2
  o2.a = o; // o2 referencia o

  return "qCoisaNão?";
}

foo();
```

Dois objetos são criados, um referenciando ao outro, formando o ciclo. O algoritmo contador de referências considera que desde que ambos os objetos sejam ***referenciados*** pelo menos uma vez, nenhum deles podem ser coletados, mesmo estes sendo pertencentes ao escopo da função.

## Referências
[JavaScript The Definitive Guide](http://docstore.mik.ua/orelly/webprog/jscript/index.htm)

[JavaScript Tutorial ­ Memory leaks](http://javascript.info/tutorial/memory-leaks)

[Gerenciamento de Memória](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Memory_Management)
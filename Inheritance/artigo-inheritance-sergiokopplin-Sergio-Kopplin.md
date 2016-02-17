# Artigos - Herança na JavaScript

**Autor**: Sergio Kopplin - [sergiokopplin](https://github.com/sergiokopplin)

**Data**: 1455590766665

## Herança

A herança em OOP é a capacidade de classes compartilharem propriedades e métodos entre sí de modo a economizar código, compartilhando assim entre suas classes filhas os métodos que ambos tem em comum.

---

No JS a herança gira em torno de objetos, pois não possuímos Classes dentro da linguagem.

A herança consíste então em um objeto herdando métodos e propriedades de um outro.

Essa herança ocorre por meio de protótipos. Protótipos em JS são críados sempre que instanciamos um array, por exemplo. Esse nosos array herdará toda propriedade de Array e de Object dentro da linguagem.

```JS
// criando o construtor
function Notebook() { ... }

// adicionando métodos
Notebook.prototype.ligar = function() { ... }

// herdando em macbook
Macbook.prototype = new Notbeook();
```

No nosso exemplo, o novo objeto Macbook herdará as propriedades e métodos de Notebook. Logo, poderá ligar().

Sendo assim, poderá funcionar da seguinte maneira.

```JS
Macbook.ligar();
```

---

Muitas outras propriedades são envolvidas nesse processo e podem ser consultadas nas referências. Até a próxima.

---

Referências:
[http://loopinfinito.com.br/2012/05/04/heranca-em-javascript-parte-1/](http://loopinfinito.com.br/2012/05/04/heranca-em-javascript-parte-1/)
[http://loopinfinito.com.br/2013/02/05/heranca-em-javascript-parte-2/](http://loopinfinito.com.br/2013/02/05/heranca-em-javascript-parte-2/)

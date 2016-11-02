# Artigo - Herança

**Autor**: Alex Morgado Pereira - [AlexMCoder](https://github.com/AlexMCoder)

Javascript é uma linguagem orientada a objetos, nas outras linguagens de programação a orientação a objetos em relação as heranças são baseadas em classes, no javascript é baseado em prototype, você consegue emular classes através de funções. Vale ressaltar que o Javascript não tem classes, apenas objetos, você consegue instânciar novos objetos a partir de outros objetos.

Com os prototype você consegue mudar os objetos globais. Porém isso torna perigoso, não é uma boa pratica, pois os objetos globais são definidos por um padrão, se mudar o comportamento pode ser perigoso correndo o risco de perder os beneficios.

É possível programar orientado a objetos com Javascript, porém, aqueles que são acostumados com orientação a objetos utilizando Java, irá se assustar com a forma como o JavaScript é tratado.

As instâncias é muito importante saber o uso delas, elas servem para criar um objeto, para isto, utilizamos o operador **new**, juntamente com a função construtora, responsável pela inicialização do objeto.

Existe outra forma para iniciar objetos, uma delas que é bem utilizada em JavaScript são os **Literais de Objetos**. Ela é definida por uma lista de nome e valor, são separados por virgulas entre um par de chaves.

##Exemplo de Herança no JavaScript

```JS
function Aluno(nome, matricula,sexo) {
var nome = 'alex';
var matricula = 5848;
var sexo = 'masculino';

this.getNome = function () {
return nome;
};

this.getMatricula = function(){
return matricula;
}

this.getSexo = function(){
return sexo;
}

}
```

##Exemplo do JavaScript Orientado a Objetos

```JS
var professor = {

darAula: function() {

},
resolverExercicios: function() {

},
passarAlunoDeAno: function() {

}
}

var AlexMCoder = Object.create(professor)
AlexMCoder.darAula()
```

##Literais de Objetos

```JS
var Escola = {
nome : "BeMEAN",
local : "GitHub",
}

// Acessando as propriedades:
console.log('Nome da Escola: + 'Escola.nome + /n + " Local: " + Escola.local);

```
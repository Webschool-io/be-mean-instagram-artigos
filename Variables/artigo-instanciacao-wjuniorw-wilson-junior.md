# MongoDB - Artigo
Autor: Wilson junior

---
## Hoisting
Em JavaScript, uma variável pode ser declarada depois de ter sido usada.

Sendo assim voce pode usa-la antes mesmo de fazer sua declaração!
como assim?
Exemplo:
```
//mudando o valor da variavel antes da declaraçao
ola = 'Ola mundo'; //inicializada
//usando a variavel
var body = document.querySelector('body');
    body.innerHTML = ola;
var ola; //declaraçao da variavel
```
Aí vc diz: meu código funciona!!! mas como? huehue

Isso acontece por causa do processo de "hoisting", esse comportamento é padrão do JavaScript, ele leva todas declarações para o topo do script ou do escopo ha que ele pertence (no caso de ela ser declarada dentro de uma função).<br>
Bem sacado né, mas eu acho ideal que se declare todas as variaveis no topo já que é assim que a linguagem lê o codigo.<br>
E tem mais, é bom ter em mente que **"declarar"** é diferente **"inicializar"** um variavel Exemplo:
```
var ola; //declaraçao da variavel
    ola = 'Ola mundo'; //inicialização da variavel

```
Isso é importante porque o hoisting só ocorre com a "declaração" e nao com a "inicialização" da variavel.<br>
Exemplo:
```
try {
  var a;
  console.log(a);
  a = 2;
} catch (e) {
  console.error('a não existe no contexto atual');
}
```
Ela vai existir só que vai ser undefined pois ela só é inicializada depois da execução do codigo, ou seja não ouve hoisting na inicializacao.

#### Hoisting com funçoes
É bem semelhante. <br>
funçoes nomeadas funcionam com hosting:
```
fnHoisting();//function com hoisting
function fnHoisting () {return 'function com hoisting';}
```
já uma função atribuida ha uma variavel não:
```
fnHoisting();
//nao functiona com hoisting "TypeError: fnHoisting is not a function"
var fnHoisting = function () {return 'nao function com hoisting';}
```

---
## Closure
Closure são funções que tem acesso as variaveis de uma função exterior.<br>
Ou seja ele tem acesso as variaveis da função em que ela pertence.<br >
Exemplo:
```
function fnExterior (paramUm, paramDois) {
var escopoPai = "Variavel exterior ";
  //esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
    function fnInterior () {
        return escopoPai + paramUm + " " + paramDois;
    }
    return fnInterior();
}
fnExterior("paramUm", "paramDois");
```


---

## Variavel Global

Variavel Global pode ser usada em qualquer funçao, pois ela esta visivel para todos escopos, inclusive pode ser alterada nesses escopos internos.
Elas podem ser definidas sem a palavra reservada **var** ou definidas no maior escopo possivel no caso do browser no objeto window.
<br>
Exemplo:
```
var globalVar = 2;
function () {
  return globalVar * globalVar;
}
```
---
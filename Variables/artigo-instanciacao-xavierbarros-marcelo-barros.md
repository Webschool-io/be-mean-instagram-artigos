## Artigo
**Autor**: Marcelo Barros
**Prazo**: sem limite

## Hoisting
Hoisting em inglês significa levantar ou suspender algo através de um aparato mecânico. Em português, significa usar um guindaste para elevar um objeto. É isto que acontece em Javascript quando declaramos uma variável ou função. Sua declaração é “elevada” para o topo do escopo e, apesar de não ser óbvio, o modo como o histing funciona é fácil de ser entendido.

**VARIÁVEL**

Toda vez que uma variável é definida, sua **declaração** é **hoisted**, mas não sua inicialização. O que quer dizer que a declaração da variável vai para cima do escopo antes mesmo do código ser executado, mas esta variável não recebe nenhum valor e permanece como `undefined`.

'' try {
'' console.log(a)
'' } catch(e) {
'' console.error('A variável 'a' não foi definida.')
'' }
''
'' try {
'' console.log(a)
'' var a = 2
'' } catch(e) {
'' console.error('A variável 'a' não foi definida.')
'' }

No exemplo acima, quando imprimimos o valor de `a`, recebemos um erro pois a variável não existe. No seguinte, `undefined` será impresso, mesmo tendo a declaração de `a` no console.log. Vemos então que ocorreu um `hoisting` da **declaração** da variável `a`, mas não da sua **inicialização**.

'' try {
'' var a
'' console.log(a)
'' a = 2
'' } catch(e) {
'' console.error('a não existe no contexto atual')
'' }

No código acima rescrevemos o exemplo anterior de forma que faça mais sentido para nós, já que este código será executado de forma linear.

Apenas a **declaração** da variável vai para o topo, mas não sua inicialização. Esta continua no mesmo lugar em que definimos no código. Por isso recebemos `undefined` quando acessamos seu valor.

**FUNCTION**
O hoisting com funções acontece de maneira diferente pois não só o nome da função é hoisted como também seu corpo.

'' foo()
'' function foo() {
'' console.log('bar')
'' }

O código acima irá imprimir `bar`, sem nenhum erro. Mesmo executando uma função antes de ser definida. Isso pois tanto o nome da função quanto seu corpo são _hoisted_.

'' function foo() {
'' function bar() {
'' return 3
'' }
''
'' return bar()
''
'' function bar() {
'' return 8
'' }
'' }
''
'' console.log(foo())

O output da função acima será **8** pois dentro do escopo da `foo`, primeiro temos a definição da função `bar` que já está no topo. Depois do `return` temos outra definição de função com o nome de `bar`. Apesar de parecer ao fim do escopo, ela é _hosted_ pois vai para o topo antes da execução. Como ela é a segunda função com o mesmo nome, ela acaba por **sobrescrever a primeira**, e o nome `bar` acaba por fazer referência à segunda função.

Uma função pode ser declarada como uma expressão, e quando desta forma, ela obedece a regra de _hoisting_ de variável. Apenas seu nome será _hosted_.

'' foo()
'' var foo = function() {}

Aqui será disparado um erro do tipo `TypeError`, avisando que `undefined` não pode ser usado como função.

## Closure
Closures permitem aos programadores utilizarem variáveis de uma função exterior, chamadas cadeia de escopo. O closure possui três cadeias de escopo:

1. Possuem acesso ao seu próprio escopo (variáveis definidas entre suas chaves);
2. Possuem acesso às variáveis da função exterior;
3. Possuem acesso às variáveis globais.

A função interior tem acesso não somente as variáveis da função exterior, mas também aos parâmetros dela. A função interior não pode chamar o objeto `arguments` da função exterior, mas pode chamar parâmetros da função externa diretamente.

É possível criar uma closure adicionando uma função dentro de outra função.

'' function showName (firstName, lastName) {
'' var nameIntro = "Your name is ";
''
'' function makeFullName() {
'' return nameIntro + firstName + " " + lastName;
'' }
'' return makeFullName();
'' }
''
'' showName("Marcelo", "Barros");

**REGRA DOS CLOSURES**

1. **Closures tem acesso a variável das funções exteriores mesmo após o retorno da função exterior**
	Uma característica importante dos closures é que a função interior continua tendo acesso as variáveis da função exterior mesmo após ela ter retornado. Quando funções Javascript executam, elas usam a mesma cadeia de escopo que estava em vigor quando foram criadas. Significa que mesmo depois da função exterior retornar, a função interior continua tendo acesso as variáveis da função exterior. Portanto, você pode chamar a função interior depois em seu programa.

'' function celebrityName(firstName) {
	'' var nameIntro = "This celebrity is ";
	''
	'' function lastName(theLastName) {
	'' return nameIntro + firstName + " " + theLastName;
	'' }
	'' return lastName;
	'' }
	''
	'' var mbName = celebrityName("Marcelo");
	'' mbName("Barros");

2. **Closures armazenam referências para as variáveis da função exterior**
	Não armazenam o valor real. Closures ficam mais interessantes quando o valor da variável da função exterior muda antes que o closure seja chamado, e essa característica pode ser aproveitada de formas criativas

'' function celebrityID() {
	'' var celebrityID = 999;
	''
	'' return {
	'' getID: function() {
	'' return celebrityID;
	'' },
	''
	'' setID: function(theNewID) {
	'' celebrityID = theNewID;
	'' }
	'' }
	'' }
	''
	'' var mbID = celebrityID();
	'' mbID.getID();
	'' mbID.setID(567);
	'' mbID.getID();

3. **Closures que deram errado**
	Por conta dos closures terem acesso ao valor atualizado das variáveis das funções exteriores, eles podem também conduzir a erros quando a variável da função exterior muda com um loop for.

'' function celebrityIDCreator(theCelebrities) {
	'' var i;
	'' var uniqueID = 100;
	'' for(i =0; i &lt; theCelebrities.length; i += 1) {
	'' theCelebrities[i]["id"] = function() {
	'' return uniqueID + i;
	'' }
	'' }
	'' return theCelebrities;
	'' }
	''
	'' var actionCelebs = [{ name: "Stallone", id: 0 }, { name: "Willis", id: 0 }, { name: "Cruise", id: 0 }];
	'' var createIDForActionCelebs = celebrityIDCreator(actionCelebs);
	'' var stalloneID = createIDForActionCelebs[0];
	''
	'' console.log(stalloneID.id());

Neste exemplo, quando a função anônima é chamada, o valor de i é  3. Este número foi adicionado ao uniqueID para criar 103 para **todos** os celebrityID. Então cada posição no arras retornado obteve o ID = 103, ao invés do valor pretendido 100, 101, 102.

Isso acontece pois o closure teve acesso à variável da função exemplo por referência. não por valor. Então acessamos a variável _i_ quando ela for alterada, depois que a função exterior rodou o `loop for` inteiro e retornou o último valor, que foi 103.

## Variável global
Variável global é aquela que é criada fora das funções, mas que podem ser usadas dentro ou fora de funções, em qualquer parte do documento podendo desse modo as variáveis serem utilizadas em mais de uma função ou em vários pontos do código.

'' var aCentaur = "a horse with rider,";
''
'' function antiquities() {
'' var aCentaur = "A centaur is probably a mounted Scythian warrior";
'' }
''
'' antiquities();
'' aCentaur += " as seen from a distance by a naive innocent.";
''
'' document.write(aCentaur);
''
'' // "a horse with ride, as seen from a distance by a naive innocent."

## Variável por parâmetro
Funções recebem parâmetros, e dentro do contexto da função elas receberão novos nomes, mesmo que sejam variáveis marcadas para a função vindas de outros contextos. Além disso, essas variáveis podem ser acessadas apenas dentro da função.

''function test(x) {
''console.log(x);
''}
''test(77)
''console.log(x) // ReferenceError: x is not defined

## Instalação usando uma IIFE
Ben Alman publicou um artigo chamado Immediately-Invoked Function Expression (IIFE), onde o código abaixo representa uma IIFE, uma função definida e auto-executável

'' (function()) {
'' console.log('Olá');
'' } ()) // Os parenteses definem a auto-execução

A utilização mais comum é quando criamos um escopo no Javascript para que as variáveis definidas dentro da função não poluam o escopo global.

'' var fn = function() { return 'oi' } (); // funciona
'' var fn = (function() { return 'oi' }()) // funciona e é validado pelo JSLint
'' var fn = (function() { return 'oi' }) () // também funciona, mas não é validado pelo JSLint

Representação de como passar uma variável por parâmetro para a IIFE.

'' var x = 'Olá'; // x é uma variável global
''
'' (function (a) {
'' console.log(a)
'' }(x)) // atribuimos o x a variavel a, assim é possivel acessar o valor de x dentro da IIFE
''
'' // É possível passar quaLquer parametro como se fosse qualquer outra função
''
'' (function(string){
'' console.log(string)
'' }('oi'))

Para retornarmos um valor de uma IIFE, podemos utilizar o `return`.

'' var a = 1;
''
'' var b = (function(c) {
'' return c;
'' }(a))
'' b // retorna valor 1 diretamente para a variável b
''
'' var d = (function(c) {
'' return function(d) {
'' return c + d;
'' };
'' }(a))
''
'' // retorna 2 pois a variável a foi passada por
'' // parâmetro e depois foi atribuído um valor na variável c;
''
'' console.log(d(1));

## Considerações

Nota-se que mesmo não declarando as variáveis ou função no topo antes de sua execução, elas são movidas, não retornando erros porém, é sempre importante manter as declarações no topo do escopo.

# Instanciação de variáveis no Javascript
Autor: **José Alves de Sousa Neto**

Neste artigo vamos abordar alguns temas importantes sobre a instanciação de variáveis em JavaScript, entre eles: hoisting, closure, variáveis locais e globais, e IIFE.

## Hoisting
Variáveis e funções declaradas em javascript são movidas para o topo do código de forma invisivel para o contêiner do interpretador javascript
Exemplo:

o codigo 

    function foo(){ 
        bar();
        var x=1;
    }
é interpletado das seguinte forma

    function foo(){ 
        var x;
        bar();
        x=1;
    }


## Closure
Um closure é uma função interior que tem acesso a variáveis de uma função exterior – cadeia de escopo. O closure tem três cadeias de escopo: ele tem acesso ao seu próprio escopo (variáveis definidas entre suas chaves), ele tem acesso as variáveis da função exterior e tem acesso as variáveis globais.

A função interior tem acesso não somente as variáveis da função exterior, mas também aos parâmetros dela. Note que a função interior não pode chamar o objeto arguments da função exterior, entretanto, pode chamar parâmetros da função externa diretamente.

Você cria um closure adicionando uma função dentro de outra função.

    function showName (firstName, lastName) {
        var nameIntro = "Your name is ";     
        //esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
        function makeFullName () {
            return nameIntro + firstName + " " + lastName;
        }     
        return makeFullName ();
    } 
    showName ("Jose", "Alves");
 

Regra dos Closures e Efeitos Colaterais

1. Closures tem acesso a variável das funções exteriores mesmo após o retorno da função exterior

Uma da mais importante e delicada característica dos closures é que a função interior continua tendo acesso as variáveis da função exterior mesmo após ela ter retornado. Sim, você leu corretamente. Quando funções no JavaScript executam, elas usam a mesma cadeia de escopo que estava em vigor quando foram criadas. Isso significa que mesmo depois da função exterior retornar, a função interior continua tendo acesso as variáveis da função exterior. Portanto, você pode chamar a função interior depois em seu programa

2. Closures armazenam referências para as variáveis da função exterior

Eles não armazenam o valor real. Closures ficam mais interessantes quando o valor da variável da função exterior muda antes que o closure seja chamado.

3. Closures que deram errado

Por causa dos closures terem acesso ao valor atualizado das variáveis das funções exteriores, eles podem também conduzir a erros quando a variável da função exterior muda com um loop for




## Variável global
Todas as variáveis declaradas fora de uma função estão no escopo global. No navegador, que é onde estamos interessados como desenvolvedores front-end, o contexto global ou escopo é o objeto window (ou o documento HTML inteiro).

Qualquer variável declarada ou inicializada fora de uma função é uma variável global, e estará portanto disponível para toda a aplicação

odas as variáveis globais são anexadas no objeto window. Então, todas as variáveis globais que nós declaramos podem ser acessadas pelo object window 

## Variável por parâmetro
Na passagem de parâmetros e passada uma copia dos parâmetros passados, logo as alterações dentro da função não são replicadas fora do scopo da função, com exceção dos parâmetros de objetos por referencia
 
## IIFE
Em 2010, um cara chamado Ben Alman, publicou um artigo chamado “Immediately-Invoked Function Expression (IIFE)”, no artigo ele conta do porque não concordava com o termo “self-executing anonymous function”, que significava isso aqui:
    
    (function () {
     console.log('Olá')
    }())

Esta função é executada no mesmo momento em que é definida. isso é chamado de “self-executing anonymous function” que significa “função anônima auto-executável”, é a definição de uma função anônima que se auto-executa.

Porém Ben Alman, sugeriu que na verdade era uma “Definição de Função Imediatamente Executável” que seria a tradução de IIFE. e o resto é história e muita gente gostou e adotou o termo IIFE para se referir a essas funções que são imediatamente executadas ao serem definidas.

Há vários locais que podemos usar uma IIFE, porém o mais comum é quando queremos criar um escopo no JavaScript para que as variáveis definidas dentro da função não poluam o escopo global.

Isso é tudo a respeito de evitar ao máximo criar variáveis globais no JavaScript, e criar escopos quando e onde precisarmos.

Como escrevo uma IIFE no JavaScript?

IIFE geralmente ficam dentro de parênteses ( ), o que eles fazem basicamente é retornar qualquer coisa que você coloque dentro dele, como se fossem parâmetros de uma função.

    (i) // undefined // Porque i não existe
    (1 + 2) // retorna 3     
    var fn = (function () { return 'oi' }) // retorna toda a função para a variável fn
  
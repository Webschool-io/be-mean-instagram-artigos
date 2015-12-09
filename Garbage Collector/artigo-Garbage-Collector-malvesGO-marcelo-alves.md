# Artigo - Garbage Collector
**Autor**: Marcelo Rodrigues Alves - [MalvesGO](https://github.com/MalvesGO)
**Data**: Qua Dez  9 17:11:38 BRST 2015

Antes de iniciarmos a falar sobre Garbage Collector, precisamos saber a sua definição: "Coletor de Lixo", também bastante conhecido como "memória gerenciada". A maioria dos desenvolvedores não tem conhecimento nesse tema e ou deixam o prorpio Javascript realizar o controle do gerenciamento de memória executando a aplicação. Poucas pessoas tem interesse ou curioside de estudar o assunto e como realmente funciona.

Isso mesmo!!! O Javascript libera espaço quando existem objetos que não estão sendo utilizados.

Vamos tentar exemplificar usando exemplos do nosso cotidiano...

Suponhamos que você recebeu seu salário hoje e o dinheiro se encontra no banco, estando no início do mês temos várias contas para pagar. O banco irá recolher o dinheiro que você deve e provavelmente assim como eu, você irá passar o resto do mês sem dinheiro. Se você percebeu sua conta ficou vazia novamente... hahahahaha

Brincadeiras a parte, no exemplo citado podemos observar que "liberamos espaço no banco" e é justamente isso que o Garbage Collector faz.

# Exemplo de Garbage Collector em Javascript


var beMean = new String("Javascript é toooop...");
document.write( beMean );

Vejamos o que estamos fazendo... Primeiro declarei uma variável contendo um texto e exibi no navegador. Daí surge a questão, não irei precisar utilizar mais esta variável ainda assim ela continuará lá ocupando memória. 

Podemos resolver este problema utilizando o Garbage Collector da seguinte forma:

beMean = null;

O seu navegador se encarrega de fazer a limpeza para você, isso ocorre automaticamente liberando assim a memória.

Então por que raios tenho que saber usar esse tal Garbage Collector? 
Simples o Javascript não adivinha quando você não esta utilizando um objeto, a não ser que primeiramente você a declare como null. Desta forma, o Javascript entende que não precisa disponibilizar memória até que a varíavel seja utilizada novamente. Garantindo assim performance para sua aplicação.

# Coleta de lixo automática

Quando utilizamos funções em Javascript qualquer variável criada dentro da função já fica disponivel para a coleta do lixo. Podemos observar neste exemplo.

function exibeTexto(){
    var beMean = new String("Javascript é toooop...");
    document.write( beMean );
}
exibeTexto();

Este exemplo resume o que fizemos no exemplo anterior, porém utilizando uma função. Porém nao podemos declarar como null agora. Por que não??? Simples, devido as funções são criadas e executadas apenas uma vez. Depois disso ela não precisa mais existir. Dessa forma todas as variáveis e objetos são definidos com null automaticamente para você e ficam disponíveis para a coleta do lixo.

# Por quê devemos usar os coletores de lixo.

É muito importante que você desenvolva suas aplicações usuando Garbage Collector, para deixar sua aplicação sem utilização desnecessária de memória.








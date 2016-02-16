---
layout: post
title:  "Artigo - Instanciação no Javascript"
date:   2015-11-18 10:00:00
categories: javascript
tags: [javascript,bemean,artigo]
---

## Hoisting
 As declarações de variáveis são processadas antes de qualquer código ser executado. Isso significa que a declaração da variável é movida para o inicio da função ou do cógido global, isso é conhecido como hoisting. É sempre recomendado declara as variáveis e função no topo antes da inicialização.

{% highlight javascript %}
// Exemplo 1
bla = 2; // Atribui um valor a variável bla
var bla; // Declaração da variável bla
// Seria o mesmo que fazer
var bla;
bla = 2;
{% endhighlight %}

Se executarmos o código abaixo no console do navegador irá retornar ò erro `ReferenceError` pois a variável ainda não foi declarada.
{% highlight javascript %}
// Exemplo 2
console.log(bar);
ReferenceError: bar is not defined
{% endhighlight %}

No entanto o código abaixo quando executado irá retornar `undefined`, pois a declaração (sem a atribuição do valor) é movido para o topo.
{% highlight javascript %}
// Exemplo 3
console.log(bla); // Retorna undefined pois não foi atribuido nenhum valor
                  // mas sua declaração foi movida para o topo.
var bla = 2; // Declaração da variável e atribuição de um valor

// equivalente a fazer
var bla = undefined; // move a declaração para o topo
console.log(bla); // retorna undefined
bla = 2; // somente atribui o valor a variável
{% endhighlight %}

No caso de funções é um pouco diferente, neste caso abaixo a função e seu conteúdo são movidos para o topo:

{% highlight javascript %}
// Exemplo 4
bar(); //Chama a função bar
function bar() { // Declaração da função bar
    console.log('bar');
}
{% endhighlight %}

No exemplo 5 o valor retornado é 2 conforme explicação no exemplo 6.
{% highlight javascript %}
// Exemplo 5
function getValor() {
    function retornarValor() { // Declaração
     return 1;
    }

    return retornarValor();
    function retornaValor() { // Declaração - é movido para o topo
        return 2;
    }
}
var resultado = getValor();
console.log(resultado); // 2
{% endhighlight %}

Representação do que acontece com o código do Exemplo 5.
{% highlight javascript %}
// Exemplo 6
function getValor() {
    function retornarValor() { // Declaração
     return 1;
    }
    function retornaValor() { // Movido para o topo e sobrescreve função anterior
        return 2;
    }
    return retornarValor();
}
var resultado = getValor();
console.log(resultado); // 2
{% endhighlight %}

No Exemplo 7 o resultado é outro devido a função ser atribuida a uma variável e o "hoisting" ocorre somente com a declaração não atribuição/inicialização de valor.
{% highlight javascript %}
// Exemplo 7
function getValor() {
    var retornarValor = function() { // Declaração e atribuição
        return 1;
    }
    return retornarValor();
    var retornarValor = function() { // Declaração e atribuição- este ponto é diferente.
        return 2;
    }
}
var resultado = getValor();
console.log(resultado);
{% endhighlight %}

Representação do que acontece com o código do Exemplo 7.
{% highlight javascript %}
// Exemplo 8
function getValor() {
    var retornarValor = undefined; // Declaração
    var retornarValor = undefined; / Declaração
    retornarValor = function() { // atribuição
        return 1;
    }
    return retornarValor();
    // Como o trecho abaixo é um atribuição de valor a uma variável, então não é movido para o topo e ao mesmo tempo não será executado devido o `return` estar antes.
    retornarValor = function() { // atribuição
        return 2;
    }
}
var resultado = getValor();
console.log(resultado); // 1
{% endhighlight %}

## Closure

Um closure é uma função interior que tem acesso a variáveis de uma função exterior - cadeias de escopo. O closure tem três cadeias de escopo: ele tem acesso ao seu próprio escopo (variáveis definidas entre suas chaves), tem acesso as variáveis da função exterior e tem acesso as variáveis globais. São usados frequentemente no JavaScript.

{% highlight javascript %}
function showName (firstName, lastName) {
    var nameIntro = "Your name is ";

    //esta função interior tem acesso as variáveis da função exterior, incluindo os parâmetros
    function makeFullName () {
        return nameIntro + firstName + " " + lastName;
    }

    return makeFullName ();
}
showName ("Michael", "Jackson"); //Your name is Michael Jackson
{% endhighlight %}

Aqui temos outro exemplo interessante, pois a função `makeAdder` irá construir outras funções que podem adicionar um determinado valor especifico a seu argumento.
{% highlight javascript %}
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

print(add5(2));  // 7
print(add10(2)); // 12
{% endhighlight %}

Utilização prática do closure,
{% highlight html %}
<html>
    <head>
        <style type="text/css">
            body {
              font-family: Helvetica, Arial, sans-serif;
              font-size: 12px;
            }

            h1 {
              font-size: 1.5em;
            }
            h2 {
              font-size: 1.2em;
            }
        </style>
    </head>
    <body>
        <a href="#" id="size-12">12</a>
        <a href="#" id="size-14">14</a>
        <a href="#" id="size-16">16</a>
        <script type="text/javascript">
            function makeSizer(size) {
                return function() {
                    document.body.style.fontSize = size + 'px';
                }
            }
            var size12 = makeSizer(12);
            var size14 = makeSizer(14);
            var size16 = makeSizer(16);

            document.getElementById('size-12').onclick = size12;
            document.getElementById('size-14').onclick = size14;
            document.getElementById('size-16').onclick = size16;
        </script>
    </body>
</html>
{% endhighlight %}

Exemplo utilizando o jQuery.
{% highlight javascript %}
$(function () {
    var selections = [];
    $(".niners").click(function () { //este closure tem acesso as variáveis de selections
        selections.push(this.prop ("name")); //atualiza a variável selection no escopo da função exterior
    });
});
{% endhighlight %}

## Variável Global

A linguagem JavaScript possui dois escopos: global e local. Uma variável declarada fora de uma definição de função é uma variável global, e seu valor será acessível e modificável em todo o seu programa. Todas as variáveis declaradas fora de uma função estão no escopo global. No navegador o contexto global ou escopo é o objeto window (ou o documento HTML inteiro), conforme exemplo abaixo:

{% highlight javascript %}
var b = 1;
a = 2;
// Variável global dentro de uma função
function showAge() {
    age = 90; // age é uma variável global pois não foi declarada como a palavra chave 'var'
    console.log(age);
}
console.log(age); // a variável age pode ser acessáda fora da função pois foi declarada como global.
{% endhighlight %}

## Variável por parâmetro

Toda função possui dois parâmetros implícitos `this`, representando o contexto da função, e `arguments`, representando os argumentos passados para a função.Quando passamos um parametro para uma função, são atribuídos a eles variáveis de mesmo nome dentro da função, o mesmo ocorre quando passamos variáveis globais por parâmetro. Ao invés de serem inicializados com `undefined`, são inicializados diretamente pelo `arguments`.

{% highlight javascript %}
var x = 1; // variável global
function init() {
    var y = 2; // variável local
    // Passando variáveis por parâmetro
    function soma(a, b) {
        return a + b;
    }
    console.log(soma(x, y)); // 3
    // Equivalente a fazer
    function soma() {
        return arguments[0] + arguments[1];
    }
    console.log(soma(x, y)) // 3
}
init();
{% endhighlight %}

## Instanciação usando uma IIFE

Ben Alman publicou um artigo chamado Immediately-Invoked Function Expression (IIFE). Onde o código abaixo representa um IIFE, uma função definida e auto-executável.

{% highlight javascript %}
(function() {
   console.log('Olá');
}()) // Os parênteses "()" define a auto-execução
{% endhighlight %}

A utilização mais comum é quando queremos cria um escopo no JavaScript para que as variáveis definidas dentro da função não poluam o escopo global (definidas no window).

{% highlight javascript %}
var fn = function () { return 'oi' }(); // Funciona
var fn = (function () { return 'oi' }()) // Funciona e é validado pelo JSLint
var fn = (function () { return 'oi' })() // Também funciona, mas não é validado pelo JSLint
{% endhighlight %}

Representação de como passar uma variável por parâmetro para a IIFE.
{% highlight javascript %}
var x = 'Olá'; // x é uma variável global
(function (a) {
console.log(a)
}(x)) // atribuimos o x a variável a, assim é possível acessar o valor de x dentro da IIFE.

// É possível passar qualquer parâmetro como se fosse qualquer outra função.
(function (string) {
    console.log(string)
}('oi'))
{% endhighlight %}

Para retornarmos um valor de uma IIFE podemos utilizar o `return`.

{% highlight javascript %}
var a = 1;
var b = (function (c) {
    return c;
}(a))
b // retorna valor 1 diretamente para a variável b

var d = (function (c) {
    return function(d) {
       return c + d;
    };
}(a))
// retorna 2 pois a variável a foi passada por
// parâmetro e depois foi atribuido um valor na variável c;
console.log(d(1));
{% endhighlight %}

## Considerações

Pode-se notar que mesmo não declarando as varíáveis ou função no topo antes da execução as mesmas são movidas assim não retornando erros, no entanto é sempre importante manter as declarações no topo do escopo. E também que o closure permite organizar, otimizar e reaproveitar funções e objetos criados. As diferenças de variáveis locais e globais e como passa-las por parâmetros de uma função. Como também a utilização do IIFE e seu funcionamento.

## Referências

[Hugo, Entendendo escopo e hoisting no JavaScript](https://www.hugobessa.com.br/posts/entendendo-escopo-e-hoisting-no-javascript/)
[Closures, Mozilla](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Closures)
[Aulas de Programação, Dicas Javascript - Ep01: Hoisting](https://www.youtube.com/watch?v=JGpekHQ_9kY)
[Javascript Brasil, Entenda Closures no Javascript com facilidade](http://javascriptbrasil.com/2013/10/12/entenda-closures-no-javascript-com-facilidade/)
[Statements Var, Mozilla](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/var)
[Loop Infinito, Hoisting e escopo em javascript](http://loopinfinito.com.br/2014/10/29/hoisting-e-escopo-em-javascript/)
[Microsoft, Escopo variável (JavaScript)](https://msdn.microsoft.com/pt-br/library/bzt2dkta(v=vs.94).aspx)
[Bruno Souza, Functions no javascript](https://brunosouza.org/functions-no-javascript.html)
[Tuts Mais, O que e iife no javascript](http://tutsmais.com.br/blog/javascript-2/o-que-e-iife-no-javascript/)

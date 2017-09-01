---
layout: post
title:  "Configurando o dump() do twig no Slim Framework 2.6.1"
description: "Como configurar o dump do twig no Slim Framework v2.6.1"
date:   2016-03-31 10:51:00
categories: php
tags: [php,slim,twig,dump,debug]
image: /images/twig/dump.png
---

Procurei na internet como configurar a função `dump()` na view do slim utilizando o twig e a [documentação](http://twig.sensiolabs.org/doc/functions/dump.html)
informa para adicionar a extensão debug e setar true para o ambiente de desenvolvimento.

No entanto minha estrutura é essa:
{% highlight json %}
# Arquivo composer.json
{
    "require":{
        "php":">=5.3.0",
        "slim/slim":"2.*",
        "slim/views":"0.1.*",
        "twig/twig":"1.*"
    },
}
{% endhighlight %}

{% highlight php %}
<?php
// index.php
...

$twigView = new \Slim\Views\Twig();
$app = new \Slim\Slim(array(
    'debug' => true,
    'view' => $twigView,
    'templates.path' => '../views/'
));

...

{% endhighlight %}

Como fazer essa bagaça funcionar ? Se você olhar a documentação do [Slim-View](https://github.com/slimphp/Slim-Views)
verá que é possível passar as configurações para a propriedade `parserOptions` e carregar as extensões na propriedade `parserExtensions`
desta forma.


{% highlight php %}
<?php

...

$twigView = new \Slim\Views\Twig();
$twigView->parserOptions = array(
    'debug' => true
);
$twigView->parserExtensions = array(
    new \Twig_Extension_Debug(),
);
$app = new \Slim\Slim(array(
    'debug' => true,
    'view' => $twigView,
    'templates.path' => '../views/'
));

...

{% endhighlight %}

Fazendo isso irá habilitar a extensão dump nas views do twig ficando muito mais fácil debugar.
Pronto! Agora é possível utilizar o dump na view do twig com no exemplo abaixo:

{% highlight php %}
<?php

$app->get('/parametros', function () use ($app) {

    $parametros = array(
        array(
            'id' => '1',
            'nome' => 'Código dos Tipos...',
            'parametro' => 'cod',
            'valor' => '6165',
            'descricao' => 'Informe os códigos dos tipos...'
        ),
        array(
            'id' => '2',
            'nome' => 'Data Inicial',
            'parametro' => 'data_inicial',
            'valor' => '01/04/2015',
            'descricao' => 'Informe a data inicial...'
        ),
        array(
            'id' => '4',
            'nome' => 'Dias para exportação...',
            'parametro' => 'dias_exportacao',
            'valor' => '365',
            'descricao' => 'Informe o número de dias para exportação...'
        )
    );

    $app->render(
        'parametros/index.twig',
        array(
            'parametros' => $parametros
        )
    );
});

{% endhighlight %}


{% highlight html %}

...

    <div id="page-wrapper">
        <div class="row">
            <div class="col-lg-12">
                { {dump(parametros) } }
            </div>
        </div>
    </div>

...

{% endhighlight %}

![dump](/images/twig/dump.png "Exemplo de retorno do dump")

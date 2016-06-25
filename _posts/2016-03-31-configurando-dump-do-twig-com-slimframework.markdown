---
layout: post
title:  "Configurando o dump() do twig no slim framework 2.6.1"
description: "Como configurar o dump do twig no slim framework"
date:   2016-03-31 10:51:00
categories: php
tags: [php,slim,twig,dump,debug]
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

Pronto! Agora é possível utilizar o dump na view do twig.

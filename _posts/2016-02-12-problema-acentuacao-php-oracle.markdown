---
layout: post
title:  "Resolver problemas com acentuação e caracteres especiais no php com oracle"
date:   2016-02-12 11:20:00
categories: php
tags: [php,oracle,oci,utf8]
---
Se ao retornar os resultados de uma consulta no banco de dados Oracle e os caractéres especiais forem subtituidos por um "?". Adicione o charset na conexão com o banco.

{% highlight php %}
<?php
// Retornando o texto - ABCDEFGHU JLMNOP DIN 206B  ? 5/8 "

$character_set = 'UTF8';
$conn = oci_connect($username, $password, $connection_string, $character_set);

// Retorna o texto corretamente - ABCDEFGHU JLMNOP DIN 206B  Ø 5/8 "

{% endhighlight %}

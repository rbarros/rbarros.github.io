---
layout: post
title:  "MongoDB - Aula 01 - Exercício"
date:   2015-11-16 22:00:00
categories: mongodb
tags: [bemean,mongodb,mongoimport]
---

Importando os restaurantes

{% highlight bash %}
rbarros:~/workspace (master) $ mongoimport --db be-mean --collection restaurantes --drop --file restaurantes.json
connected to: 127.0.0.1
2015-11-10T03:34:32.589+0000 dropping: be-mean.restaurantes
2015-11-10T03:34:33.484+0000 check 9 25359
2015-11-10T03:34:33.485+0000 imported 25359 objects
Contando os restaurantes

> db.restaurantes.find({}).count()
25359
{% endhighlight %}

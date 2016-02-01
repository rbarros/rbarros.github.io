---
layout: post
title:  "Formatar preço com JavaScript de forma simples"
date:   2016-01-31 22:12:00
categories: javascript
tags: [javascript,price,value]
---
Uma forma simples de formatar preços em javascript.

{% highlight js %}
var prive = 170090;
var money = 'R$' + parseInt(price.toString().replace(/[\D]+/g,''))
          .toString()
          .replace(/([0-9]{2})$/g, ',$1')
          .replace(/([0-9]{3}),([0-9]{2}$)/g, '.$1,$2')
console.log(money); // 'R$ 1.700,90'
{% endhighlight %}
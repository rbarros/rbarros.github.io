---
layout: post
title:  "Formatar preço com JavaScript de forma simples"
date:   2016-01-31 22:12:00
categories: javascript
tags: [javascript,price,value]
---
Uma forma simples de formatar preços em javascript.

{% highlight js %}
var price = 1700.90;
var money = 'R$ ' + parseFloat(price, 10).toFixed(2).replace(/./g, function(c, i, a) {
    return i && c !== "." && ((a.length - i) % 3 === 0) ? '.' + c : (c === '.' ? ',' : c);
});
console.log(money); // "R$ 1.700,90"
{% endhighlight %}

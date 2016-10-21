---
layout: post
title:  "Correção no looping do grunt watch"
date:   2016-10-21 16:48
categories: grunt
tags: [grunt,watch,sysctl]
---

O watch do grunt utiliza o Inotify so sistema para monitorar mudanças nos arquivos e ocorre que em
alguns sistemas o número máximo de identificadores permitidos é baixo e acaba retornando erros ou
fazendo com que o watch fique em looping:

{% highlight bash %}
Running "watch" task
Waiting...
Warning: watch /path/foo/bar ENOSPC

Running "watch" task
Waiting...
Warning: watch /path/foo/bar ENOSPC

Running "watch" task
Waiting...
Warning: watch /path/foo/bar ENOSPC

Running "watch" task
Waiting...
Warning: watch /path/foo/bar ENOSPC
{% endhighlight %}

É necessário adicionar no arquivo /etc/sysctl.conf o parâmetro:

{% highlight bash %}
fs.inotify.max_user_watches=524288
{% endhighlight %}

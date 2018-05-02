---
layout: post
title:  "Iniciando no PHP"
description: "Para os que estão começando a desenvolver com a linguagem PHP, vou compartilhar algumas dicas que irão ajudar."
date:   2017-02-28 22:25
categories: php
tags: [php]
---

Vou postar algumas dicas para você que esta iniciando no PHP e não sabe exatamente por onde começar.

Primeiramente você deve ler o post [PHP do Jeito Certo](http://br.phptherightway.com/){:target="_blank"} criado por [Josh Lockhart](https://twitter.com/codeguy){:target="_blank"} que contém uma referência rápida e fácil de ler, introduzindo você às melhores práticas e ver o [Curso de PHP](http://www.cursoemvideo.com/lesson/curso-php-historia-php/){:target="_blank"} para entender a história e funcionamento da linguagem.

"Bom eu li PHP do Jeito Certo e vi os videos e cursos, já vou criar meu website... mas por onde começar ?"

Ótimo que tenha visto tudo, no entanto, como será sua estrutura de diretórios ? Irá utilizar um framework ? Como vou carregar os arquivos ?

Já trabalhei com o CodeIgniter 2 e 3, Laravel 3, 4 e 5, CakePHP e com a experiência e busca na comunidade de desenvolvimento percebi que quando não queremos utilizar um framework que já possui uma estrutura pronta nos deparamos com a dificuldade de organizar os arquivos. Então vou mostrar abaixo a estrutura que costumo utilizar para iniciar um projeto, você pode criar a estrutura que achar conveniente para o seu projeto.

{% highlight bash %}
<projeto>/
 |__ dump/ - Sql do banco
 |__ modules/ - Diretório de modulos
 |__ public/ - Assets do sistema (public_html ou htdocs nas hospedagens)
 |  |__ fonts/ - Fonts
 |  |__ (img|image)/ - Imagens do sistema
 |  |__ libs/ - Bibliotecas e Plugins utilizados pelo sistema
 |  |__ (scripts|js)/ - Scripts utilizados pelo sistema
 |  |__ (styles|css)/ - Estilos do template e sistema
 |  |__ .htaccess
 |  |__ index.php - Arquivo principal da aplicação
 |
 |__ src/ - Diretório source contendo o Core do sistema
 |__ storage - Diretório de cache, log e sessão da aplicação
 |  |__ cache - Cache
 |  |__ logs - Logs do sistema
 |  |__ session - Sessão utilizada pela aplicação
 |
 |__ vendor/ - Dependencias (composer) do sistema
 |__ README.md - Informações do projeto
 |__ composer.json - Arquivo de configuração do composer
{% endhighlight %}

Nesta estrutura você pode notar que existe um diretório ```public/```, na qual corresponde aos diretórios ```public_html``` ou ```htdocs``` em hospedagens compartilhadas.

Posso jogar tudo no ```public_html``` ou ```htdocs``` ?

Não aconselho você fazer isso, pois quando acessamos o domínio caimos diretamente neste diretório, se houver alguma falha no servidor, dependendo do problema é possível efetuar o download dos arquivos presentes nestes diretórios, ficando seu site ainda mais vulnerável.
Em alguns casos o servidor não inicia o modulo do php no apache e acaba exibindo literalmente o conteúdo dos arquivos com extensão ```.php```. Desta forma se você deixar somente o arquivo ```index.php``` que corresponde ao arquivo principal para carregar o site, você evita que usuários consigam visualizar arquivos de configuração, senhas de banco de dados entre outros dados importantes, pois nestes diretórios irá conter somente arquivos assets.



[Manual do PHP](http://php.net/manual/pt_BR/index.php){:target="_blank"}


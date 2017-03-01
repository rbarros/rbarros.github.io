---
layout: post
title:  "Iniciando no PHP"
description: "Para os que estão começando a desenvolver com a linguagem PHP, vou compartilhar algumas dicas que irão ajudar."
date:   2017-02-28 22:25
categories: php
tags: [php]
---

Vou postar algumas dicas para você que esta iniciando no PHP e não sabe exetamente por onde começar.

Primeiramente você deve ler o post PHP do Jeito Certo criado por [Josh Lockhart]: https://twitter.com/codeguy que contém uma referência rápida e fácil de ler, introduzindo você às melhores práticas.

[PHP do Jeito Certo]: http://br.phptherightway.com/

[Curso de PHP - Importante assistir para entender a linguagem]: http://www.cursoemvideo.com/lesson/curso-php-historia-php/

"Bom eu li PHP do Jeito Certo e vi os videos e cursos, já vou criar meu website..."

Ótimo que tenha visto tudo no entanto como será sua estrutura de diretórios ? Irá utilizar um framework ? Como vou carregar os arquivos ?

Já trabalhei com o CodeIgniter 2 e 3, Laravel 3, 4 e 5, CakePHP e com a experiência e busca na comunidade de desenvolvimento percebi que quando não queremos utilizar um framework que já possui uma estrutura pronta nos deparamos com a dificuldade de organizar os arquivos. Então vou mostrar abaixo a estrutura que costumo utilizar para iniciar um projeto, você pode criar a estrutura que achar conveniente para o seu projeto, esta é uma dica e acredito que trabalhando em equipe
todos entenderam.

{% highlight bash %}
<projeto>/
 |__ dump/ - Sql do banco
 |__ modules/ - Diretório de modulos
 |__ public/ - Assets do sistema
 |  |__ fonts - Fonts
 |  |__ (img|image) - Imagens do sistema
 |  |__ libs -- Bibliotecas e Plugins utilizados pelo sistema
 |  |__ (scripts|js) - Scripts utilizados pelo sistema
 |  |__ (styles|css) - Stilos do template e sistema
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

Manual do PHP
http://php.net/manual/pt_BR/index.php


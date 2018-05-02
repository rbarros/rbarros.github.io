---
layout: post
title:  "Iniciando no PHP - Parte 2"
description: "Para os que estão começando a desenvolver com a linguagem PHP, vou compartilhar algumas dicas que irão ajudar."
date:   2018-05-01 19:02
categories: php
tags: [php]
---

Neste post vamos continuar a falar sobre a estrutura dos diretórios, porem vamos pegar a estrutura do CodeIgniter 3, caso não tenha visto de uma olhada no post [Iniciando no PHP](http://www.ramon-barros.com/php/2017/02/28/iniciando-no-php.html).

{% highlight bash %}
<projeto>/
|__ application/ - Diretório com os arquivos do projeto
|__ system/ - Core do CodeIgniter NÃO ALTERAR NADA AQUI
|__ composer.json - Arquivo com as dependências do composer
|__ contributing.md - Arquivo com informações para contribuir com o projeto (pode apagar)
|__ index.php - Arquivo principal para carregamento do site
|__ license.txt - Arquivo de licença do CodeIgniter
|__ readme.rst - Arquivo com informações do CodeIgniter (pode apagar)
{% endhighlight %}

Antes de falor sobre a estrutura, você deve ter notado que escrevi "NÃO ALTERAR NADA AQUI". É isso mesmo você não deve alterar nada no core de um framework, ou em um diretório vendor, para isso existem outros recursos que podem ser utilizados para extender e mudar a forma com que o core do sistema funciona.
Isso faz com seja mais fácil a atualização do core do sistema caso surja uma correção de segurança por exemplo.

Para esta estrutura de diretório você poderia mover o arquivo ```index.php``` para dentro do diretório ```public_html/``` ou ```htdocs``` na sua hospedagem (compartilhada ou não).

Mas não é só jogar estes arquivos para minha hospedagem e pronto ?

Não aconselho isso conforme o post anterior, para evitar os mesmos problemas, por uma falha no servidor. O CodeIgniter permite você configurar ele desta forma.

Movendo o arquivo ```index.php``` para o diretório ```public_html/``` por exemplo, você deve alterar dentro do arquivo ```index.php``` este trecho:

{% highlight php %}
<?php

...

/*
 *---------------------------------------------------------------
 * SYSTEM DIRECTORY NAME
 *---------------------------------------------------------------
 *
 * This variable must contain the name of your "system" directory.
 * Set the path if it is not in the same directory as this file.
 */
    $system_path = '../system';

/*
 *---------------------------------------------------------------
 * APPLICATION DIRECTORY NAME
 *---------------------------------------------------------------
 *
 * If you want this front controller to use a different "application"
 * directory than the default one you can set its name here. The directory
 * can also be renamed or relocated anywhere on your server. If you do,
 * use an absolute (full) server path.
 * For more info please see the user guide:
 *
 * https://codeigniter.com/user_guide/general/managing_apps.html
 *
 * NO TRAILING SLASH!
 */
    $application_folder = '../application';

...

{% endhighlight %}

Basta só colocar dois pontos e uma barra no começo do ```system``` e ```application``` e desta forma você já cosegue acessar o website.

![welcome-to-codeigniter](/images/codeigniter/welcome-to-codeigniter.png "Exemplo de acesso ao website com o diretório public_html")

No próximo post vou escrever sobre o Laravel 5.6.

[Manual do CodeIgniter](https://www.codeigniter.com/user_guide/){:target="_blank"}


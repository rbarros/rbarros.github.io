---
layout: post
title:  "Trabalhando com Memcached no PHP"
description: "Tutorial de utilização do Memcached no PHP."
date:   2017-03-1 22:25
categories: php
tags: [php]
---

Faça a instalação do Memcached, no meu caso fiz em um Ubuntu 16.04.2 LTS.

{% highlight bash %}
$ sudo apt-get install php-memcached memcached
{% endhighlight %}

Verificando se o Memcached esta instalado e habilitado:
{% highlight bash %}
$ php -i | grep memcached
/etc/php/7.0/cli/conf.d/20-memcached.ini,
memcached
memcached support => enabled
libmemcached version => 1.0.18
memcached.compression_factor => 1.3 => 1.3
memcached.compression_threshold => 2000 => 2000
memcached.compression_type => fastlz => fastlz
memcached.serializer => php => php
memcached.sess_binary => 0 => 0
memcached.sess_connect_timeout => 1000 => 1000
memcached.sess_consistent_hash => 0 => 0
memcached.sess_lock_expire => 0 => 0
memcached.sess_lock_max_wait => 0 => 0
memcached.sess_lock_wait => 150000 => 150000
memcached.sess_locking => 1 => 1
memcached.sess_number_of_replicas => 0 => 0
memcached.sess_prefix => memc.sess.key. => memc.sess.key.
memcached.sess_randomize_replica_read => 0 => 0
memcached.sess_remove_failed => 0 => 0
memcached.sess_sasl_password => no value => no value
memcached.sess_sasl_username => no value => no value
memcached.store_retry_count => 2 => 2
memcached.use_sasl => 0 => 0
Registered save handlers => files user memcached
{% endhighlight %}


Exemplo de utilização
{% highlight php %}
<?php
$memcached = new Memcached();
$memcached->addServer('localhost', 11211, true);
function cache(Memcached $memcached, $chave = null, $default = null) {
    $valor = $memcached->get($chave);
    if (!empty($valor)) {
       echo $valor.'<br/>';
    } else {
       echo 'Chave não encontrada.<br/>';
       $memcached->set($chave, $default);
    }
}
cache($memcached, 'chave', 'valor');
cache($memcached, 'chave', 'default');
{% endhighlight %}

Retorno:
Chave não encontrada.

valor

Onde poderiamos utilizar este cache ?

Nas configurações do sistema por exemplo. Alguns sistemas armazenam as configurações do sistema em uma tabela do banco
de dados.

{% highlight bash %}
# Estrutura da tabela settings
+-------------+-------------------------------------------+------+-----+---------+----------------+
| Field       | Type                                      | Null | Key | Default | Extra          |
+-------------+-------------------------------------------+------+-----+---------+----------------+
| id          | int(11)                                   | NO   | PRI | NULL    | auto_increment |
| param       | varchar(255)                              | NO   |     | NULL    |                |
| description | text                                      | YES  |     | NULL    |                |
| value       | varchar(255)                              | YES  |     | NULL    |                |
| type        | enum('input','select','radio','checkbox') | NO   |     | input   |                |
| status      | tinyint(1)                                | NO   |     | 1       |                |
+-------------+-------------------------------------------+------+-----+---------+----------------+
{% andhighlight %}

{% highlight bash %}
# Registros da tabela settings
+----+--------------+----------------------------------+--------------------+-------+--------+
| id | param        | description                      | value              | type  | status |
+----+--------------+----------------------------------+--------------------+-------+--------+
|  2 | smtp_host    | Endereço do servidor de e-mails  | smtp.teste.com.br  | input |      1 |
|  3 | smtp_porta   | Porta do servidor de e-mails     | 587                | input |      1 |
|  4 | smtp_usuario | E-mail responsável pelo envio    | teste@teste.com.br | input |      1 |
|  5 | smtp_senha   | Senha do e-mail                  | 12345              | input |      1 |
|  6 | smtp_nome    | Nome que aparece no e-mail       | Teste              | input |      1 |
|  7 | per_page     | Número de registro por página    | 20                 | input |      1 |
+----+--------------+----------------------------------+--------------------+-------+--------+
{% andhighlight %}

Para facilitar ainda mais a utilização irei criar uma classe Cache.

{% highlight php %}
<?php
namespace Lib;

use Memcached;
use Exception;

/**
 * Class para o sistema de cache
 */
class Cache
{
    /**
     * Armazena a instância da classe Memcached
     * @var Memcached
     */
    private static $instance;

    /**
     * Criando a instância da classe Memcached
     * e retornando a instância da classe Cache
     * @return Cache
     */
    public static function getInstance()
    {
        if (!isset(self::$instance)) {
            if (class_exists('Memcached')) {
                self::$instance = new Memcached();
                self::$instance->addServer('localhost', 11211, true);
            } else {
                throw new Exception("Memcached não instalado!");
            }
        }
        $class = __CLASS__;
        return new $class();
    }

    /**
     * Recupera o valor do cache referente a chave informada
     * @param  string   $key       Chave que identifica o cache
     * @param  function $callback  Função anônima para callback
     * @return mixed               Retorna o valor armazenado em cache ou do retorno do callback
     */
    public function get($key = null, $callback = null)
    {
        if (!($rows = self::$instance->get($key))) {
            if (self::$instance->getResultCode() == Memcached::RES_NOTFOUND) {
                $rows = $callback();
                self::$instance->set($key, $rows);
            }
        }
        return $rows;
    }

    /**
     * Armazena o valor no cache na chave informada
     * @param string $key   Chave que identifica o valor
     * @param mixed $value  Valor que será armazenado
     */
    public function set($key = null, $value = null)
    {
        self::$instance->set($key, $value);
        return $this;
    }

    /**
     * Exclui o cache referente a chave
     * @param  string $key Chave que identifica o cache
     * @return Cache
     */
    public function delete($key)
    {
        self::$instance->delete($key);
        return $this;
    }
}
{% endhighlight %}

Com esta classe Cache criada consigo trabalhar desta forma:
{% highlight %}
<?php
use PDO;
use Lib\Cache;

$dsn = 'mysql:host=localhost'.
       ';dbname=dbtest'.
       ';port='.
       ';charset=utf8'.
       ';connect_timeout=15';
$db = new PDO($dsn, 'root', 'root');

$param = 'smtp_host';
$cache = Cache::getInstance();

echo 'Cache: settings.'.$param;

$smtp_host = $cache->get('settings.'.$param, function() use($db, $param) {
        $query = $db->prepare('SELECT value FROM settings WHERE param = :param;');
        $query->bindValue(':param', $param);

        $value = null;
        if ($query->execute()) {
            $value = $query->fetch(PDO::FETCH_ASSOC);
        }
        return !empty($value['value']) ? $value['value'] : null;
});

var_dump($smtp_host);die;

/var/www/html/memcached.php:92:string 'smtp.teste.com.br' (length=17)

{% endhighlight %}
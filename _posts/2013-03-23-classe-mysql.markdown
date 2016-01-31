---
layout: post
title:  "Classe MySQL"
date:   2013-03-23 17:03:00
categories: mysql
tags: [mysql,codeigniter]
---
Criei esta [classe] para facilitar o meu trabalho com a manipulação das informações do bando de dados mysql como no framework CodeIgniter. Segue abaixo como utiliza-la:

{% highlight php %}
<?php

  /**
   * Carregar a classe
   */
  require "MySQL.php";

  /**
   * Instânciar a classe MySQL para o objeto $db
   */
  $db = new MySQL('localhost','usuario','senha','banco_de_dados');

  /**
   * Você pode executar as consultas diretamente desta forma
   * passando para a função $db->query();
   */
  $db->query('SELECT * FROM tabela');

  /**
   * Ou também montar a consulta desta forma
   */
   $db->select('*')
      ->from('tabela')
      ->query();
      // Retorna : "SELECT * FROM tabela"

  $db->select('*')
     ->from('tabela')
     ->where('id',100)
     ->query();
     // Retorna : "SELECT * FROM tabela WHERE id='100'"

  $db->select('*')
     ->from('tabela')
     ->where('id<',100)
     ->query();
     // Retorna : "SELECT * FROM tabela WHERE id<'100'"

  $db->select(array('id','titulo','descricao'))
     ->from('tabela')
     ->where('id=',1)
     ->query();
    // Retorna : "SELECT id,titulo,descricao FROM tabela WHERE id='1'"

  $db->select(array('id','titulo','descricao'))
     ->from('tabela')
     ->where(array('id'=>1,'data>='=>'2013-01-01'))
     ->query();
     // Retorna : "SELECT id,titulo,descricao FROM tabela WHERE id='1' AND data>='2013-01-01'"

  $db->select(array('t1.id','t2.titulo'))
     ->from('tabela1 t1')
     ->join('tabela2 t2','t2.id=t1.id');
     ->where('t1.id=',1)
     ->query();
    // Retorna : "SELECT t1.id,t2.titulo FROM tabela1 t1 JOIN tabela2 t2 (t2.id=t1.id) WHERE t1.id=1"

  $db->select(array('t1.id','t2.titulo'))
     ->from('tabela1 t1')
     ->left_join('tabela2 t2','t2.id=t1.id')
     ->where('t1.id',1)
     ->query();
   // Retorna : "SELECT t1.id,t2.titulo FROM tabela1 t2 LEFT JOIN tabela2 t2 (t2.id=t1.id) WHERE t1.id='1'"

   // A função query() executa o script, então você pode montar a consulta e executar quando necessário.
  $db->select()
     ->from('tabela')
     ->where('id',5);
     $ordem = $_GET['ordem']; // nome
     if(isset($ordem) and strlen($ordem)>0){
       $db->order_by($ordem,'DESC');
     }
  $db->query();
  // Retorna : "SELECT * FROM tabela WHERE id='5' ORDER BY nome DESC"

  // Outras funções que você pode utilizar
  $db->where_in('tipo',array(11,12,13)); // "AND tipo IN (11,12,13)"
  $db->where_not_in('tipo',array('a','b','c')); // "AND tipo NOT IN ('a','b','c')"
  $db->where_or_in('tipo',array(11,12,13)); // "OR tipo IN (11,12,13)"
  $db->where_or_not_in('tipo',array(1,2,3)); // "OR tipo NOT IN (1,2,3)"
  $db->where("FIND_IN_SET(".$db->escape('site').",para)"); // AND FIND_IN_SET('site',para)
  $db->where(array("date_format(valido_ate, '%Y-%m-%d %h:%i:%s') >="=>"date_format(NOW(), '%Y-%m-%d %h:%i:%s')"));
  $db->where("para LIKE","'%teste%'"); // AND para LIKE '%teste%'
  $db->where("para REGEXP('site')"); // AND para REGEXP('site')
  $db->where("DATE_ADD(P.inserido_em, INTERVAL 7 DAY) >= ",'NOW()');
  $db->where("UPPER(name)='".strtoupper('nome')."'");

  $db->escape('tipo') // retorna: 'tipo'
  $db->escape(null) // retorna: NULL
  $db->escape(FALSE) // retorna: 0
  $db->escape(TRUE) // retorna: 1

  $db->where('a<',1);
  $db->update('usuarios',array('nome'=>'Ramon'),array('id'=>'1'),'0,30');
  // Retorna : UPDATE usuarios SET `nome` = 'Ramon' WHERE a<'1' AND id='1' LIMIT 0,30

  $table = 'test';
  $set = array('id'=>1,'title'=>'Title','description'=>'Descrição');
  $affected_rows = $this->db->insert($table,$set);
  // Retorna: INSERT INTO test ( `id`,`title`,`description` ) VALUES ( '1','Title','Descrição' )

{% endhighlight %}

[classe]: https://gist.github.com/rbarros/3078427/download
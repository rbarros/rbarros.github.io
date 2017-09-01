---
layout: post
title:  "Instalação do Ubuntu Server 16.04"
description: "Tutorial de instalação do Ubuntu Server 16.04"
date:   2016-09-07 22:33
categories: php
tags: [ubuntu,server,webserver]
#image: /images/ubuntu-server/u2.png
---

A instalação do Ubuntu Server não tem mistério é bem intuitiva, pois há um ambiente gráfico, onde pode-se utilizar teclado para selecionar as opções.

Inicio do boot pelo CD ou Pendrive (no meu caso .iso através do qemu)

![u1](/images/ubuntu-server/u1.png "Boot pelo CD")

![u2](/images/ubuntu-server/u2.png "Instalar o Ubuntu Server")

Os primeiros passos são para selecionar o idioma e layout do teclado.

![u3](/images/ubuntu-server/u3.png "Informações sobre pacote de idiomas")

![u4](/images/ubuntu-server/u4.png "Seleção do Idioma para o teclado")

![u5](/images/ubuntu-server/u5.png "Seleção do layout do teclado")

Após a seleção do layout do teclado o instalador irá buscar unidades de cd-rom

![u6](/images/ubuntu-server/u6.png "Deteção de ubidades de cd-rom")

![u7](/images/ubuntu-server/u7.png "Carregando componentes adicionais")

![u8](/images/ubuntu-server/u8.png "Configurar a rede")

![u9](/images/ubuntu-server/u9.png "Configurar usuários e senhas - Nome completo")

![u10](/images/ubuntu-server/u10.png "Configurar usuários e senhas - Usuário")

![u11](/images/ubuntu-server/u11.png "Configurar usuários e senhas - Senha")

![u12](/images/ubuntu-server/u12.png "Alerta de senha fraca")

![u13](/images/ubuntu-server/u13.png "Encriptar pasta pessoal")

![u15](/images/ubuntu-server/u15.png "Detectando discos e todo o restante do hardware")

No particionamento do disco foi selecionado o LVM pois possibilita redimencionar as parções conforme a necessidade.

![u17](/images/ubuntu-server/u17.png "Particionar discos")

![u18](/images/ubuntu-server/u18.png "Gravar mudanças nos discos e configurar LVM")

![u19](/images/ubuntu-server/u19.png "Tamanho da partição a ser utilizada no partifionamento")

![u20](/images/ubuntu-server/u20.png "Iniciando o particionador")

![u21](/images/ubuntu-server/u21.png "Escrever as mudanças nos discos")

![u22](/images/ubuntu-server/u22.png "Formatação de partições")

Depois da formatação das partições necessárias para o sistema o instalador irá iniciar a instalação do sistema.

![u23](/images/ubuntu-server/u23.png "Instalando o sistema")

![u24](/images/ubuntu-server/u24.png "Configurando o apt")

![u25](/images/ubuntu-server/u25.png "Selecionar e instalar softwar")

![u26](/images/ubuntu-server/u26.png "Configuração de atualização automáica")

Nesta tela é possível selecionar os softwares que serão instalados neste servidor, no meu caso selecionei o LAMP que ira instalar automaticamente o Apache2, PHP 7 e MySQL 5.7 (servidor web), Samba file server para compartilhamento de arquivos, e OpenSSH para conexão remota por ssh.

![u27](/images/ubuntu-server/u27.png "Seleção de software")

Definição da senha do banco de dados MySQL (caso selecionado o LAMP)

![u28](/images/ubuntu-server/u28.png "Configurando mysql-server-5.7")

![u29](/images/ubuntu-server/u29.png "repetir senha do mysql")

![u30](/images/ubuntu-server/u30.png "Instalaçãdo do mysql")

![u31](/images/ubuntu-server/u31.png "Instalando o pacote grub2")

Será solicitado se você deseja instalar o grub no registro mestre de inicialização, se houver outro sistema operacional (dual boot) o grub irá acrescentar (caso não ocorra falhas) na lista de sistema operacionais.

![u32](/images/ubuntu-server/u32.png "Instalar o grub no registro mestre")

![u33](/images/ubuntu-server/u33.png "Instalação do grub no disco")

![u34](/images/ubuntu-server/u34.png "Finalizando a instalação")

Após a instalação o sistema ira solicitar que remova qualquer midia cd-rom ou usb e reiniciará o servidor, aparecerá o Grub contendo os
sistemas operacionais detectados.

![u35](/images/ubuntu-server/u35.png "Grub")

![u36](/images/ubuntu-server/u36.png "Inicializaçãdo do servidor")

Pronto o Ubuntu Server foi instalado e você estará pronto para começar a configurar e trabalhar nele.

![u37](/images/ubuntu-server/u37.png "Informações do servidor")

Fique atento para mais tutoriais, como o LVM e configurações básicas para trabalhar com um servidor web.
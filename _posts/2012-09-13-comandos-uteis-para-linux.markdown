---
layout: post
title:  "Comandos uteis para Linux"
date:   2012-09-13 10:09:00
categories: linux
tags: [linux,comandos, shell]
---
Alguns comandos do linux que costumo utilizar.

{% highlight bash %}
# Apache2
#Editar arquivo php.ini Ubuntu 12.04
$ sudo gedit /etc/php5/apache2/php.ini

# Reiniciar Apache
$ sudo /etc/init.d/apache2 restart
# ou
$ sudo service apache2 restart

# Ativar mod_rewrite
$ sudo a2enmod rewrite

# clearfiles
# Encontra determinado arquivo e exclui 
# Utilizo para limpar alguns projetos. 

$ find -name ".DS_Store" -exec rm -v '{}' \;
$ find -name "Thumbs.db" -exec rm -v '{}' \;
$ find -name "*.esproj" -exec rm -v '{}' \;
$ find -name ".cache" -type d -exec rm -rv '{}' \;
$ find -name ".project" -type d -exec rm -rv '{}' \;
$ find -name ".settings" -type d -exec rm -rv '{}' \;
$ find -name ".tmproj" -type d -exec rm -rv '{}' \;
$ find -name ".svn" -type d -exec rm -rv '{}' \;
$ find -name "_notes" -type d -exec rm -rv '{}' \; 
$ find -name "*bak*" -type f -exec rm -v '{}' \; 
$ find -name "*bkp*" -type f -exec rm -v '{}' \;

# Compactar arquivo tar
$ tar -cvf arquivo.tar /diretorio/*

# Compactar arquivo tar respeitando a estrutura do diretórios
$ tar -cvf arquivo.tar ./diretorio/*

# Compactar opção z
$ tar -czvf arquivo.tar.gz /diretorio/*

# Descompactar arquivo zip:
$ gunzip arquivo.zip

# Descompactar arquivo rar:
$ unrar x arquivo.rar

# Descompactar arquivo tar:
$ tar -xvf arquivo.tar

# Descompactar arquivo tar.gz:
$ tar -xzvf arquivo.tar.gz

# Descompactar arquivo bz2:
$ bunzip  arquivo.bz2

# Descompactar arquivo tar.bz2:
$ tar -jxvf arquivo.tar.bz2

# Compoartilhar pasta
#Ubuntu
$ sudo mount -t smbfs -o username=usuario,password=senha //server/diretorio /mnt/server/diretorio
#Xubuntu
$ sudo mount -t cifs -o username=usuario,password=senha //server/diretorio /mnt/server/diretorio

# Acessar url com curl
$ curl 'url'
$ curl [option] 'url'
$ curl -O 'url'
$ curl -L -O 'url'

# Download
$ curl -o output.file.name.here 'url-here'
$ curl -o foo.pdf 'http://example.com/foo.pdf'

# dpkg
# apt-get autoclean
# apt-get clean
# apt-get update
# apt-get upgrade
# apt-get --reinstall install

for i in $(dpkg -l|awk '/^ii/ {print $2}')
do
    apt-get --reinstall -y install $i
done

# Mostra o tamanho de cada subdiretorio 
$ du -h --max-depth=1 

# Mostra o tamanho total do diretório 
$ du -sh /diretorio/ 

# Mostra o tamanho total dos subdiretório 
$ du -sh /diretorio/*

# mysqlExport
$ mysqldump -u user -p database | gzip > database.sql.gz

# mysqlImport
$ find -name "*.sql" | awk '{ print "source",$0 }' | mysql -u root db
$ gunzip < database.sql.gz | mysql -u user -p database

# Atualização do nodejs

$ sudo npm cache clean -f
$ sudo npm install -g n
$ sudo n stable
$ sudo ln -sf /usr/local/n/versions/node/<VERSION>/bin/node /usr/bin/node

# Partições
$ cat /etc/fstab
# ou
$ df

## Criar pendrive bootavel pelo terminal
# Instalar pv para mostrar o processo de gravação
$ sudo apt-get update
$ sudo apt-get install pv
$ sudo fdisk -l # Identificar o pendrive
Dispositivo Inicializar Start      Fim  Setores  Size Id Tipo
/dev/sdb1   *            2048 15630335 15628288  7,5G  c W95 FAT32 (LBA)
$ sudo mkfs.vfat /dev/sdb1 # Formatar o pendrive
mkfs.fat 3.0.28 (2015-05-16)
$ dd if=/home/ramon/Downloads/xubuntu-15.04-desktop-amd64.iso |pv|dd of=/dev/sdb1 bs=1M
1972224+0 registros de entrada=>                                               ]
1972224+0 registros de saída
 963MiB 0:01:39 [9,63MiB/s] [<=>                                               ]
1009778688 bytes (1,0 GB) copiados, 99,9754 s, 10,1 MB/s
5+9474 registros de entrada
5+9474 registros de saída
1009778688 bytes (1,0 GB) copiados, 103,303 s, 9,8 MB/s

# Release
$ cat /etc/issue
$ cat ls /etc/*release /etc/*version
$ lsb_release -a
# Arquiterura
$ file /bin/bash | cut -d' ' -f3
32-bit
# or
64-bit

# smb
$ cat /etc/samba/smb.conf

# User
$ sudo chown user:group -hR path

{% endhighlight %}

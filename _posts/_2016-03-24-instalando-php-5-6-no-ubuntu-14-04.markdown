---
layout: post
title:  "Instalando PHP 5.6 no Ubuntu 14.04"
date:   2016-01-21 16:20:00
categories: arduino
tags: [arduino]
---
15.10  wily       jessie / sid
15.04  vivid      jessie / sid
14.10  utopic     jessie / sid
14.04  trusty     jessie / sid
13.10  saucy      wheezy / sid
13.04  raring     wheezy / sid
12.10  quantal    wheezy / sid
12.04  precise    wheezy / sid
11.10  oneiric    wheezy / sid
11.04  natty      squeeze / sid
10.10  maverick   squeeze / sid
10.04  lucid      squeeze / sid

This PPA contains latest PHP 5.5 packaged for Ubuntu 14.04 LTS (Trusty).

You can get more information about the packages at https://deb.sury.org

If you need other PHP versions use:
  PHP 5.4: ppa:ondrej/php5-oldstable (Ubuntu 12.04 LTS)
  PHP 5.5: ppa:ondrej/php5 (Ubuntu 14.04 LTS)
  PHP 5.6: ppa:ondrej/php5-5.6 (Ubuntu 14.04 LTS - Ubuntu 16.04 LTS)
  PHP 5.6 and PHP 7.0: ppa:ondrej/php (Ubuntu 14.04 LTS - Ubuntu 16.04 LTS)

BUGS&FEATURES: This PPA now has a issue tracker: https://deb.sury.org/pages/bugreporting.html

PLEASE READ: If you like my work and want to give me a little motivation, please consider donating: https://deb.sury.org/pages/donate.html

WARNING: add-apt-repository is broken with non-UTF-8 locales, see https://github.com/oerdnj/deb.sury.org/issues/56 for workaround:

# apt-get install -y language-pack-en-base
# LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php5

$ sudo add-apt-repository ppa:ondrej/php5-5.6
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get autoremove

$ sudo apt-get install php5
...
Unpacking apache2-bin (from .../apache2-bin_2.4.16-4+deb.sury.org~precise+4_amd64.deb) ...
dpkg: error processing /var/cache/apt/archives/apache2-bin_2.4.16-4+deb.sury.org~precise+4_amd64.deb (--unpack):
 trying to overwrite '/usr/share/man/man8/apache2.8.gz', which is also in package apache2.2-common 2.2.22-1ubuntu1.4
dpkg-deb (subprocess): subprocess data was killed by signal (Broken pipe)
dpkg-deb: error: subprocess <decompress> returned error exit status 2
Processing triggers for man-db ...
Errors were encountered while processing:
 /var/cache/apt/archives/apache2-bin_2.4.16-4+deb.sury.org~precise+4_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)

$ sudo dpkg -i --force-overwrite /var/cache/apt/archives/apache2-bin_2.4.16-4+deb.sury.org~precise+4_amd64.deb
$ sudo dpkg -i --force-overwrite /var/cache/apt/archives/apache2_2.2.22-1ubuntu1.10_amd64.deb

$ sudo apt-get -f install (or) apt-get install php5

Configuration file `/etc/logrotate.d/apache2'
 ==> Modified (by you or by a script) since installation.
 ==> Package distributor has shipped an updated version.
   What would you like to do about it ?  Your options are:
    Y or I  : install the package maintainer's version
    N or O  : keep your currently-installed version
      D     : show the differences between the versions
      Z     : start a shell to examine the situation
 The default action is to keep your current version.
*** apache2 (Y/I/N/O/D/Z) [default=N] ? Y
...
Configuration file `/etc/apache2/mods-available/ssl.conf'
...
Configuration file `/etc/apache2/mods-available/php5.conf'
...
Configuration file `/etc/apache2/apache2.conf'

$ php -v
PHP 5.6.14-1+deb.sury.org~precise+1 (cli)
Copyright (c) 1997-2015 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2015 Zend Technologies
    with Zend OPcache v7.0.6-dev, Copyright (c) 1999-2015, by Zend Technologies

$ sudo a2enmod authz_host
$ sudo service apache2 restart


MYSQL

# mysql_install_db
# mysqladmin -u root password 'new-password'
# chown -R mysql.mysql /var/lib/mysql
# safe_mysqld &
# mysql


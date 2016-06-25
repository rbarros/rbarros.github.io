---
layout: post
title:  "Mensagem brcmsmac na inicialização do Xubuntu 16.04/Ubuntu 14.04.3"
description: "Resolvendo problema do kernel 4.2.0 e Broadcom Corporation BCM4313."
date:   2016-03-31 10:51:00
categories: php
tags: [xubuntu,ubuntu,brcmsmac, brcms, broadcom]
---

Tenho um notebook HP G42 e após a atualização do Xubuntu 14.04 para o 16.04 começou aparecer a mensagem abaixo e apresentar problemas na conexão wifi:
```
brcmsmac bcma0:1: brcms_ops_bss_info_changed: qos enabled: false (implement)
brcmsmac bcma0:1: brcms_ops_config: change power-save mode: false (implement)
```

Digitei o comando ```lspci``` no terminal para verificar o modelo da placa wifi.

```
Network controller: Broadcom Corporation BCM4313 802.11bgn Wireless Network Adapter (rev 01)
```

Pesquisei no forum do [ubuntu](http://ubuntuforums.org/showthread.php?t=2148444&page=2) e informaram que resolveria instalando o bcmwl-kernel-source, seguindo as dicas tentei instalar pelo synaptic, no entanto não funcionou mostrando outro problema:
```
A descompactar bcmwl-kernel-source (6.30.223.141+bdcom-0ubuntu2)
...
ERROR (dkms apport): kernel package linux-headers-4.2.0-25-generic is not supported
Error! Bad return status for module build on kernel: 4.2.0-25-generic (x86_64)
Consult /var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/make.log for more information.
modprobe: FATAL: Module wl not found in directory /lib/modules/4.2.0-25-generic
...
```

Tentei baixar o bcmwl-kernel-source e instalar pelo ```dpkg``` apresentando o mesmo problema:
```
DKMS make.log for bcmwl-6.30.223.141+bdcom for kernel 4.2.0-25-generic (x86_64)
Sex Jun 24 20:41:43 BRT 2016
make: Entering directory '/usr/src/linux-headers-4.2.0-25-generic'
CFG80211 API is prefered for this kernel version
Using CFG80211 API
  LD      /var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/built-in.o
  CC [M]  /var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/src/shared/linux_osl.o
  CC [M]  /var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/src/wl/sys/wl_linux.o
/var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/src/wl/sys/wl_linux.c: In function ‘wl_alloc_linux_if’:
/var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/src/wl/sys/wl_linux.c:1354:64: error: macro "alloc_netdev" requires 4 arguments, but only 3 given
  dev = alloc_netdev(sizeof(priv_link_t), intf_name, ether_setup);
                                                                ^
/var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/src/wl/sys/wl_linux.c:1354:8: error: ‘alloc_netdev’ undeclared (first use in this function)
  dev = alloc_netdev(sizeof(priv_link_t), intf_name, ether_setup);
        ^
/var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/src/wl/sys/wl_linux.c:1354:8: note: each undeclared identifier is reported only once for each function it appears in
/var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/src/wl/sys/wl_linux.c: In function ‘wl_dump_ver’:
/var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/src/wl/sys/wl_linux.c:2047:3: error: macro "__DATE__" might prevent reproducible builds [-Werror=date-time]
   __DATE__, __TIME__, EPI_VERSION_STR);
   ^
/var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/src/wl/sys/wl_linux.c:2047:13: error: macro "__TIME__" might prevent reproducible builds [-Werror=date-time]
   __DATE__, __TIME__, EPI_VERSION_STR);
             ^
cc1: some warnings being treated as errors
scripts/Makefile.build:258: recipe for target '/var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/src/wl/sys/wl_linux.o' failed
make[1]: *** [/var/lib/dkms/bcmwl/6.30.223.141+bdcom/build/src/wl/sys/wl_linux.o] Error 1
Makefile:1398: recipe for target '_module_/var/lib/dkms/bcmwl/6.30.223.141+bdcom/build' failed
make: *** [_module_/var/lib/dkms/bcmwl/6.30.223.141+bdcom/build] Error 2
make: Leaving directory '/usr/src/linux-headers-4.2.0-25-generic'
```

Resolvi atualizar o kernel de 4.2.0 para 4.6.0 baixando as imagens em [kernel](http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.6-yakkety/)

* linux-headers-4.6.0-040600_4.6.0-040600.201606100558_all.deb
* linux-headers-4.6.0-040600-generic_4.6.0-040600.201606100558_amd64.deb
* linux-image-4.6.0-040600-generic_4.6.0-040600.201606100558_amd64.deb

```
sudo dpkg -i linux-*.deb
```

E depois baixei um versão mais recente do [bcmwl](https://launchpad.net/ubuntu/xenial/amd64/bcmwl-kernel-source/6.30.223.248+bdcom-0ubuntu8) e instalei:
```
sudo dpkg -i bcmwl-kernel-source_6.30.223.248+bdcom-0ubuntu8_amd64.deb
```

Foi instalado corretamente o kernel e o driver bcmwl e a wifi voltou a funcionar.

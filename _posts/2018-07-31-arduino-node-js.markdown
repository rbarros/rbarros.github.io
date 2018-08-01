---
layout: post
title:  "Programando em JS com Node.js no Arduino"
date:   2013-01-31 22:01:00
categories: arduino
tags: [arduino,nodejs,node,js,javascript]
image: /images/arduino/j5/johnny-five.png
---
Bom este tutorial eu criei para facilitar para o desenvolvedor que gostaria de
programar em javascript para o Arduino e criar uma página web de forma mais
fácil. Dedico este post ao meu amigo Felipe, que estava buscando uma forma de
fazer isso também em seus projetos, sem ter que adicionar tags html no código em
c++.

![johnny-five](/images/arduino/j5/johnny-five.png "Johnny-Five")

Rick Waldron é o criador do projeto [Johnny-Five][j5] que utiliza JavaScript para
o desenvolvimento de robótica. Existem varios exemplos no website do projeto com
a possibilidade de utilizar sensores, motores, gpios, etc. Suporta uma variedade
de [hardwares][platform]:

![platform-support](/images/arduino/j5/platform-support.png "Platform Support")

O que precisa para começar ?

Bom em primeiro lugar você vai precisar do [Node.js][nodejs] instalado e da 
[IDE][ide] do Arduino. No meu caso estou utilizando uma distribuição Linux, 
a Xubuntu, uma derivada do Ubuntu.

No Ubuntu/Xubuntu basta baixar o Arduino IDE conforme a arquitetura do seu
processador 32 ou 64 bits.

![download-arduino-ide](/images/arduino/download-arduino-ide.jpg "Download Arduino IDE")

[j5]: http://johnny-five.io
[platform]: http://johnny-five.io/platform-support
[nodejs]: https://nodejs.org
[ide]: https://www.arduino.cc/en/Main/Software
---
layout: post
title:  "Programando em JS com Node.js no Arduino"
date:   2018-07-31 21:00
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

Rick Waldron é o criador do projeto [Johnny-Five][j5]{:target="_blank"} que utiliza JavaScript para
o desenvolvimento de robótica. Existem varios exemplos no website do projeto com
a possibilidade de utilizar sensores, motores, gpios, etc. Suporta uma variedade
de [hardwares][platform]{:target="_blank"}:

![platform-support](/images/arduino/j5/platform-support.png "Platform Support")

O que precisa para começar ?

Bom em primeiro lugar você vai precisar do [Node.js][nodejs]{:target="_blank"} instalado e da 
[IDE][ide]{:target="_blank"} do Arduino. No meu caso estou utilizando uma distribuição Linux, 
a Xubuntu, uma derivada do Ubuntu.

No Ubuntu/Xubuntu basta baixar o Arduino IDE conforme a arquitetura do seu
processador 32 ou 64 bits.

![download-arduino-ide](/images/arduino/download-arduino-ide.jpg "Download Arduino IDE")

No caso do Linux o arquivo é como este arduino-1.8.5-linux64.tar.xz, basta você
descompactar no diretório que quiser e depois abrir com o terminal o arquivo 
install.sh uma janela como esta irá aparecer e iniciar a instalação.

![install-sh](/images/arduino/install.sh.png "Instalação do Arduino IDE")

Após a instalação é criado o icone da IDE na sua Área de Trabalho e menu.

![arduino-instalado](/images/arduino/arduino-instalado.png "Arduino IDE instalado")

Abra o Arduino IDE, copie e cole o código do link abaixo:

[StandardFirmataPlus.ino][StandardFirmataPlus.ino]{:target="_blank"}

Clique em verificar e depois em carregar, caso ocorra algum erro como este no terminal:

{% highlight bash %}
avrdude: ser_open(): can't open device "/dev/ttyACM0": Permission denied
ioctl("TIOCMGET"): Inappropriate ioctl for device
{% endhighlight %}

Você deve dar permissão no dispositivo, digitando os dois comandos abaixo:
{% highlight bash %}
sudo usermod -a -G dialout $USER
sudo chmod a+rw /dev/ttyACM0
{% endhighlight %}

Tente novamente carregar o código no Arduino, se funcionar você vai ver no console
da IDE algum como isto:

![arduino-carregado](/images/arduino/arduino-carregado.png "Arduino IDE Compilado e Carregado")

Agora seu Arduino esta pronto para ser chamado pelo Johnny-Five.
Crie um diretório em algum lugar de sua preferencia e cole este código em um
arquivo, por exemplo board.js:

{% highlight js %}
// board.js
var five = require("johnny-five");
var board = new five.Board();

// The board's pins will not be accessible until
// the board has reported that it is ready
board.on("ready", function() {
  console.log("Ready!");

  var led = new five.Led(13);
  led.blink(500);
});
{% endhighlight %}

Em um terminal execute este comando para instalar o johnny-five:

{% highlight js %}
# Instalação da biblioteca johnny-five para o Node.js
npm install johnny-five
{% endhighlight %}

No mesmo terminal digite este comando para executar o código em board.js
{% highlight js %}
node board.js
{% endhighlight %}

![board-js](/images/arduino/j5/board.js.png "Execução do código no Arduino")

Se tudo der certo você verá que a luz padrão do Arduino começará a piscar.

![video-arduino](/images/arduino/j5/video-arduino.gif "Video do Arduino"){:style="width:400px;"}

[j5]: http://johnny-five.io
[platform]: http://johnny-five.io/platform-support
[nodejs]: https://nodejs.org
[ide]: https://www.arduino.cc/en/Main/Software
[StandardFirmataPlus.ino]: https://raw.githubusercontent.com/firmata/arduino/master/examples/StandardFirmataPlus/StandardFirmataPlus.ino
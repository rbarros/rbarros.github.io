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

# Johnny Five
![johnny-five](/images/arduino/j5/johnny-five.png "Johnny-Five")

Rick Waldron é o criador do projeto [Johnny-Five][j5]{:target="_blank"} que utiliza JavaScript para
o desenvolvimento de robótica. Existem varios exemplos no website do projeto com
a possibilidade de utilizar sensores, motores, gpios, etc. Suporta uma variedade
de [hardwares][platform]{:target="_blank"}:

![platform-support](/images/arduino/j5/platform-support.png "Platform Support")

# O que precisa para começar ?

Bom em primeiro lugar você vai precisar do [Node.js][nodejs]{:target="_blank"} instalado e da 
[IDE][ide]{:target="_blank"} do Arduino. No meu caso estou utilizando uma distribuição Linux, 
a Xubuntu, uma derivada do Ubuntu.

No Ubuntu/Xubuntu basta baixar o Arduino IDE conforme a arquitetura do seu
processador 32 ou 64 bits.

# Instalação do Arduino IDE
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

# Firmata - Comunicação com o Arduino usando o JS
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

Agora podemos alterar o código javascript para utilizar o express para criar 
rotas para acessarmos os recursos do Arduino.

Para ficar mais fácil você pode baixar ou clonar este repositório [Arduino-J5] e
dentro deste diretório executar o comando:
{% highlight js %}
# Instalação das bibliotecas para o projeto
npm install
{% endhighlight %}

![npm-install](/images/arduino/j5/npm-install.png "npm install")

Se você analizar o código `server.js` verá que tem algumas rotas:
1. / - Rota principal que exibe uma mensagem
2. /public - Rota public necessária para carregar arquivos no dashboard.html
3. /dashboard - Rota que exibe o arquivo dashboard.html
4. /:pin/state - Rota para exibir o estado do pino
5. /led/off - Rota para apagar a luz do pino 13
6. /led/on - Rota para acender a luz do pino 13

No arquivo `dashboard.html` utilizei o bootstrap para
criar uma página bem simples onde tem um botão para ligar e desligar a luz do pino 13 (luz padrão do Arduino ou a que estiver conectada).

![localhost](/images/arduino/j5/localhost.png "localhost")

O script `public/arduino.js` foi criado para poder
chamar a rota `/led/on` e '/led/off` quando clicado
no botão On ou Off e tem a função também de alterar o
texto e cor do botão.

![arduino-j5-dashboard](/images/arduino/j5/arduino-j5-dashboard.jpeg "Arduino J5 Dashboard")

Segue o funcionamento da página de exemplo ao clicar no botão On ou Off.

![video-dashboard](/images/arduino/j5/video-dashboard.gif "Video Arduino J5 Dashboard")

A vantagem de utilizar o Johnny-Five é que você pode programar utilizando o JavaScript, criar rotas e varios outros recursos aproveitando o máximo que o Node.js pode oferecer.

No entanto o Arduino depende de um computador para manter a conexão aberta pelo terminal `node server.js` uma alternativa seria utilizar
uma shield Wifi para o Arduino Uno, neste caso não precisaria ficar
conectado ao PC, mas ainda iriamos precisar de um computador ou dispositivo para enviar os dados do js para o Arduino.

Para utilização do Johnny-Five sem um computador enviando dados seria necessário utilizar outro tipo de embarcado um [Tessel](https://tessel.io/) ou Raspberry Pi, na qual é possível conectar por ssh e executar os códigos js dentro deles.

[j5]: http://johnny-five.io
[platform]: http://johnny-five.io/platform-support
[nodejs]: https://nodejs.org
[ide]: https://www.arduino.cc/en/Main/Software
[StandardFirmataPlus.ino]: https://raw.githubusercontent.com/firmata/arduino/master/examples/StandardFirmataPlus/StandardFirmataPlus.ino
[Arduino-J5]: https://github.com/rbarros/Arduino-J5.git
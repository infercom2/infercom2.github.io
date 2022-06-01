---
layout: single
title: GSM con Osmocom - Introducción Parte 5 - Software de BTS con LimeSDR y osmo-bts-trx 
excerpt: "Usando un LimeSDR (LimeSoftware Define Radio) como BTS de GSM con Osmocom"
date: 2022-05-27
classes: wide
header:
  teaser: /assets/images/osmocom/p5/LimeSDR-Osmocom.png
  teaser_home_page: true
  icon: /assets/images/osmocom/p1/Osmo.webp
categories:
  - osmocom
  - networking
tags:
  - linux
  - osmocom
  - gsm
  - mobile
  - telephony
---

Usando un LimeSDR (LimeSoftware Define Radio) como BTS de GSM con Osmocom

![](/assets/images/osmocom/p5/LimeSDR-Osmocom.png)


Osmo-BSC acepta conexiones Abis sobre IP desde diferente número de fuentes.
Existe una lista de equipos BTS que pueden hablar con el Osmo-BSC, como la familia de Ericsson RBS, el ip.access nanoBTS, las BTS de Nokia y Siemens e incluso una BTS virtual para simular las conexiones.

Si estás utilizando alguna de las BTS arriba mencionadas, o la osmo-BTS-virtual, probablemente sólo necesitas configurar lo básico en tu BTS y apuntarla hacia tu BSC.

El siguiente apartado trata de cómo usar un hardware comun SDR como BTS. Si no estás usando hardware SDR, puedes saltarlo y continuar en el apartado sobre BSC.

## Osmo-TRX
Para poder manejar un arreglo grande de hardware SDR, Osmocom ha introducido el Osmo-TRX, el cual soporta la capa física (Layer 1) de la BTS y se conecta a la Osmo-BTS, la cual funge como BTS y habla con la MSC a través del protocolo Abis sobre IP.


Algunas BTS pueden comunicarse directo con Osmo-BTS, pero vamos a utilizar Osmo-TRX como el intermediario entre nuestro hardware SDR y la BTS.

![](/assets/images/osmocom/p5/osmo-trx.png)

El diagrama de arriba, tomado de la wiki de Osmocom, muestra cómo interactúan todos los módulos con nuestra plataforma genérica SDR. 
A continuación se muestra cómo funciona para nosotros:

![](/assets/images/osmocom/p5/osmo-trx-lms.png)

_osmo-trx-lms_ se encargará del lado del SDR de la ecuación, muy parecido a como si fuera un módem, enviando todo lo que obtiene en la interfaz Uu hacia _osmo-bts-trx_ sobre el protocolo UDP, y todo lo que recibe de _osmo-bts-trx_  enviándolo sobre UDP hacia la interfaz Uu.

Osmo-bts-trx entonces establecerá una conexión hacia la BSC con el protocolo Abis sobre IP.

## LimeSDR 
Mi creciente colección de hardware SDR ahora incluye un LimeSDR, el cual estaré utilizando para esta serie.

![](/assets/images/osmocom/p5/LimeSDR-Osmocom.png)

Antes de ir más allá, debemos establecer los pre-requisitos para que el LimeSDR pueda conectarse con Osmo-trx.

Osmocom ahora provee un binario para inter conectarse con la tarjeta del Lime SDR directamente en lugar de usar la abstracción UHD. Esta es una manera mucho más limpia de conectarse con las tarjetas y el camino que tomaré.

## Instalación del software 

Para este tutorial estaré utilizando Ubuntu 18.04 y utilizar, en la medida de lo posible, los paquetes de los Repos en lugar de compilar desde la fuente.

La suite de Lime provee los controladores y utilerías para conectarse con el LimeSDR.

```
add-apt-repository -y ppa:myriadrf/drivers
apt-get update
apt-get install limesuite limesuite-udev
```

Después conectaré el LimeSDR a un puerto USB3 y confirmaré la conexión y actualizaré su firmware:

```
LimeUtil --find
```


Asumiendo que tu LimeSDR se conectó y todo lo instalado está correcto, deberás ver un mensaje similar al siguiente:

![](/assets/images/osmocom/p5/LimeSDR-Found.png)

En este caso, podemos actualizar el firmware de LimeSDR con:

```
LimeUtil --update
``` 


En el siguiente paso, empezaremos a instalar las fuentes de Osmocom:

```
wget https://download.opensuse.org/repositories/network:/osmocom:/latest/Debian_10/Release.key  
apt-key add Release.key && rm Release.key
echo "deb https://download.opensuse.org/repositories/network:/osmocom:/latest/xUbuntu_18.04/ ./" > /etc/apt/sources.list.d/osmocom-latest.list 
apt-get update
```


Ahora que ya tenemos los Repos de Osmocom Debian agregados, podemos instalar los paquetes que necesitamos.
Instalaremos Osmo-BTS-TRX para hablar con la BSC a través de Abis, e instalaremos Osmo-TRX-LMS para hablar con el SDR.

```
apt-get install osmo-bts-trx osmo-trx-lms 
``` 

Después de que instalaste los paquetes, Osmo-BTS-TRX correará un daemon, lo dentendremos por ahora y recuperaremos manualmente al primer plano.

```
systemctl disable osmo-bts-trx
systemctl disable osmo-trx-bts
``` 

## Configuración de Software 

Ahora que ya tenemos dos piezas del rompecabezas, es momento de conectar el SDR al Osmo-TRX-LMS y el Osmo-TRX-LMS al Osmo-BTS-TRX.

Empezaremos por correr el Osmo-TRX-LMS para conectarlo al LimeSDR y encapsular los datos Uu dentro de paquetes UDP que enviaremos a Osmo-BTS-TRX.

Archivos de configuración para Osmocom están instalados en /etc/osmocom/, así que correremos todo desde ese directorio.

```
osmo-trx-lms -C osmo-trx-lms.cfg
``` 

Si todo fue exitoso, verás algo similar a lo que yo obtuve abajo, mostrando que Osmo-TRX-LMS se ha conectado al SDR y está listo para arrancar.

![](/assets/images/osmocom/p5/Osmo-trx-lms-LimeSDR-Connection.png)


Pero si escaneas las ondas de radio ahora, no encontrarás ningún dato saliendo del transmisor del SDR. Eso es porque Osmo-TRX-LMS necesita conectarse al Osmo-BTS-TRX.
Dejaremos corriendo Osmo-TRX-LMS, así que abramos otra sesión y arranquemos Osmo-BTS-TRX.


```
osmo-bts-trx -c osmo-bts-trx.cfg
``` 

Para empezar, verás que nuestro trans-receptor está abierto (¡hurra!): 

![](/assets/images/osmocom/p5/osmo-bts-trx-No-Abis.png)

Verás reflejado lo anterior en el stdout de Osmo-TRX-LMS, pero mostrará que el comando de apagado (poweroff) ha sido enviado  a éste. ¿Entonces?

![](/assets/images/osmocom/p5/transciver-power-off.png)

La respuesta se vuelve clara si dejas Osmo-BTS-TRX corriendo por uno o dos minutos. Eventualmente el proceso se detiene, reportando:

```
<000d> abis.c:142 Signalling link down
<0001> bts.c:292 Shutting down BTS 0, Reason Abis close
```

![](/assets/images/osmocom/p5/osmo-bts-no-abis.png)

¿Qué está pasando? De la misma manera en que vimos nuestra BTS virtual apagarse solita, sin una conexión a la BSC (vía la interfaz Abis), la BTS se apagará solita y no será capaz de arrancar por sí sola.

Penosamente me tomó mucho tiempo descubrir porqué se estaba parando…

En nuestro siguiente apartado introduciré nuestra BSC y el aprovisionamiento de una BTS en ella.



## Fuente Nickvsnetworking
[GSM with Osmocom Part 5 Software BTS with LimeSDR and osmo-bts-trx](https://nickvsnetworking.com/gsm-with-osmocom-part-3-bts-in-practice-with-limesdr-osmo-bts-trx/)
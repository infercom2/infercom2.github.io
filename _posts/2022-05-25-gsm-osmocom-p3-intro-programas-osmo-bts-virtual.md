---
layout: single
title: GSM con Osmocom - Introducción Parte 3 - Programas Osmo y BTS Virtual
excerpt: "Configuración de una BTS Virtual en nuestra BSC con Osmocom."
date: 2022-05-25
classes: wide
header:
  teaser: /assets/images/osmocom/p2/GSM-Architecture-1.png
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

Configuración de una BTS Virtual en nuestra BSC con Osmocom.

![](/assets/images/osmocom/p2/GSM-Architecture-1.png)

Esta serie de publicaciones se centrará en el uso de programas Osmocom para crear una red GSM, así que instalemos algunos programas de Osmocom, y hablaremos sobre cómo configurar y ejecutar cada elemento nodo de la red. 

## Paquetes de Osmocom 
Para esta serie de tutoriales usaré Ubuntu 18.04 e intentaré lo mayor posible usar paquetes de repos en lugar de compila desde la fuente. 

Esto hará que la clave de Osmocom se agregue a nuestro administrador de paquetes y las fuentes de Osmocom en apt estén lista para que la instalemos.

```
wget https://download.opensuse.org/repositories/network:/osmocom:/latest/Debian_10/Release.key
apt-key add Release.key && rm Release.key
echo "deb https://download.opensuse.org/repositories/network:/osmocom:/latest/xUbuntu_18.04/ ./" > /etc/apt/sources.list.d/osmocom-latest.list
apt-get update

``` 

## Antes de continuar
Los paquetes de osmocom están divididos de la siguiente manera: 

### version latest:
Osmocom mantiene un conjunto de paquetes binarios que reflejan las últimas versiones etiquetadas según 
las etiquetas de versión en el repositorio git de origen. Se denominan network: osmocom: latest. 

Ellos recomiendan  a los usuarios que utilicen los paquetes latest para la mayoría de los casos de uso normales

### version nightly
Osmocom mantiene otro conjunto de paquetes binarios que se compilan automáticamente una vez cada 24 horas desde 
el maestro del repositorio de git de origen., Osmocom mantiene otro conjunto de paquetes binarios que se construyen automáticamente una vez cada 
24 horas desde el maestro del repositorio git fuente.


Recomiendan a los usuarios que utilicen compilaciones nightly solo si quieren mantenerse a la vanguardia, 
p. Ej. para probar algunas correcciones de errores o nuevas funciones que se fusionaron en master.


### Stable  release
El proyecto Osmocom planea ofrecer lanzamientos estables en algún momento en el futuro. Una versión estable 
sería un conjunto específico de paquetes en todos los repositorios. Las versiones recibirían mantenimiento 
y corrección de errores durante algún tiempo.

En este punto, todavía no tenemos los recursos para mantener versiones estables. Si necesita versiones estables 
y le gustaría contribuir con mano de obra o financieramente, háganoslo saber.


## Osmo-BTS-virtual


Para comenzar, instalaremos una BTS Virtual. Esta BTS Virtual no simulará la interfaz Um (aerea), pero simulará la interfaz Abis hacía la BSC para que podamos configurar esta BTS virtual en nuestra BSC.


La instalación es bastante sencilla: 

```
apt-get install osmo-bts-virtual
```

De forma predeterminada los programas de Osmocom se ejecutan como un demonio (daemon)  en systemctl , Lo deshabilitaremos y detendremos este servicio por ahora para que podamos entender mejor que se ejecuta en primer plano: 

<pre>
systemctl stop osmo-bts-virtual
systemctl disable osmo-bts-virtual
</pre>

## Archivos de Texto = Configuraciones de Osmo.

Si hechas un vistazo en _/etc/osmocom/_ verás archivos _.cfg_ que contienen nuestras configuraciones en archivos de texto.

Pero no este no es la única forma (incluso la recomendada) de armar la configuración de los programas de Osmocom, pero comenzaremos editando el archivo de configuracion manualmente. 

Comenzaremos por editar la Unidad ID (Unit ID) de la BTS y configurar la IP de la BSC.

<pre>
cd /etc/osmocom/
vi osmo-bts-virtual.cfg
</pre>

Editaremos el ip remota del oml _oml remote-ip_ para apuntar la IP del servidor en donde se ejecuta nuestra BSC, si está planeando ejecutar la BTS y la BSC en el mismo equipo, pueden dejarlo como localhost (127.0.0.1). 

Estableceré en _unit-id 4242_  cambiando _ipa unit-id 4242 0_

Finalmente cambiaremos la configuración de registro (logging) para mostrar todo, cambiando a: 

```
log stderr
 logging filter all 1
!
```

Así que esto es todo en terminos de configuración de nuestra BTS virtual a través de archivos de texto, guardamos el archivo e intentamos iniicar osmo-bts-virtual.

<pre> osmo-bts-virtual -c osmo-bts-virtual.cfg </pre>

Tendrás un resultado similar a este: 
<pre>
root@gsm-bts:/etc/osmocom# osmo-bts-virtual -c osmo-bts-virtual.cfg
 ((*))
   |
  / \ OsmoBTS
<0010> telnet_interface.c:104 Available via telnet 127.0.0.1 4241
<0012> input/ipaccess.c:901 enabling ipaccess BTS mode, OML connecting to 127.0.0.1:3002
<000d> abis.c:142 Signalling link down
<0001> bts.c:292 Shutting down BTS 0, Reason Abis close
Shutdown timer expired

root@gsm-bts:/etc/osmocom#
</pre>


Entonces, ¿Qué estamos viendo aquí? 

Bueno, Osmo-BTS-Virtual está  tratando de abrir su interfaz Abis pero no está obteniendo una conexión con la BSC (aún no hemos configurado una). Sin conexión a una BSC significa que la BTS no se activará ya que no tiene ningun procesamiento para sí misma, por lo que eventualmente se agotará y se apagará.


En la siguiente publicación nos moveremos de usar una osmo-bts virtual a usar un SDR para ejecutar Osmo-BTS. Si usarás un dispositivo RAN comercia, o solo simplemente jugando sin ningún RAN, pasa directamente a la publicación sobre la  Controladora de Estación Base (BSC), en donde vamos a retomar la adición de nuestra BTS Virtual a la BSC.

## Fuente Nickvsnetworking
[GSM with osmcom part 3 introduction to osmo software virtual bts](https://nickvsnetworking.com/gsm-with-osmocom-part-3-introduction-to-osmo-software-virtual-bts/)
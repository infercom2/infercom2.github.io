---
layout: single
title: GSM con Osmocom - Introducción Parte 4 - La Estación Base Controladora (BSC)
excerpt: "Configurando una Estación Base Controladora GSM con el stack de Osmocom"
date: 2022-05-25
classes: wide
header:
  teaser: /assets/images/osmocom/p4/BSC-Architecture.png
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

Configurando una Estación Base Controladora GSM con el stack de Osmocom

![](/assets/images/osmocom/p2/GSM-Architecture-1.png)

En nuestras publicacion anterior terminamos de configurar una Estación Base Transceptora (BTS) pero no sirve de nada a menos que pueda ubicar una Estación Base Controladora (BSC) 

Entonces, ¿Qué es lo que una BSC hace?

La BSC actua como una central controladora para una o más BTS.

En la practica esto significa que la BSC configura la mayoria de los parámetros de la BTS y los pone en funcionamiento cuando están listas.


La BSC monitorea las mediciones de los usuarios para determinar cuándo transferir de una BTS a  la BTS vecina.

La BSC también maneja la asignación de canales de radio y los recursos de radio a través de la BTS que administra.

En resumen, hace prácticamente todo lo relacionado con la radio de la BTS excepto transmitir y recibir datos a traves de la Interfaz Aerea (Um)

Además de administrar  y tener bajo su control a la BTS, el otro papel igualmente importante  de la BSC es proveer conectividad al resto de la red GSM, conectandose al **Centro de Conmutación Movil (MSC – Mobile Switching Center)** que maneja las llamadas hacia y desde nuestros suscriptores moviles y autenticarlos.

Al actuar como una especie de embudo, la MSC solo necesita una conexión con cada BSC en lugar de a cada BTS ( Lo que sería una gran cantidad de conexiones no tan practicas) 

## La instalación de Osmo-BSC

 Osmocom tiene su propia BSC - Acertadamente llamada Osmo-BSC                                                                                      

 La instalación es bastante sencilla, asegurate de tener el repositorio  de Osmocom en tu lista de fuentes (source list):

 ```
apt-get install osmo-bsc                                                                                                                             
systemctrl stop osmo-bsc
 ```


 Instalará estas dependencias:

 ```
libosmo-mgcp-client6 libosmo-sigtran5 libosmonetif8 osmo-bsc
 ```


Para dar servicio a las BTSs que controla, Osmo-BSC se basa en la conectividad al Centro de Conmutación Movil (MSC - Mobile Siwtching Center ), que a su vez se conecta a HLR (Home Location Register - Registro de ubicación de Origen). La BSC y la MSC se comunican por el protocolo SS7, y el enrutamiento se realiza mediante un Punto de Transferencia de Señal (STP - Signal Transfer Protocol)

Analizaremos cada uno de estos elementos con mayor detalle, pero para que nuestra BSC funcione de manera util, necesitaremos instalar e iniciar estas aplicaciones

Hablaremos sobre la MSC y los conceptos básicos de  SS7 , HLR y Sigtran más adelante, por ahora instalaremos e iniciaremos a ciegas.


```
apt-get install osmo-stp osmo-msc osmo-hlr
systemctl start osmo-stp
systemctl start osmo-msc
systemctl start osmo-hlr
```

Regresaremos y cubriremos estos elementos con mayor detalle en su debido tiempo.


## Configuración Osmo – Terminal interactiva vía Telnet

Ahora que tenemos la BSC instalada, hemos apuntado nuestra BTS a la IP de la BSC, necesitaremos ejecutar osmo-bsc y agregar la configuración para nuestra nueva BTS. 

En lugar de trabajar con el archivo de texto , iniciaremos el servicio y trabajar a través de Telnet, como lo hariamos con muchos dispositivos de red comunes

Osmo-BSC escucha en el puerto 4242, por lo que iniciaremos Osmo-BSC y conectarnos a través de Telnet:

```
systemctl start osmo-bsc
telnet localhost 4242
```

La interfaz deberia ser bastante natural para cualquiera que haya pasado mucho tiempo configurando otros dispositivos, muchos de los comandos son similares y si, ¡funciona el autocompletar palabras con TAB!

Comenzaremos habilitando el registro (logging) para que podamos tener una idea de lo que está sucediendo

```
OsmoBSC> enable
OsmoBSC# logging enable
OsmoBSC# logging filter all 1
OsmoBSC# logging color 1
```

A continuación en una nueva sesion de terminal SSH, ejecutamos nuestra BTS virtual nuevamente;      

<pre> osmo-bts-virtual -c osmo-bts-virtual.cfg </pre>

En esta ocasión tendremos una salida diferente desde la BTS cuando intentamos iniciarlo: 

<pre>
  root@gsm-bts:/etc/osmocom# osmo-bts-virtual -c osmo-bts-virtual.cfg
 ((*))
   |
  / \ OsmoBTS
<0010> telnet_interface.c:104 Available via telnet 127.0.0.1 4241
<0012> input/ipaccess.c:901 enabling ipaccess BTS mode, OML connecting to 127.0.0.1:3002
<0012> input/ipa.c:128 127.0.0.1:3002 connection done
<0012> input/ipaccess.c:724 received ID_GET for unit ID 4242/0/0
<0012> input/ipa.c:63 127.0.0.1:3002 lost connection with server
<000d> abis.c:142 Signalling link down
<000d> abis.c:156 OML link was closed early within 0 seconds. If this situation persists, please check your BTS and BSC configuration files for errors. A common error is a mismatch between unit_id configuration parameters of BTS and BSC.

root@gsm-bts:/etc/osmocom#

</pre>

También veremos errores en la terminal de la BSC: 

![](/assets/images/osmocom/p4/Abis-Over-IP-connection-failing-due-to-no-BTS-Config.png)

<pre>

<0016> input/ipa.c:287 0.0.0.0:3002 accept()ed new link from 10.0.1.252:39383
<0016> bts_ipaccess_nanobts.c:480 Unable to find BTS configuration for 4242/0/0, disconnecting
</pre>

### Entonces, ¿Qué está pasando aquí?

Bien , nuestra BTS está tratando de conectarse a nuestra BSC, y esta vez puede hacerlo, pero nuestra BSC no tiene ninguna configuracion para esta BTS, entonce la BSC está rechazando la conexión

Así que ahora tenemos que configurar la BSC para que reconozca nuestra BTS

## Configurando/Abasteciendo (provisioning) una nueva BTS en la BSC

Este tutorial va a ser lo suficientemente generico para que cualquiera pueda segurilo, primero vamos a configurar la BTS virtual en nuestra BSC para empezar, escribí sobre la instalación de OsmoBTS-Virtual en este post

Podemos obtener información sobre el intento de conexion de la BTS rechazada  desde la terminal de la BSC:

<pre>
OsmoBSC# show rejected-bts
 Date                Site ID BTS ID IP
2020-03-29 01:32:37    4242      0      10.0.1.252 
</pre>

Conocemos el ID del Sitio Site-ID que es 42424 (lo configuramos previamente) y el ID de la BTS para ese sitio es 0, así vamos a crear una BTS en la BSC

<pre>
OsmoBSC> enable
OsmoBSC# configure terminal
OsmoBSC(config)# network
OsmoBSC(config-net)# bts 1
OsmoBSC(config-net-bts)# type sysmobts
OsmoBSC(config-net-bts)# description "Virtual BTS"
OsmoBSC(config-net-bts)# ipa unit-id 4242 0
OsmoBSC(config-net-bts)# band DCS1800 
OsmoBSC(config-net-bts)# codec-support fr hr efr amr
OsmoBSC(config-net-bts)# cell_identity 4242
OsmoBSC(config-net-bts)# location_area_code 4242
OsmoBSC(config-net-bts)# base_station_id_code 4242
OsmoBSC(config-net-bts)# base_station_id_code 42
OsmoBSC(config-net-bts)# ms max power 40
OsmoBSC(config-net-bts)# trx 0
OsmoBSC(config-net-bts-trx)# max_power_red 20
OsmoBSC(config-net-bts-trx)# arfcn 875
OsmoBSC(config-net-bts-trx)# timeslot 0
OsmoBSC(config-net-bts-trx-ts)# phys_chan_config CCCH+SDCCH4
OsmoBSC(config-net-bts-trx-ts)# exit
OsmoBSC(config-net-bts-trx)# timeslot 1
OsmoBSC(config-net-bts-trx-ts)# phys_chan_config TCH/F
OsmoBSC(config-net-bts-trx-ts)# exit
OsmoBSC(config-net-bts-trx)# timeslot 2
OsmoBSC(config-net-bts-trx-ts)# phys_chan_config TCH/F
OsmoBSC(config-net-bts-trx-ts)# exit
OsmoBSC(config-net-bts-trx)# timeslot 3
OsmoBSC(config-net-bts-trx-ts)# phys_chan_config TCH/F
OsmoBSC(config-net-bts-trx-ts)# exit
OsmoBSC(config-net-bts-trx)# timeslot 4
OsmoBSC(config-net-bts-trx-ts)# phys_chan_config TCH/F
OsmoBSC(config-net-bts-trx-ts)# exit
OsmoBSC(config-net-bts-trx)# timeslot 5
OsmoBSC(config-net-bts-trx-ts)# phys_chan_config TCH/F
OsmoBSC(config-net-bts-trx-ts)# exit
OsmoBSC(config-net-bts-trx)# timeslot 6
OsmoBSC(config-net-bts-trx-ts)# phys_chan_config TCH/F
OsmoBSC(config-net-bts-trx-ts)# exit
OsmoBSC(config-net-bts-trx)# timeslot 7
OsmoBSC(config-net-bts-trx-ts)# phys_chan_config TCH/F
OsmoBSC(config-net-bts-trx-ts)# exit 
OsmoBSC(config-net-bts-trx)# exit
OsmoBSC(config-net-bts)# exit
OsmoBSC(config-net)# exit
OsmoBSC(config)# exit 
OsmoBSC# copy running-config startup-config
</pre>


¡Peeew!

### ¿Qué hicimos realmente aquí?

Bien, como estamos obteniendo la mayoria del procesamiento de la BTS desde la BSC, tenemos que decirle a la BSC todo sobre como queremos la configuración de la BTS ( Lo creas o no esta es la configuración más simplificada que pude hacer)


El tipo de Unidad ID IPa, banda (band) y Identidad de Celda (Cell Identity) constituyen algunos parametros que necesitamos para identificar la BTS (Unidad ID IPA - IPA Unid ID) y darle sus parametros básicos de identidad.

Después en la sección trx0  configurarmos el contenido de los 8 timeslots de GSM. Nuestro primer slot lo configuramos como CCCH+SDCCH4 lo que significa que el primer timeslot contendrá un canal de control común (Common Control Channel) y 4 Canales de Control Dedicados independientes, utilizados para la señalización, mientras que los 7 timeslot sobrantes se utilizarán como canales de tráfico (Traffic Channel) para voz de velocidad completa (Full-Rate) (TCH/F) 

Es importante que lo que le configuremos a la BSC de las caracteristicas/capacidades de la BTS coincidan con las caracteristicas/capacidades reales de la BTS. Por ejemplo, no tiene sentido configurar el soporte de GRPS o EDGE en la BSC si la BTS no lo soporta.

Si tienes habilitado el logging (registros) cuando la BTS se conecta a la BSC, verás errores que enumeran las funciones que no coinciden entre los dos dispositivos.

Como puedes imaginar,hay mejores opciones que esta para agregar BTSs de forma masiva - La interfaz de Control de Osmocom (Osmocom Control Inferfaz) expone estas funciones en una forma similar a una API, pero antes de iniciar con la preparación de la red es bueno saber lo esencial.

## Conectandonos la BTS a la BSC 

¡Sigamos adelante! Conectemos nuestra BTS a la BSC.
Si has cerrado la terminal de la BSC en donde habilitamso el logging(registro), necesitarás habilitarla de nuevo:

```
OsmoBSC> logging enable
OsmoBSC> logging filter all 1
OsmoBSC> logging color 1
```

Y  a continuación, trataremos de iniciar la BTS de nuevo:

<pre>
root@gsm-bts:/etc/osmocom# osmo-bts-virtual -c osmo-bts-virtual.cfg
 ((*))
   |
  / \ OsmoBTS
<0010> telnet_interface.c:104 Available via telnet 127.0.0.1 4241
<0012> input/ipaccess.c:901 enabling ipaccess BTS mode, OML connecting to 127.0.0.1:3002
<0012> input/ipa.c:128 127.0.0.1:3002 connection done
<0012> input/ipaccess.c:724 received ID_GET for unit ID 4242/0/0
  </pre>

 Y en la BSC verás más o menos lo mismo: 

 <pre>
OsmoBSC# 
<0016> input/ipa.c:287 0.0.0.0:3002 accept()ed new link from 127.0.0.1:40193
<0004> abis_nm.c:490 BTS1 reported variant: omso-bts-virtual
<0004> abis_nm.c:578 OC=BASEBAND-TRANSCEIVER(04) INST=(00,00,ff): BTS1: ARI reported sw[0/1]: TRX_PHY_VERSION is Unknown
<0016> input/ipa.c:287 0.0.0.0:3003 accept()ed new link from 127.0.0.1:44053
<0003> osmo_bsc_main.c:291 bootstrapping RSL for BTS/TRX (1/0) on ARFCN 0 using MCC-MNC 001-01 LAC=4242 CID=4242 BSIC=42 
</pre>

Si has llegado hasta aquí, felicitaciones!. Nuestra BTS Virtual ahora está conectada a nuestra BSC - ¡Si no fuera virtual, estariamos al aire!

## Traducción de Nickvsnetworking

[GSM con osmocom part4 the base station controller bsc](https://nickvsnetworking.com/gsm-with-osmocom-part-4-the-base-station-controller-bsc/)

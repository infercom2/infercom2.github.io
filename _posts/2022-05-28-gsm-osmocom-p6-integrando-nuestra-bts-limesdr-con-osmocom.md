---
layout: single
title: GSM con Osmocom - Introducción Parte 6 - Integrando nuestra BTS LimeSDR con OsmoBSC 
excerpt: "Conectando nuestro LimeSDR basada en una BTS GSM con OsmoBSC"
date: 2022-05-28
classes: wide
header:
  teaser: /assets/images/osmocom/p6/GSM-Architecture-1.png
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

Conectando nuestro LimeSDR basada en una BTS GSM con OsmoBSC


![](/assets/images/osmocom/p6/GSM-Architecture-1.png)

En la publicación anterior cubrimos la configuración requerida para aprovisionar una nueva BTS en nuestra BSC

Vamos a hacer más o menos lo mismo esta vez, ya que conectamos nuestra BTS basada en SDR a  nuestra BSC. 

Antes de activar el lado BTS de las cosas, vamos a asegurarnos de haber detenido la BTS Virtual y deshabilitarlo. 

```
systemctl stop osmo-bts-virtual
systemctl disable osmo-bts-virtual
```

## Configurar Osmo-BTS-TRX

A continuación, editaremos la configuración de osmo-bts-trx 

```vi /etc/osmocom/osmo-bts-trx.cfg```

Editaremos el  oml remote-ip a la IP del servidor que esta ejecutando tu BSC, si estas ejecutandola en el mismo equipo puedes dejarla como localhost (127.0.0.1).

Ahora, Configuraremos la Unidad-ID (Unit-ID) de la BTS, esto identifica la BTS dentro de la BSC.

Voy a configurar para unit-id 1234 cambiando ipa unit-id 1234 0 
Finalmente cambiaremos la configuración de registro (logging) para que muestre todo cambiandola a: 

```
log stderr
 logging filter all 1
!
```

Después, configuramos la BTS en la BSC.

## Suministros de la BSC

Esto es esencialmente una copia del resto de aprovisionamiento que seguimos en la publicación anterior, la única diferencia es que usaremos BTS 2( ya que la BTS 1 está configurada para nuestra BTS Virtual) en la configuración, y estableceremos los pocos identificadores diferentes como el ID de la unidad ipa (ipa unit id ) para la BTS basada en SDR. 


```
telnet localhost 4242
OsmoBSC> enable
OsmoBSC# configure terminal
OsmoBSC(config)# network
OsmoBSC(config-net)# bts 2
OsmoBSC(config-net-bts)# type sysmobts
OsmoBSC(config-net-bts)# description "LimeSDR Based BTS"
OsmoBSC(config-net-bts)# ipa unit-id 1234 0
OsmoBSC(config-net-bts)# band DCS1800
OsmoBSC(config-net-bts)# codec-support fr hr efr amr
OsmoBSC(config-net-bts)# cell_identity 1234
OsmoBSC(config-net-bts)# location_area_code 1234
OsmoBSC(config-net-bts)# base_station_id_code 1234
OsmoBSC(config-net-bts)# base_station_id_code 12
OsmoBSC(config-net-bts)# ms max power 40
OsmoBSC(config-net-bts)# trx 0
OsmoBSC(config-net-bts-trx)# max_power_red 20
OsmoBSC(config-net-bts-trx)# arfcn 876
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
```

## Iniciar la BTS basada en SDR

Antes de iniciar la BTS basada en SDR,  probablemente sea  mejor tener 3 terminales abiertos.

Una logueada en Osmo-BSC  con los registros (logging) habilitados (ver la publicación anterior para información de como hacer eso) 

Iniciaremos otra terminal para correr el modem TRX con la interfaz de Capa 1:

```osmo-trx-lms -C /etc/osmocom/osmo-trx-lms.cfg```

Y en otra terminal nueva vamos a iniciar el lado de la BTS: 

```osmo-bts-trx -c /etc/osmocom/osmo-bts-trx.cfg```

Todo va bien  en nuestra termianl con Osmo-BSC debería  de informar la conexión:

<pre>
OsmoBSC#
<0016> input/ipa.c:287 0.0.0.0:3003 accept()ed new link from 10.0.1.252:39595
<0003> osmo_bsc_main.c:291 bootstrapping RSL for BTS/TRX (2/0) on ARFCN 875 using MCC-MNC 001-01 LAC=1234 CID=1234 BSIC=12
</pre>

Y las ventanas osmo-trx-lms y osmo-bts-trx deberian de tener datos pasando a una gran velocidad. 

## Verificando del funcionamiento de la Celda

Si todo va según lo planeado, nuestra SDR esta conectada al equipo a través de osmo-trx-lms el cual está actuando como un modem para osmo-bts-trx el cual está conectado ahora a la BSC. Hay mucho que recorrer, pero a partir de aquí se vuelve más fácil.

Analicemos las redes de nuestro teléfono. Descubrí que poner el mío en GSM solo antes de buscar redes significaba que aparecía mucho más rápido. 

![](/assets/images/osmocom/p6/Screenshot_2020-03-29-19-45-51.png)
## Fuente Nickvsnetworking

[GSM con osmocom part6 Integrating our LimeSDR BTS with OsmoBSC](https://nickvsnetworking.com/gsm-with-osmocom-part-6-integrating-our-limesdr-bts-with-osmobsc/)

---
layout: single
title: GSM con Osmocom - Introducción Parte 9 - ¡Llamadas y SMS al Fin!
excerpt: " Llamadas y SMS "
date: 2022-05-31
classes: wide
header:
  teaser: /assets/images/osmocom/p9/Noka-GSM-phones-on-Osmocom.png
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

![](/assets/images/osmocom/p9/Noka-GSM-phones-on-Osmocom.png)


Así que ahora hemos cubierto los conceptos básicos de lo que implica, obtengamos algo de tráfico en nuestra red.
Para empezar, necesitaremos iniciar cada uno de los elementos de nuestra red y encender el dispositivo BTS que estemos utilizando. 

Para que nuestras llamadas tengan audio, tendremos que configurar un parametro en la Puerta de Enlace de Comunicación (Media Gateway). Cubriremos con más detalle la Puerta de Enlace de Medios de Comunicación (Media Gateway) en el futuro, pero hay un valor en el MGW que necesitaremos configurar para que las llamadas funcionen, y ese es el valor **rtp bind-ip**.

Puedes  configurarlo en el archivo de configuración o través de VTY/Telnet en el puerto 4243.

Hemos hablado de usar systemctl para iniciar todos los servicios, pero hay un script en el directorio _/etc/osmocom/_  llamado _osmocom-all.sh_   el cual inicia todos los elementos de la red por nosotros. 

Una vez se estén ejecutando todos los servicios, sugiero que ingrese a OsmoBSC y habilite todos los registros (logging) que pueda, luego conecte/ encienda su BTS.
Debería de ver aparecer la conexión Abis sobre IP y el enlace OML cuando la BTS se conecte a la BSC.

Y luego, contenga la respiración, encienda un teléfono y busque redes.

Si todo va bien, verá OsmoMSC en la busqueda de red, selecciónelo y debería de ver los datos de registro apareciendo rápidamente mientras el teléfono (“terminal”) se conecta a la red.

Suponiendo que configuró el IMSI del SIM en el HLR, debería de estar conectado a la red y mostrar las barras de recepción en el teléfono. 

Puedes revisar el número telefónico (MSISDN) marcando el codigo USSD  *#**100**# 
Pero no es una red con un sólo teléfono conectado, conecte un segundo teléfono, revise su número telefónico de la misma forma y realize una llamada entre ambos. 
Los SMS también deberían de funcionar. 


>**Y ahí lo tienes, una Red GSM funcional!**

Pero este no es el final para nosotros,  en realidad es solo el comienzo.

Todavia hay mucho más por aprender y trabajar – Durante las proximas semanas / meses agregaremos paquetes de datos a la red con GPRS o EDGE, nos conectaremos a interfaces de enrutamiento de llamadas externas y de enturamientos de SMS, usaremos Circuit Switched Fallback para brindar servicio de voz a los usuarios de las redes LTE y moverse entre ellas. 

## Fuente Nickvsnetworking

[GSM with osmocom part9 Calls & SMS at last! ](https://nickvsnetworking.com/gsm-with-osmocom-part-9-calls-sms-at-last/)

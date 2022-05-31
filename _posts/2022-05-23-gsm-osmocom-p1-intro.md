---
layout: single
title: GSM con Osmocom - Introducción Parte 1
excerpt: "Introducción a una serie de publicaciones sobre GSM usando el stack de Osmocom"
date: 2022-05-23
classes: wide
header:
  teaser: /assets/images/osmocom/p1/Osmo.png
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
![](/assets/images/osmocom/p1/Osmo.png)

Introducción a una serie de publicaciones sobre GSM usando el stack de Osmocom

## Meta

Como la mayoria de gente en este momento debido al confinamiento, tengo un poco de tiempo más de lo normal.

Debido a esto, pensé que finalmente me sumergiría en GSM/UMTS y toda esa tecnologia de interconexión de circuitos que se salta antes de iniciar con LTE.

Disculpa el tono cariñoso que utilizo al describir una tecnológia muy antigua, es el resultado de ser una tragedia telefonica que recuerda los primeros telefonos con los que interactuaron y preguntarse como funcionaba todo.

## Entonces, ¿Por que estudiar GSM?

Mi mejor amigo es un traductor de documentos tecnicos. En la Universidad, cual fué el primer idioma que ellos estudian? El latin, por que es la raíz de muchos idiomas.

Mientras muchos operadores ya han apagado sus redes GSM (No hay redes públicas en Australia),  el nucleo de GSM se comparte esencialmente con el de UMTS/3G, que seguirá existiendo en un futuro previsible.

Circuit Siwtche Fallback (CSFB) es todavia comun hoy en día para las llamadas de voz para una gran cantidad de telefonos LTE sin soporte VoLTE. GSM soporta GSM-R, el estandar GSM específico para rieles que se utilizan en toda Europa. La potencia de uplink (enlace ascendente) de GSM puede ser de hasta 8 Watts (mientras que en LTE es de 20dBm – 0.1W) lo que significa que el area de servicio eficaz podría ser mayor que las interfaces aereas de 3G y 4G.

GSM no está tan muerto como podría parecer, ¡Así que tengamos una semana en Bernie’s!

Exención de responsabilidad : Avíseme si tengo algún problema o he entendido mal cualquier cosa. Estas publicaciones se centrarán en los elementos de red Osmocom que manejan algunas cosas de forma “no estandar”

## Plataformas

Escribí la semana pasada sobre el uso de YateBTS para obtener una red en linea de GSM/GPRS funcional 

Es grandioso que funcione desde la perspectiva de la interfaz Um, tu terminal/UE puede ver y conectarse a la red. Pero es una especie de solución todo en uno, no hay centro de conmutación Movil (Mobil Switching Center), Estación Base Controladora (Base Station Controller), Sigtran, HLR o Medios de Puerta de enlace; Yatee uno todo esto en un solo paquete fácil de usar.

Esto lo pone al aire en poco tiempo, pero desafortunadamente no se expone como funciona GSM/ UMTS en el mundo real – Las redes reales no se ven como YateeBTS NITPC

## Entrar en Osmocom

Osmocom ( y el equipo de Sysmocom que dirigen muchos de los proyectos ) han hecho un trabajo fenomenal al construir  cada uno de los elementos de red de los que acabo de hablar practicamente  “según el libro”, lo que significa/ dice que la mayoria deberia de interoperar con equipos comerciales y cumplir con los estandares. 

Durante las  siguientes semanas cubriré la configuración de cada uno de los elementos de la red, hablaré sobre que es lo que hacen y como funcionan, y lo usaré para crear una red funcional GSM (2G/Circuito de conmutación) usando el software de Osmocom.

Una vez tengamos nuestra red funcional, le agregaremos SMS , Datos (GPRS/EDGE), codigos USSD e incluso inter-RAT traspaso/handover con LTE.

Para esta serie de publicaciones, usaré una combinación de dispositivos. Para iniciar usaré una Radio Definida por Software – Software Defined Radio – (LimeSDR) para hacer el lado de  Radio Frecuencia  (RF)de la red (BTS). 
Osmocom es compatible con una gran cantidad de dispositivos SDR comunes, por lo que es de espera que cualquiera que desee seguirlo en casa que tenga acceso a un LimeSDR o USRP.

Osmocom es compatible con muchos productos comerciales  de proveedores de BTS, así como dispositivos de Sysmocom , dispositivos de Range Network, Osmocom ademas es compatible con la gama NanoBTS de ip.access – que está disponible  de segunda mano a un precio bastante bajo, que también agregaré a nuestra red.

>"Así que compre algunas tarjetas SIM programables baratas, toma tu hardware SDR u ordena en linea una BTS GSM barata  de segunda mano, ¡y comencemos a contruir una red GSM!"

## Temas

Actualizaré los enlaces aquí a medida que haga las publicaciones, es posible que lo olvide, así que si falta algo, busque en el sitio o envieme un comentario.

+ [Mis archivos de configuración de Osmocom en Github](https://github.com/nickvsnetworking/osmocom-config/)

## Red de acceso por radio(Inglés)

+ [Fundamentos de la BTS](https://nickvsnetworking.com/gsm-with-osmocom-part-2-bts-basics/)
+ [Practica de BTS con un LimeSDR y osmo-bts-trx ](https://nickvsnetworking.com/gsm-with-osmocom-part-3-bts-in-practice-with-limesdr-osmo-bts-trx/)
+ [La Estación Base Controladora (BSC – Base Station Controller) ](https://nickvsnetworking.com/gsm-with-osmocom-part-4-the-base-station-controller-bsc/)
+ [Integrando nuestra BTS LimeSDR con OsmoBSC ](https://nickvsnetworking.com/gsm-with-osmocom-part-6-integrating-our-limesdr-bts-with-osmobsc/)
+ [Integrando una NanoBTS ipaccess con OsmoBSC ](https://nickvsnetworking.com/gsm-with-osmocom-nanobts/)
+ [Tipos de Canales ](https://nickvsnetworking.com/gsm-with-osmocom-channel-types/)

## Conmutación y Señalización
+ [Registro de Ubicación Inicial (HLR – Home Location Register) (con EIR y AuC)](https://nickvsnetworking.com/gsm-with-osmocom-part-7-the-hlr-home-location-register-and-friends/)
+ [La Señalización de Punto de Transferencia (Signaling Transfer Point)  SS7](https://nickvsnetworking.com/gsm-with-osmocom-part-8-ss7-sigtran/)
+ [El Centro de Conmutación Móvil (MSC) ](https://nickvsnetworking.com/gsm-with-osmocom-part-7-the-mobile-switching-center/)
+ [Conceptos básicos de llamadas y SMS](https://nickvsnetworking.com/gsm-with-osmocom-part-9-calls-sms-at-last/)
+ [Enrutamiento de llamadas](https://nickvsnetworking.com/gsm-with-osmocom-call-routing-in-gsm/)

## Traducción de Nickvsnetworking
[GSM with osmocom part 1 - Intro ](https://nickvsnetworking.com/gsm-with-osmocom-part-1-intro/) 


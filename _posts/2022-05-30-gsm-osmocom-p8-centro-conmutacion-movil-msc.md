---
layout: single
title: GSM con Osmocom - Introducción Parte 8 - El Centro de Conmutación Móvil (MSC-The Mobil Switching Center) 
excerpt: "Configurando el Centro de Conmutación Móvil para una Red GSM utilizando OsmoMSC"
date: 2022-05-30
classes: wide
header:
  teaser: /assets/images/osmocom/p8/MSC-Architecture.png
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

Configurando el Centro de Conmutación Móvil para una Red GSM utilizando OsmoMSC

## Las funciones del Centro de Conmutación Móvil (MSC – Mobile Switching Center)

![](/assets/images/osmocom/p8/MSC-Architecture.png)

Nos estamos acercando al final de nuestra historia de “configuración” - Hasta ahora hemos cubierto la red de acceso (BTS y BSC) y nuestra base de datos de suscriptores (HLR) , así que ahora hablaremos sobre una de los elementos claves “El nucleo (core) ” de nuestra red – El centro de conmutación móvil (MSC – Mobile Switching Center). 

El nombre de MSC lo dice todo, es un Centro de Conmutación para Móviles.

>La MSC hace la conmutación de llamadas de voz y los mensajes de texto /SMS  entre los suscriptores  y redes locales  así como las remotas. 

## Función de Conmutación

Debido a que GSM fue diseñado para estar centrado en la voz ( Tenga en cuenta que la primera red GSM se puso en marcha en 1991) la función primaria del MSC es cambiar las llamadas telefónicas entre suscriptores. * Para ello, el MSC debe de realizar un seguimiento de los suscriptores a los que atiende actualmente, sus capacidades y como llega a ellos – que BSC les atiende y por lo tanto que BTS le atiende. 

El OsmoMSC también cuenta con un **SMSC** (Servidor de Servicio de Mensajes Cortos – Short Message Service Server ) minimalista para el enrutar el tráfico de SMS entre suscriptores de la red. 

Este SMSC básico actúa de forma de almacenamiento y retransmisión. Las redes de producción normalmente usarían un SMSC externo para manejar SMS, OsmoMSC tiene la funcionalidad SMSC incorporada de forma predeterminada, pero las interfaces estan ahí si desea usar un SMSC externo. 

Todas las llamadas/ mensajes de textos a suscriptores/ destinos fuera del MSC (por ejemplo una llamada al telefóno de un suscriptor de operador diferente  o en  la PSTN – Public Switched Telephone Network, Red Pública Telefónica Conmutada- ) son tipicamente enrutadas a otra **MSC conocida como la puerta de enlace (Gateway ) MSC.**

La GMSC maneja la interconexion con tras redes.  Hablaremos de esto más adelante con el conector SIP (SIP Connector), pero por ahora nos enfocaremos solo en las llamadas entre suscriptores de una red.

Vale la pena señalar que en el MSC no se  encuentra el flujo de medios /comunicación (media sream), simplemente configura y elimina las llamadas. Pronto cubriremos más sobre el meollo de las llamadas en GSM.

## Función de Registro de Ubicación para Visitantes

El MSC también actua como interfaz del HLR para AAA, como cubrimos en nuestra ultima  publicación , el HLR proporciona la función de autenticación y también proporciona los datos del suscriptor al MSC. Los datos del suscriptor son copiados del HLR al cache del HLR interno en la MSC conocido como **Registro de Ubicación para Visitantes (VLR – Visitor Location Register )** después de que el suscriptor se conectó. 

## Consultas de Autenticación, Cifrado y EIR

En la última publicación hablamos sobre la función del HLR en términos de Autenticación en la red,  los vectores de autenticación, pero las políticas que hacen cumplir estas se establecen en el MSC. 
El MSC consulta la información de control de admisión del HLR, pero es el MSC el responsable de hacer cumplir estas reglas.

## Identidad del Nucleo de la Red (Core Network Identity)

El MCC (Código del País para Móvil  – Mobile Country Code) y el MNC (Codigo de la Red Móvil- Mobile Network Code) de la red (Juntos el MCC + MNC se denominan [ID PLMN](https://nickvsnetworking.com/plmn-identifier-calculation-mcc-mnc-to-plmn/), junto con el nombre de la red, se configuran en el MSC. 

Si bien esto puede parecer un detalle bastante pequeño, el ID de PLMN es analogo del SSID en una red WiFi – Es lo que identifica tu red de todas las demás activas, y el nombre de la red se muestra en tu telefono cuando estás conectado mostrando el nombre de la red.

## Configuración y Conexiones

La BSC que configuramos previamente se comunica con el MSC a través de Codigos de Punto SS7  ( SS7 Point Codes). Veremos como los códigos de puntos enrutan solicitudes en una publicación posterior, pero mientras esté ejecutandose Osmo-BSC, Osmo-MGW, Osmo-MSC y Osmo-HLR en el mismo equipo, no necesitarás vincularlos entre ellos, como tuvimos que hacer al agregar nuestra BTS a la BSC. 

En su lugar, sólo tendremos que iniciar todos los programas necesarios : 
```
systemctl restart osmo-stp
systemctl restart osmo-hlr
systemctl restart osmo-mgw
systemctl restart osmo-msc
```


La conexión GSUP entre el MSC y el HLR será establecida al inicio, pero la BSC solo  establecerá la conexión con el MSC cuando ellos necesiten algo del MSC. 

Una vez  que tengamos todo ejecutandose, podemos conectarnos a por Telnet al MSC para configrar que está ejecutandose y revisar su estado: 

<pre> root@gsm # telnet localhost 4254 </pre> 

Suponiendo que lograste conectarte, ese es otro elemento de la red en línea. - Dejaremos los códigos de punto (Point Codes)  predeterminados en su lugar para que la BSC pueda conectarse al MSC, pero tengamos en cuenta que la BSC solo  establecerá una conexión cuando este necesite algo del MSC. 

## Para Terminar

Hay algunos temas que omitimos en este tema, cosas como SS7/ SIGTRAN, como se enrutan las llamadas GSM del mundo real usando MNCC-SAP, la Puerta de Enlace de Media/Comunicación (Media Gateway) y el anclaje de transmisión de medios (media streams) y que hace un SMSC.

Haré publicaciones que cubran cada uno de estos temas a mayor profundidad.


## Fuente Nickvsnetworking

[GSM con osmocom part8 The Mobile Switching Center ](https://nickvsnetworking.com/gsm-with-osmocom-part-7-the-mobile-switching-center/)

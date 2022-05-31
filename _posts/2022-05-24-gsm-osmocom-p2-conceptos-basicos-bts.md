---
layout: single
title: GSM con Osmocom - Introducción Parte 2 - Conceptos Básicos de la BTS
excerpt: "¿Qué es una BTS y dónde encaja en la arquitectura de red GSM?"
date: 2022-05-24
classes: wide
header:
  teaser: /assets/images/osmocom/p2/gsm-base-station.jpg
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

¿Qué es una BTS y dónde encaja en la arquitectura de red GSM?

![](/assets/images/osmocom/p2/gsm-base-station.jpg)

Por mucho, la parte más visible de cualquier red móvil ( además de tu teléfono) son las estaciones base (Transceptoras  - BTS Base Transciver Station)

Distribuidas por la ciudad, en mástiles, torres y monopolos , ya sea que los notes o no, las estaciones base (BTS) están por todas partes.

## Arquitectura 

El lado de RF (Radio Frecuencia) de LTE tiene un eNodeB, el cual es un dispositivo inteligente. - Lo conectas a una red TCP/IP, establece una conexión con tu MME(s) y listo.



Una **BTS GSM (Estación Base Transceptora )** no es del todo inteligente… 


>La BTS es similar a los puntos de acceso WiFi se comunican a un controlador centralizado para todos sus procesamientos.
Una BTS obtinee la mayor parte de sus “cerebros” de otros lugares y esencialmente solo maneja los datos de TX / RX  de la Banda base.

Eso que se encuentra en otros lugares es la **BSC - Estación Base Controladora (Base Station Controller)** . Cada BTS se conecta a una BSC, y una BSC normalmente controla una serie de BTS.

Exploraremos la BSC y sus conexiones a profundidad, pero abajo he elaborado un diagrama básico de cómo encaja todo.

![](/assets/images/osmocom/p2/GSM-Architecture-1.png)

## Interfaz Um

La interfaz Um es la interfaz aerea de GSM. La cual toma los datos y los envía “por aire”

Hay mucho que saber sobre las interfaces aereas  y yo sé muy poco. Lo que si sé es que necesito configurar la interfaz Um para una banda de frecuencia para que admita mi móvil (para poder ver y conectarme a la red).

## Interfaz Abis

El hecho de que GSM se implementó por primera vez en 1991 explica por qué la interfaz Abis utilizó enlaces ISDN E1/T1 TDM para conectar  las Estaciones Base Transceptoras (BTSs) a las Estacion Base Controladora (BSC). 

Si miramos hacía atrás , podemos preguntarnos por qué no se utilizó TCP/IP para la interfazz Abis, tengamos en cuenta que Windows 95 fué la primera versión que incluyó compatibilidad con TCP/IP , y eso nos da una idea de como era la situación. ISDN es muy confiable y era muy conocido en el mundo de las telecomunicaciones en ese momento.

Ya no tengo ningun dispositivo ISDN, por lo que todo lo construiremos utilizando redes de conmutación que funcionan como circuitos de conmutación. 

Osmocom tiene soporte para interfaces E1/T1, así que si tienes dispositivos BTS que se comunica unicamente a través de enlaces TDM, esa también es una opción.

GSMA nunca escribió un estándar para llevar Abis sobre IP, por lo que cada proveedor lo han implementado de manera diferente. 

Osmocom tiene una versión del protocolo Abis sobre IP que han desarrollado basándose en los rastros de una implementación comercial la cual usaremos.

[Puedes encontrar las especificaciones completas para el Abis de Osmocom sobre interfaz IP aquí](https://ftp.osmocom.org/docs/latest/osmobts-abis.pdf)

## Interfaz OML

Con todos los  cerebros/ procesamientos de la BTS residiendo en la BSC, existe la necesidad de controlar la BTS desde la BSC. El enlace de operación y mantenimiento (OML – Operation and Maintanance Link ) es un protocolo para cambiar ciertos parametros de la BTS desde la BSC.

Un excelente ejemplo de uso del OML sería la BSC que apague y encienda la BTS. 

Veremos una pequeña parte del uso de OML en la siguiente publicación, solo para apagar y prender la BTS. 


Así que vamos a poner esto en práctica y configurar una BTS Virtual con Osmocom.

## Traducción de Nickvsnetworking

[GSM with osmcom part 2 BTS Basics](https://nickvsnetworking.com/gsm-with-osmocom-part-2-bts-basics/)
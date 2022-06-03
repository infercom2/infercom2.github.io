---
layout: single
title: GSM con Osmocom - Introducción Parte 7 - el HLR (Home Location Register/ Registro de Ubicación Base) 
excerpt: "Que es lo que el Registro de Ubicación Base (HLR -Home Location Register) hace en GSM, como configurarlo y configurar subscriptores. "
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

Que es lo que el Registro de Ubicación Base (HLR -Home Location Register) hace en GSM, como configurarlo y configurar subscriptores. 


![](/assets/images/osmocom/p7/OsmoHLR-Update-Location-Request.png)


## Autenticación

Una necesidad obvia es autenticar a nuestros subscriptores para que la red pueda verificar su identidad.

El IMSI ( Identidad Internacional de subscriptor móvil – International Mobile Subscriber Identify ) se utiliza para identificar al usuario de todos los demás subscriptores móviles del mundo. El IMSI está expuesta al usuario, pero la transmisión del IMSI   en condiciones claras suele ser algo que se evita siempre que se posible en la interfaz áerea (Air Interface). 

GSM utiliza un único secreto compartido enre la SIM y la red (la clave -key- K) para la autenticación. Este secreto compartido no está expuesto al usuario y nunca trasmitido en el aire. 

Cuando un usuario quiere autenticarse, el HSS de la red toma una clave aleatoria (RAND -Random Key) y las combina con la clave secreta (K) para generar una Respuesta Firmada (Signed Response) llamada SRES. La eed envia la clave RAND a la persona subscriptora, y su SIM toma la clave secreta (K) y la combina con el valor RAND de la red, antes de enviar su respuesta firmada (SRES) de vuelta a la red. 

Si el SRES enviado por la persona subscriptora coincide con la SRES generada por el HSS, entonces la persona usuaria es autenticada. El conjunto de claves que se utilizan para una sesión de autenticación se denomina como un Vector de Autenticación o Tupla de Autenticación.

En Osmocom la Generación de tuplas de autenticación es generada en una solicitud  GSUP “SendAuthInfo”, y responde mediante la “SendAuthInfoResponse” enviado al HLR por la MSC. 


El Registro de Ubicación Base (HLR- Home Location Register) sirve para las funciones AAA en una red GSM / UMTS (2G/3G) así como para localizar el Centro de Conmutación Móvil (MSC -Mobile Switching Center) que atiende a los subscriptores. 

El HLR es lo equivalente al Servidor de Subscriptores Base (Home Subscriber Server ) en LTE (he escrito bastante sobre la función del HSS en las redes LTE y he publicado mi propio software HSS de código abierto) 


![](/assets/images/osmocom/p7/OsmocomHLR-SendAuthInfoRequest.png)


## Nota al margen sobre la seguridad GSM

>En una configuración GSM, la red solo autentica a las personas subscriptoras, las personas subscriptoras no autentican la red. En la práctica, esto significa que no hay forma de verificar en GSM si la red en la que la persona está conectada es la que dice ser.

Debido a esta deficiencia y la debilidad criptográfica en el algoritmo A5/x, 3GPP especificó el algoritmo AkA para la autenticación de red mutua en redes 3G / UMTS. 

He escrito un poco sobre la función de las tarjetas SIM paa la autenticación en LTE es el mismo esquema que usa 3G/UMTS [si quieres tener más información](https://nickvsnetworking.com/hss-usim-authentication-in-lte-nr-4g-5g/)   

![](/assets/images/osmocom/p7/With-AUTN.png)

Tecnicamente la generación de vectores de autenticación es manejada por el Centro de Autenticación (AuC – Authentication Center) sin embargo, OsmoHLR tiene un AuC interna  que maneja todo esto internamente. 


## Seguimiento de Ubicación

Después de que una persona usuario ha sido autenticada, la MSC senvia un UpdateLocationRequest a través de GSUP al HLR para hacerle saber la ubicación actual de la persona subscriptora que es utilizada para la MSC. 

La Solicitud de Actualización de Ubicación ( Uptade Location Request) es enviada al inicio de la sesión, periodicamente Update Location Request se puede enviar en función a los temporizadores configurados y se puede enviar una Solicitud de Cancelación de Ubicación (Cancel Location Request)  cuando la persona subscriptora se desconecta del MSC.

![](/assets/images/osmocom/p7/OsmoHLR-Update-Location-Request.png)

## Datos de Información de la persona Subscriptora

Cuando la Solicitud de Actualización de Ubicación ha sido enviada por la MSC, el HLR envia la información  MSC de la persona subscriptora, y la MSC copia esto en su HLR interna propia llamada Registro de Ubicación de Vistantes (VLR – Visitor Location Register). El VLR significa que el MSC no necesita seguir consultado los datos de la persona usuaria del HLR.

Esto lo vuelve a solicitar el MSC al HLR a través de una solicitud de GSUP InsertSubscriberData Request que contiene:  
 
    • Subscriber’s IMSI ( IMSI de la persona Subscriptora) 
    • Subscriber’s MSISDN – MSISDN de la persona subscriptora (Número de teléfono )
    • Allowed Domains – Dominios Permitidos  (CS/PS)

![](/assets/images/osmocom/p7/OsmoHLR-Insert-Subscriber-Data-Request.png)

**Nota:** En las redes GSM en producción TCAP/MAP es usada para la comunicación entre el HLR y la MSC. Osmocom usa GSUP para transportar estos datos.

## Registro de Identidad del Equipo (Equipment Identity Register)

Dado que lo móviles son caros lo cual historicamente son objeto de robo. 
Para tratar de mitigar esto, la GSMA alienta a los operadores a implementar un Registro de Identidad del Equipo (EIR – Equipment Idetify Register) 

El EIR es esencialmente una base de datos que contiene  los IMEI ( Los Identificadores de móviles / terminales – The Identifiers of Mobiles / Terminals I y permite / niega el acceso a la red basandose en el IME. 

La idea es que si se roban un dispositivo / terminal movil, su IMEI se incluye en una lista negra de EIR e independientement de la SIM que se le ponga, no se le permite acceder a la red. 
Cuando un dispositivo se conecta a la red si la MSC está configurada soliciara el EIR (en el HLR en nuestro caso) con un Check IMEI Request (Solcitud de revisión de IMEI), y regresará un Check IMEI Result ( Resultado de revisión de IMEI)  que permitirá o negará el acceso a la red. 

![](/assets/images/osmocom/p7/OsmoHLR-Check-IMEI-Request.png)

Desafortunadamente, no hay una base de datos global de IMEI robados, lo cual significa que si un dispositivo es robado y bloqueado por la red  Operador de red móvil (MNO – Mobile Network Operator ) X ,  este podria funcionar en la red de Operador Móvil (MNO) Y, si no comparte los datos IMEI robados. 

## Iniciando y Configurando OsmoHLR

De hecho instalamos OsmoHLR en la publicación de Estación Base Controladora, así que solo necesitamos iniciar el demonio/servicio : 

```systemctl start osmo-hlr ```

Voy a habilitar la funcionalidad EIR del HSS cambiando la configuración del HLR, esto es opcional pero es útil para el usar la funcionalidad de EIR. 

Al igual que con nuestros otros elementos  de red, usamos Telnel para configurar esto de forma interactiva 

<pre>root@gsm-bts:/home/nick# telnet localhost 4258
Welcome to the OsmoHLR VTY interface
OsmoHLR> enable
OsmoHLR# configure terminal
OsmoHLR(config)# hlr
OsmoHLR(config-hlr)# store-imei
OsmoHLR(config-hlr)# exit
OsmoHLR(config)# exit
OsmoHLR# copy running-config startup-config
</pre>
Agregando Personas Subscriptoras a OsmoHLR
Pero antes de que agreguemos personas subscriptoras, dejame hablarte sobre las SIM.
De acuerdo!. He escrito antes mucho acerca de las SIM, ¡pero todavia hay mucho mas que hablar!
En realidad, solo hay una parte de la información de la SIM que neceistamos para agregar a la persona subscriptora al HLR, y eso es el IMSI: El Identificador Unico de la persona Subscriptora (The unique identifier of the subscriber) en la SIM. Normalmente puedes ver la IMSI desde tu dispositivo/ terminal móvil. 

>Si quieres autenticar personas subscriptoras adecuadamente (confirma su identidad) y habilita el cifrado (encryption) en la interface área (Um Interface), necesitas conocer la clave K de la SIM , para esto necesitas una tarjeta SIM programable como la tarjeta SIM programable de Sysmocom [Comprando de Sysmocom también estas apoyando al proyecto de Osmocom](http://shop.sysmocom.de/t/sim-card-related/sim-cards).

Así que ahora tenemos esto más claro, agreguemos una persona subscriptora:

Nos conectamos a OsmoHLR a través de Telnet, el puerto de escucha es el 4258:
<pre>
root@gsm-bts:/home/nick# telnet localhost 4258
Welcome to the OsmoHLR VTY interface
OsmoHLR> enable
OsmoHLR# subscriber imsi 001010000000004 create
OsmoHLR# subscriber imsi 001010000000004 update msisdn 61412341234
OsmoHLR# subscriber imsi 001010000000004 update aud2g comp128v3 ki 465B5CE8B199B49FAA5F0A2EE238A6BC
</pre>

Así que he creado un subscriptor con IMSI 001010000000004  en el HSS y asignandole un MSISDN (número telefónico).
Opcionalmente, si estas utilizando tarjetas SIM puedes programar la clave Ki/ K para la autenticación usando la función de actualización aud2g de lo contrario puedes omitir este paso.

Y con esto hemos creado nuestra primera persona suscrita, repita este paso con cualquier persona/ SIM  que desees suscribir. 

De forma predeterminada, los suscriptores creados con este método tienen acceso a redes de conmutación de circuitos -Circuit Switched – (Voz y SMS) y de conmutación de paquetes -Packet Switched – (Datos) . (Todavía no hemos configurado los servicios de Conmutación de Paquetes. 

Si deseas restringir el acceso a una, ambas o ninguna de las opciones anteriores, puedes hacerlo mediante el comando de actualización de suscriptor (subscriber uptade) para configurar los servicios disponibles para estos suscriptores. 

```
OsmoHLR# subscriber id 3 update network-access-mode cs+ps
OsmoHLR# subscriber id 3 update network-access-mode cs
OsmoHLR# subscriber id 3 update network-access-mode ps
OsmoHLR# subscriber id 3 update network-access-mode none
```

## Creación de Suscriptores Mediante Programación.

En realidad, si estas intentando de operar una de, no es posible agregar manualmente cada suscriptor según sea necesario. 

Si estás comprando tarjetas SIM preconfiguradas a granel, se te enviará un archivo que contiene los valores IMSI y Cifrado de cada tarjeta, y lo introducirás en el HLR.

Hemos utilizado la interfaz Osmocom VTY / Telnet en bastantes publicaciones (es de esperarque  te sientas 
cómodo(a) con ella) pero hay otra interfaz que tienen la mayoria de software de Osmocom, la interfaz de control  de Osmocom (Osmocom Control Interface) , cuyo objetivo es proporcionar una forma uniforme de interconectar scripts/programas externos con Osmocom.

Si deseas obtener más información sobre la Interfaz de Control, lea el [manual de OsmoHLR]( http://ftp.osmocom.org/docs/latest/osmohlr-usermanual.pdf)  y echa un vistazo a [estos ejemplos en Python]( https://github.com/osmocom/python_osmo-python-tests/blob/master/scripts/osmo_ctrl.py)

## Creando Suscriptores Bajo Demanda (Opcional)

Para posibles escenarios, debería de pre-suscribir cada SIM en el HLR, el IMSI de la SIM no está en el HLR, entonces el acceso es rechazado. Sin embargo, hay algunos escenarios en los que es posible que se desee permitir que cualquier persona pueda acceder a la red, en este escenario OsmoB-HLR tiene una función de “Crear suscriptores bajo demanda”.

Esto puede ser util si, por ejemplo, estás configurando una red en la que no controlas las SIM.

Supongamos que queremos crear usuarios de forma automatica con acceso a servicio de voz y datos, y asignarle un MSISDN de 10 digitos para ese suscriptor, podemos hacerlo con:

```
OsmoHLR> enable
OsmoHLR# configure terminal
OsmoHLR(config)# hlr
OsmoHLR(config-hlr)# subscriber-create-on-demand 10 cs+ps
```

Alternativamente, es posible que desee agregar suscriptores al HLR pero no agregar ningún servicio:

```
OsmoHLR> enable
OsmoHLR# configure terminal
OsmoHLR(config)# hlr
OsmoHLR(config-hlr)# subscriber-create-on-demand no-msisdn none
```

Luego, si quieres  otorgarles acceso a estos usuarios puedes usar el metodo _subscribers update network-access-mode_ del que hablamos anteriormente para permitir servicios para este usuario. 

## Captura de Paquetes

Para dar un poco más de contexto, he adjuntado una captura de paquetes de la conexión de la MSC al HLR para algunos procedimientos de conexión a la red de mi laboratorio. 

[Descargar Osmocom_HLR_GSUP_.pcap](https://nickvsnetworking.com/wp-content/uploads/2020/05/Osmocom_HLR_GSUP.pcap)


## Fuente Nickvsnetworking

[GSM con osmocom part7 The HLR – Home Location Register (and Friends)](https://nickvsnetworking.com/gsm-with-osmocom-part-7-the-hlr-home-location-register-and-friends/)

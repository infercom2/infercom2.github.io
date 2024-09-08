---
layout: single
title: Sistemas 4G Autónomos - CoLTE + Open5g - Introducción (I)
excerpt: "Un sistema aútonomo 4G, implementado con los programas Colte + Open5gs es una manera sencilla de ofrecer conectividad en zonas en donde las empresas de telecomunicaciones aún no ofrecen cobertura de sus servicios.

Este es uno de varios artículos  con fines de análisis sobre sistemas de autónomos/comunitarios, haciendo uso de software abierto como lo son CoLTE (Community LTE ) y Open5gs los cuales pueden brindar el servicio de telefonía con las tecnlogías 4G y 5G(Unicamente Open5gs), este primer articulo es una descripción del entorno de laboratorio. "
date: 2024-09-08
classes: wide
header:
  teaser: /assets/images/colte-open5gs/articles/CoLTE-open5gs-img.png
  teaser_home_page: true
  icon: /assets/images/colte-open5gs/articles/CoLTE-open5gs-img.png
categories:
  - lte
  - networking
  - open5gs
  - colte
  - telecomunicaciones
tags:
  - comunitarias
  - seguridad
  - privacidad

---

![](/assets/images/colte-open5gs/articles/CoLTE-open5gs-img.png){: height="350px" width="500px" style="display:block; margin-left:auto; margin-right:auto" }



## Introducción

[comment]: ![](/assets/images/personal/low-orbit-sate-community/conectividad-comunidades.jpg){: height="300px" width="450px" style="display:block; margin-left:auto; margin-right:auto" }


<!-- <p style="text-align: justify;">  -->
 
Un sistema aútonomo 4G, implementado con los programas Colte + Open5gs es una manera sencilla de ofrecer conectividad en zonas en donde las empresas de telecomunicaciones aún no ofrecen cobertura de sus servicios, ya que utilizan protocolos similares a los que se utilizan en nuestras redes domesticas para la conectividad de nuestros dispositivos, y algunos otros protocolos para la conexión entre dispositivos moviles y las redes 4G .

Este es uno de varios artículos  con fines de análisis sobre sistemas de autónomos/comunitarios, haciendo uso de software abierto como lo son CoLTE (Community LTE ) y Open5gs los cuales pueden brindar el servicio de telefonía con las tecnlogías 4G y 5G(Unicamente Open5gs), este primer articulo es una descripción del entorno de laboratorio.
<!--</p> -->




---
## Open5gs

Open5GS es una implementación en código abierto de la red 5G Core y EPC, compatible con los estándares 3GPP Release-17, que ofrece flexibilidad y escalabilidad para adaptarse a diferentes entornos y necesidades. Es una herramienta valiosa para la comunidad de desarrollo de software y la industria para desarrollar y probar soluciones de redes 4G o 5G.

### Caracteristicas
* Implementación abierta y gratuita para la industria y la comunidad
* Compatible con los estándares 3GPP Release-17
* Soporte para la arquitectura de red 5G Core y EPC(4G)
* Integración con diferentes componentes de red, como eNB/gNB y MME
* Flexibilidad y escalabilidad para adaptarse a diferentes entornos y necesidades

### Uso 
* Desarrollar y probar soluciones de red 4G/5G
* Realizar pruebas y demostraciones de concepto
* Implementar redes 4G/5G en entornos de prueba y desarrollo
* Ayudar a la comunidad de desarrollo de software y a la industria a entender y mejorar los estándares 4G/5G

### Comunidad y Soporte 
Open5GS cuenta con una comunidad activa y un equipo de desarrollo que proporciona soporte y documentación. Además, se ofrecen recursos adicionales, como tutoriales y ejemplos de código, para ayudar a los desarrolladores a integrar y utilizar la implementación.

## CoLTE 

Colte LTE es el Proyecto de Redes Celulares Comunitarias (Community LTE Project) desarrollado en el laboratorio de Tecnologías de la Información y Comunicación para el Desarrollo (ICTD) de la Universidad de Washington. El objetivo es expandir el acceso a Internet y conectividad en todo el mundo mediante la creación de redes LTE pequeñas y escalables que las comunidades puedan desplegar y mantener ellas mismas.

En este marco, CoLTE se enfoca en desarrollar un núcleo de red LTE pequeño que las comunidades puedan configurar y operar de manera autónoma. Esto permite a las comunidades rurales o remotas tener acceso a servicios de comunicación móvil y Internet de alta velocidad, sin depender de redes comerciales o infraestructuras existentes.


### Comunidad y Soporte 

Actualmente no cuenta con soporte actualizado, puede ser modificado de manera individual para hacerla funcionar en caso de que se presenten problemas en su ejecución. 

Nota: Actualmente me encuentro haciendo el uso de este sistema para poder hacer pruebas de control de suscriptores en un sistema LTE. 

---
## Sistemas 4G Autónomos

<p style="text-align: justify;"> 
Al instalar un sistema 4G ya sea con el sotfware Open5gs ( con el nucleo 4G), o la integración de la capa de front-end ( gestión de usuarios ) CoLTE, existen varios requisitos que se tienen que cumplir, para un entorno de implementación real, se requiere un permiso/concesión que la da la entidad de telecomunicaciones de cada país. 
</p>

### Consideraciones 
* Es muy dificil conseguir una concesión, por que ya se encuentran concesionadas a particulares y/o al mismo estado. 
* Se requiere de equipos sumamente costosos para poder cubrir muchas zonas, dependiendo del equipo que se utilice y la potencia será la extención de territorio a cubrir.
* Estabilidad de la conexión a internet 
* Capacidad y conocimiento técnico en varias areas de sistemas y redes. 


----

## Entorno de Pruebas
![](/assets/images/colte-open5gs/articles/1/lime-sdr.jpg){: height="200px" width="300px" style="display:block; margin-left:auto; margin-right:auto" }


### Software

* Virtual Box
* Maquina Virtual Debian 12 Server (CoLTE+Open5gs)
* Maquina Virtual Debian 12 Server (repositorio local de binarios)
* CoLTE + Open5gs
* IPtables
* SRSRan
* Maquina Virtual Windows 7 (Para programar tarjetas SIM)

### Hardware
* 2 Smartphones
* 1 LimeSDR Mini 2.0*
* 2 Antenas 4G/5G
* 2 Tarjetas SIM programables
* 1 Programador de tarjetas

### Instalación y configuraciones

1. Preparación de maquina(s) virtual(es) Debian (puede ser utilizada la misma maquina virtual para tener el repositorio de binarios a instalar de CoLTE)


2. Instalación de los paquetes CoLTE deb generados, a través de apt dpkg

3. Instalación de **SRSRan** y configuracion del archivo `enb.conf` para levantar el servicio de la portadora (eNB).

4. Configuración del archivo `colte.yaml` ubicado en `/etc/colte/`
```
# basic network settings
enb_iface_addr: Direccion_IP_eNB
wan_iface: Puerto conexion a internet (ejemplo: eth0)
network_name: Nombre de la red(sin caracteres especiales)
lte_subnet: Subred para la conexion equipos moviles ( Ejemplo: 10.45.0.0/16)
# PLMN = first 5 digits of IMSI = MCC+MNC
mcc: XXX (Codigo de pais)
mnc: XX (Codigo de proveedor)
Material de [Ayuda aquí](https://es.wikipedia.org/wiki/MCC/MNC) 
# advanced EPC settings
dns: IP DSN (Ejemplo: 1,1,1,1 o 8.8.8.8) 
```
Posteriormente ejecutar el comando: 
`sudo colteconf`
Con la finalidad de actualizar las secciones de Open5gs y CoLTE los valores actualizados en el archivo `colte.yaml`


### Programar SIM Card
A diferencia de la tecnología 2G que autentica la red y el telefono con el IMSI , la tecnología 4G para autenticar necesita autenticar varias credenciales con distintos protocolos en este caso las tarjetas SIM se configuran los siguientes valores: 

K   = Llave de autenticación del suscriptor (128 bit)
OP  = Codigo del Operador, Lo mismo para todas las SIM de un solo operador
OPc = Código de Operador derivado único para cada SIM. 

### CoLTE agregar SIM

CoLTE cuenta con un script para agregar las sim al sistema, tanto a PosgreSQL(haulage-colte) como a Mongo(Open5gs) y puedan conectarse al sistema.

Ejemplo: 

```
#!/bin/bash
sudo coltedb add 460660003400030 30 192.168.151.30 0x00112233445566778899AABBCCDDEEFF 0x000102030405060708090A0B0C0D0E0F
sudo coltedb add 460660003400031 31 192.168.151.31 0x00112233445566778899AABBCCDDEEFF 0x000102030405060708090A0B0C0D0E0F
sudo coltedb add 460660003400032 32 192.168.151.32 0x00112233445566778899AABBCCDDEEFF 0x000102030405060708090A0B0C0D0E0F
sudo coltedb add 460660003400033 33 192.168.151.33 0x00112233445566778899AABBCCDDEEFF 0x000102030405060708090A0B0C0D0E0F
sudo coltedb add 460660003400034 34 192.168.151.34 0x00112233445566778899AABBCCDDEEFF 0x000102030405060708090A0B0C0D0E0F
```


[Fuente](https://github.com/uw-ictd/colte/blob/main/sample_load_users.sh)

### Configuración de dispositivos móviles

* Insertar la SIM Previamente programada y agregada en el sistema 
* Configuracíon APN 
```
Nombre: internet
APN: internet
Protocolo: IPv4 (en el caso de que solo se utilice IPv4)
```

### Interfaces CoLTE
**Administración**

![](/assets/images/colte-open5gs/articles/1/colte-admin.png){: height="250px" width="350px"  }

**Usuarios**

![](/assets/images/colte-open5gs/articles/1/colte-user.png){: height="250px" width="350px"  }

### Consideraciones

* De acuerdo a al ancho de banda configurado en la radio base (eNB), afectará a la velocidad de las terminales (dispositivos móviles) conectados a la red. 
* Otros de los puntos que verán afectados la velocidad de las terminales es la conexion del servicio a internet con el que se cuente. 
* No se analiza la configuración de las llamadas entre terminales, por defecto unicamente se ofrece la conexión a internet.



--- 
## Agradecimientos 

> Un profundo agradecimiento a Fernanda Rosa Asistente de profesor de la Universidad de Virginia Tech por la donación de 2 equipos LimeSDR Mini y las antenas para la misma, con el objetivo de realizar investigaciones en cuestión de tecnologías de telecomunicación.

## Referencias: 


*  https://www.orbit-lab.org/wiki/Documentation/gWide/bLTESIM
*  https://open5gs.org/open5gs/docs/
*  https://github.com/uw-ictd/colte
*  https://github.com/infercom2/colte
---
layout: single
title: Panel de Configuracion Router Tor
excerpt: "Descripción de Panel de Configuracion de Router GL-Inet Shadow/Mango Tor"
date: 2023-01-12
classes: wide
header:
  teaser: /assets/images/tor/01-config-router/arm300-ext.jpg
  teaser_home_page: true
  icon: /assets/images/tor/01-config-router/arm300-ext.webp
categories:
  - tor
  - networking
tags:
  - linux
  - seguridad
  - privacidad

---
![](/assets/images/tor/01-config-router/arm300-ext.jpg){: height="500px" width="500px" style="display:block; margin-left:auto; margin-right:auto" }


El contenido de este artículo sirve para poder ingresar al sistema del pequeño pero poderoso Router que viene con un cliente de Tor para poder brindar conexiones dentro del circuito de la red Tor.

## Conexión del equipo 

El dispositivo es un GL-Inet  AR300M (Shadow) también puede ser usada la configuración con un GL-Inet MT300-V2 (Mango)

1. Cuenta con puertos de Red
* WAN: Para la conexión a internet, desde un proveedor de Internet Externo
* LAN: Para brindarle servicio de internet a otro equipo
2. Conexión Inálambrica: Para poder brindar conexión a equipos con conexión inálambrica

![](/assets/images/tor/01-config-router/Conexion-cableada.jpg){: height="300px" width="300px" style="display:block; margin-left:auto; margin-right:auto" }


## Acceder a la interfaz / Panel de Bienvenida

![](/assets/images/tor/01-config-router/P-B-Router.jpg)

Al conectarse al dispositivo este por defecto tiene la dirección IP **192.168.8.1** por lo que desde cualquier navegador podemos ingresar a la interfaz de configuración con la IP. 
La contraseña por **defecto** para los equipos adquiridos deben de ser los últimos 8 dígitos del número de serie.

http://192.168.8.1/

En la imagén superior se puede ver el panel de Bienvenida en donde se encuentran : 
1. Menu para las opciones
* Sistema (System)
* Red (Network)
* LUCI: Interfaz de administración a través del meta paquete de LuCI de Openwrt

2. Parámetros obtenidos de la conexión de Red Cableada (Ethernet)
* Dirección IP obtenida en el puerto WLAN
* Mascara de Red 
* Dirección MAC de la interfaz del puerto Ethernet
* Cantidad de datos Recibidos
* Cantidad de datos Transmitidos

3. Parámetros de la configuración Inálambrica 
* Dirección MAC de la tarjeta inálambrica 
* Cantidad de datos Recibidos
* Cantidad de datos Transmitidos


Más adelante se comentarán algunas consideraciones al tener activo el servicio de Tor.

## Panel de Sistema (SYSTEM)

![](/assets/images/tor/01-config-router/P-System-Router.png)

En esta sección se puede configurar algunos parámetros así como visualizar la versión actual del dispositivo, así como poder cargarle una versión más actualizada.

1. Panel de configuración 
* Nombre del equipo
* Contraseña
* Confirmación de Contraseña

Con sus respectivos botones de Guardar y Reiniciar (Save & Reboot ) y Descargar 

## Panel de Red (NETWORK)

![](/assets/images/tor/01-config-router/P-Network-Router.png)


En el panel de Red se pueden configurar algunos parámetros como son:

1. Tipo de configuración de Protocolo de Internet: DHCP
* El dispositivo se conecta a través de un cable de Red por el puerto WLAN y recibe una dirección por DHCP

2. Inálambrico / Wireless
* Habilitado (ENABLED) : Activado / Desactivado
* Nombre de la Red Inálambrica
* Seguridad (Security)
* Contraseña (Password)

3. Configuración LAN
* Dirección IP Del equipo: Si esta dirección es cambiada, se cambia a toda la red y se debe acceder por la nueva IP

Botones de Guardar y Aplicar, también el botón de Descartar


## LuCI 
![](/assets/images/tor/01-config-router/LuCi-Panel-Router-Portatil.png)

Al seleccionar el botón de LuCi nos redirige a la interfaz de Usuario LuCI que viene con openwrt, el cual puede encontrarse más información en la página del proyecto de [OpenWRT](https://openwrt.org/docs/start])

## Consideraciones

Al Habilitar El servicio de Tor:
- No se puede hacer ping a dispositivos dentro de la red, ni al exterior
- No se puede acceder a la Interfaz de Usuario
- Antes de navegar debe de verificar que se encuentre Tor Activo:  Puede verificar ingresando a la plataforma [CheckTor](https://check.torproject.org/) del Proyecto Tor
![](/assets/images/tor/01-config-router/LuCi-Panel-Router-Portatil.png)
- La Velocidad de navegación será distinta de acuerdo a los nodos conectados

## Checktor: Conexión Activa por Tor

Puede verificar que el icono de la cebolla es de color Verde cuando está conectada a través de la red Tor, así como la IP de Salida a la que está conectada

![](/assets/images/tor/01-config-router/check-tor-browser-tor-activo.png)



## Checktor: Conexión sin la red Tor

Puede verificar que el icono de la cebolla es de color Roja cuando NO está conectada a través de la red Tor, así como la IP Pública de su servicio de Internet.

![](/assets/images/tor/01-config-router/Check-tor-browser-desactivado-tor.png)

## Capturas de velocidad a tráves de Tor

Las capturas de velocidad se realizaron con una conexión de Internet Descarga: 200Mbps Subida: 100Mbps , pero la Conexión inalambrica 2.4GHz solo permite entre 50 y 70 Mbps , por lo que se pueden ver los siguientes datos:

1. Proveedor Dataideas con Ubicación en Dallas.
* Descarga: 1.9 Mbps 
* Subida: 2.8 Mbps
![](/assets/images/tor/01-config-router/speed-1.jpg)

2. Proveedor F3 Netze con Ubicación en Frankfurt
* Descarga: 3.4 Mbps
* Subida: 4.3 Mbps
![](/assets/images/tor/01-config-router/speed-2.jpg)



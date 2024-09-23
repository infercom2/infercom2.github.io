---
layout: single
title: Sistemas 4G Autónomos - CoLTE + Open5gs - Vulnerabilidades (II)
excerpt: "Este nuevo capitulo se enfoca en las vulnerabilidades, y se menciona un poco de la teoría sobre los activos de información, vulnerabilidad, la importancia y los tipos de escáneres de vulnerabilidades, y un análisis básico de lo que se podría encontrar en una red 4G autónoma.  "
date: 2024-09-22
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



Después de haber conocido un poco de los programas utilizados para la creación de una red autónoma 4G en el articulo anterior.

Este nuevo capitulo se enfoca en las vulnerabilidades, y se menciona un poco de la teoría sobre los activos de información, vulnerabilidad, la importancia y los tipos de escáneres de vulnerabilidades, y un análisis básico de lo que se podría encontrar en una red 4G autónoma.
El núcleo (core ) 4G  Open5gs + CoLTE en este entorno de pruebas, se encuentra en una maquina virtual, existe la opción de hacerla en un equipo de computo y/o un contenedor. La cual es un sistema que tendrá las mismas vulnerabilidades que un equipo de computo, normal común y corriente dependiendo de como se esté implementando, es decir existirán vulnerabilidades a distintas escalas, tanto a nivel de hardware, como a nivel de software, redes y subredes involucradas.

Debido a que los sistemas 4G autónomos, utilizan la arquitectura del Modelo OSI el funcionamiento similar a las redes de internet convencionales (cableadas, inalámbricas) para la gestión del tráfico de la información, éste articulo puede ser utilizado como apoyo para las redes de internet comunitarias que su forma de conexión pueda ser cableada o inalámbrica.






---
## Antecedentes

### Activos de Información

![](/assets/images/colte-open5gs/articles/2/activos-informaticos.png){: height="150px" width="250px" style="display:block; margin-left:auto; margin-right:auto" }

#### ¿Qué son los activos de información?

El mundo comenzó un proceso de digitalización acelerado y hiper conexión global desde principios de 2020. Uno de los aspectos más destacados fue la masiva incorporación del trabajo remoto. Esta nueva "era digital" trajo consigo grandes beneficios, pero también trajo consigo nuevas amenazas informáticas.

Es esencial proteger los activos de información a toda costa, ya que son los que dan vida a la organización y permiten que funcione correctamente. Mientras tanto, según las cifras de seguridad cibernética global, los entornos digitales, informáticos y virtuales se encuentran en una situación de mayor inseguridad que en cualquier momento anterior.

Todas las organizaciones, ya sean gubernamentales, empresas privadas u organizaciones no gubernamentales, tienen que proteger los activos de información de alto valor. Toda información o recursos necesarios para crear, almacenar, administrar o transmitir información se denominan activos de información. Estos activos de información son muy valiosos para la alta dirección porque son necesarios para que la organización funcione correctamente y logre los objetivos que ha propuesto.

Algunos activos de información son los siguientes:


#### Activos de Información materiales:

- Aparatos tecnológicos para el almacenamiento de información.
- Equipos informáticos como servidores, computadores, tablets, celulares.
- Los colaboradores que manejan información especializada y sensible.
- Las redes y sistemas que dan soporte a la comunicación de la organización.
- Instalaciones donde se almacenan otros activos de información.
- Otros contenedores como: Armarios, cajas fuertes, muebles de archivos, salas de servidores, etc.

#### Activos de información intangibles:

- Datos e información que se almacena, analiza y manipula.
- Aplicaciones de software para recopilar y trabajar la información.
- Sistemas operativos internos.
- Software de respaldo y bases de datos.
- Suministro eléctrico, telefónico e internet.
- Imagen, confiabilidad y reputación de la empresa.

### Vulnerabilidades

![](/assets/images/colte-open5gs/articles/2/vulnerabilidades.jpg){: height="350px" width="500px" style="display:block; margin-left:auto; margin-right:auto" }

Las vulnerabilidades informáticas son fallas o debilidades en sistemas, redes o aplicaciones que los atacantes pueden explotar para violar la seguridad. Por otro lado, las amenazas informáticas son acciones maliciosas destinadas a explotar estas vulnerabilidades para causar daño o robo de datos.


#### ¿Qué es el escaneo de vulnerabilidades?

El escaneo de vulnerabilidades, también conocido como "evaluación de vulnerabilidades", es el proceso de evaluar las redes o los activos de TI para detectar fallas de seguridad, es decir, fallas o debilidades que los actores de amenazas externos o internos pueden aprovechar. El primer paso en el ciclo de vida de la gestión de vulnerabilidades más amplia es el escaneo de vulnerabilidades.

#### Las vulnerabilidades de seguridad

Una vulnerabilidad de seguridad es una debilidad en la estructura, funcionalidad o implementación de cualquier activo o red de TI. Los piratas informáticos u otros actores de amenazas pueden aprovechar esta vulnerabilidad para obtener acceso no autorizado y causar daño a la red, a los usuarios o a la empresa. Las vulnerabilidades más comunes son:

- Defectos de codificación, como aplicaciones web que son susceptibles a scripting entre sitios, inyección SQL y otros ataques de inyección debido a la forma en que manejan las entradas del usuario.
- Puertos abiertos no protegidos en servidores, portátiles y otros endpoints, que los hackers podrían utilizar para difundir malware.
- Configuraciones incorrectas, como un depósito de almacenamiento en la nube que expone datos confidenciales a la red pública de Internet porque tiene permisos de acceso inadecuados.
- Falta de parches, contraseñas débiles u otras deficiencias en la higiene de la ciberseguridad.

Se descubren miles de nuevas vulnerabilidades cada mes. El Instituto Nacional de Estándares y Tecnología (NIST) y la Agencia de Seguridad de Infraestructura y Ciberseguridad (CISA) son dos agencias gubernamentales conocidas en los Estados Unidos que poseen catálogos de búsqueda de vulnerabilidades de seguridad. 

También se pueden seguir el [Top 10 de OWASP](https://owasp.org/Top10/es/) que es una comunidad abierta y sin fines de lucro dedicada a mejorar la seguridad del software, y se puede conocer las vulnerabilidades en software y hardware.

#### Importancia del escaneo de vulnerabilidades

![](/assets/images/colte-open5gs/articles/2/importancia-esc-vulns.png){: height="150px" width="250px" style="display:block; margin-left:auto; margin-right:auto" }

Las vulnerabilidades son frecuentemente descubiertas por hackers o ciberdelincuente  antes que por las organizaciones, lo que las sorprende. Para mejorar la seguridad, los equipos de TI implementan programas de gestión de vulnerabilidades, que incluyen escaneos para identificar debilidades. Este proceso continuo busca resolver riesgos de seguridad antes de que sean explotados por actores maliciosos.
Muchos equipos de seguridad también utilizan escaneo de vulnerabilidades para:

- Valide las medidas y los controles de seguridad: después de implementar nuevos controles, los equipos a menudo realizan otro análisis.
- Mantener el cumplimiento normativo.


#### Funcionamiento del proceso de escaneo de vulnerabilidades

Entre aplicaciones en la nube y locales, dispositivos móviles y de IoT, portátiles y otras terminales tradicionales, las redes empresariales modernas contienen demasiados activos para realizar escaneos de vulnerabilidades manuales. En su lugar, los equipos de seguridad utilizan escáneres de vulnerabilidades para realizar escaneos automatizados de forma recurrente.

#### Identificación de vulnerabilidades

Para identificar vulnerabilidades, los escáneres recopilan información sobre activos de TI. Luego, comparan los activos escaneados con una base de datos de vulnerabilidades (CVE), que incluye registros de problemas comunes en hardware y software. Los escáneres verifican si hay defectos, como errores en protocolos que podrían ser explotados por ciberdelincuentes.

#### Priorización e informes
El escáner produce un informe que describe las vulnerabilidades descubiertas para que el equipo de seguridad las analice. Los informes fundamentales simplemente enumeran los problemas de seguridad. Para monitorear la gestión de vulnerabilidades en el tiempo, algunos escáneres ofrecen explicaciones detalladas y comparan los resultados con escaneos anteriores. Los escáneres avanzados utilizan información de amenazas de código abierto como puntuaciones CVSS para priorizar vulnerabilidades según su gravedad. Además, pueden emplear algoritmos complejos para evaluar los defectos dentro de la organización y sugerir métodos para corregir y mitigar cada uno.

#### Programación de análisis

Los riesgos de seguridad de una red están en constante cambio a medida que se agregan nuevos activos y se detectan vulnerabilidades. Las organizaciones realizan análisis con frecuencia para estar al tanto de las amenazas cibernéticas en desarrollo. La mayoría de los análisis de vulnerabilidades no pueden abarcar todos los activos de la red de una sola vez debido a la falta de recursos y tiempo. Por lo tanto, los equipos de seguridad generalmente clasifican los activos según su importancia y los analizan en lotes, realizando escaneos más frecuentes para los activos más críticos. Además, se pueden realizar análisis cuando hay cambios significativos en la red, como cuando se agregan nuevos servidores web o se crea una nueva base de datos confidencial. Aunque algunos escáneres sofisticados ofrecen análisis continuo, esto puede no ser siempre posible debido a posibles interferencias con el rendimiento de la red.

### Tipos de escáneres de vulnerabilidades
En seguridad informática, se utilizan varios tipos de escáneres para detectar fallas en las redes. Algunos se concentran en productos particulares, como escáneres en la nube para servicios en la nube y herramientas de escaneo web para aplicaciones web. Es posible que se instalen localmente o se ofrezcan como SaaS. Algunas empresas subcontratan el análisis de vulnerabilidades, y los escáneres están disponibles de forma gratuita y de código abierto. Otras herramientas de ciberseguridad, como SIEM y EDR, pueden complementar estos escáneres. Cada vez más, los proveedores ofrecen escáneres como parte de un conjunto completo de gestión de vulnerabilidades, que incluye la gestión de la superficie de ataque, los activos, los parches y otras características importantes en una sola solución.

### Tipos de análisis de vulnerabilidades
Según las necesidades, los equipos de seguridad pueden realizar una variedad de análisis. Los siguientes son algunos de los tipos de análisis de vulnerabilidad más comunes:

- Los **análisis de vulnerabilidades externas evalúan cortafuegos**, exploran la red desde fuera y detectan fallas en activos web. Revelan cómo un ciberdelincuente puede acceder a la red desde el exterior.
- Los **análisis de vulnerabilidades internas** evalua los posibles movimientos de un ciberdelincuente al ingresar, así como la información confidencial que podría comprometerse en caso de una filtración de datos.
- Los **análisis autenticados, también llamados "análisis acreditado"**, requieren permisos de usuario autorizado. El escáner muestra la aplicación de la misma manera que un usuario autenticado lo haría. Estos análisis muestran las acciones potenciales de un ciberdelincuente con una cuenta secuestrada o daños causados por una amenaza interna de usuario.
- Los **análisis no autenticados, también llamados “análisis sin credenciales”** solo muestran activos desde una perspectiva de usuario externo y no requieren permisos. Equipos de seguridad pueden realizar análisis internos y externos sin autenticación para encontrar vulnerabilidades.

Es posible combinar varios tipos de análisis para una variedad de razones. Un análisis autenticado revelaría amenazas internas, mientras que un análisis no autenticado revelaría lo que un ciberdelincuente vería si entrara en la red. A pesar de que cada análisis tiene un propósito específico, pueden superponerse y usarse juntos.

## Desarrollo

El capitulo anterior, describe un poco las características a nivel de software del entorno de pruebas. A esto y la subred del sistema autónomo 4G.
Actualmente el sistema no almacena el tráfico que hace uso una persona usuaria, sino la cantidad de tráfico que consume.

En los sistemas informáticos, uno de los activos más importantes sino el más importante es la información que se maneja, la información por si misma no es nada, pero una vez tratada o analizada, puede ser utilizada con diferentes fines, tantos benéficos como maliciosos, por lo que es de gran importancia protegerla. Antes de protegerla, se debe de escanear los sistemas y detectar las vulnerabilidades que el o los sistemas pueden tener, para poder analizarlas y tomar decisiones para su control y mitigación, es decir identificar los niveles de riesgos a los que se puede enfrentar, dependerá de cada organización, comunidad, empresa como actuar ante estos riesgos.

Para ello, también se debe considerar un ponto importante en la implementación de controles de mitigación de riesgos, que son los niveles de madurez, no todas las organizaciones involucradas deban de tener las mismas, pero si hay reglas básicas que pueden seguir que se presentará en un próximo articulo para tocar este punto.
Lo importante es iniciar con lo básico y tener un plan recurrente de inspección del mismo, para analizar la efectividad, y analizar las políticas o controles implementados, e ir incrementando el nivel de madurez conforme pasa el tiempo, sin dejar de analizar lo ya implementado.

Para la realización de estas pruebas se hizo el uso de una versión de kali linux (distribución GNU/Linux ) para Android la cual es NetHunter, enfocada para pruebas de penetración informática, esta distribución cuenta con una gran cantidad de herramientas de pruebas y escáneres de vulnerabilidades

A continuación se mencionan las herramientas utilizadas para las pruebas realizadas.

Software

- Nethunter – Kali Linux para Android
- Software para compartir pantalla desde smartphone a  equipo de computo
- Nmap: es una herramienta de código abierto y gratuita utilizada para explorar redes y dispositivos conectados a ellas, así como para determinar sus funcionalidades y vulnerabilidades.
- Core 4G (Open5gs + CoLTE)
- Iptables: Cortafuegos/Firewall


Hardware
- 2 Celulares Inteligentes (Smartphone) Android – Sin rootear*
- Equipo de computo : anfitrion de maquinas virtuales

\* Rootear : se refiere al proceso de obtener acceso root o superusuario en un dispositivo Android, lo que permite modificar ciertas funciones y características del sistema operativo que no están disponibles para los usuarios normales.


En la documentación de Open5gs: [Quickstar (Inicio Rápido)](https://open5gs.org/open5gs/docs/guide/01-quickstart/), existe una sección llamada **“Adding a route for the UE to have WAN connectivity”** la cual nos da unas opciones de protección básica de la red y subredes, en el caso de que el servicio de internet se esté compartiendo con otra red de computadoras agenas al sistema 4G.

Con la actualización y el mejoramientos de los smartphone estos han estado desplazando a los equipos de computo para el uso cotidiano, esto no se ha quedado ahí, en temas de seguridad informática las herramientas se han adaptado para poder ejecutarlos desde de un smartphone  como lo es la herramienta NetHunter, la cual puede ser instalada sin requerir permisos de administrador o superusuario (root) teniendo limitaciones, pero pudiendo ser utilizadas para obtener información de los equipos dentro de la red móvil (puede ser utilizado para cualquier red ).

Como se mencionó previamente en la introducción, existen distintos tipos de análisis de vulnerabilidades, las cuales pueden ser enfocadas a la red, al sistema operativo, al software de gestión de contenedores, a cada software utilizado en los sistemas, en el caso de los contenedores, por estar separados los servicios no lo exime de  estar seguro, ya que es otra capa en la nivel de sofware que debe de protegerse, que puede ser explotada.

Las pruebas antes realizadas, han sido en un entorno controlado sin afectar a terceros, los escáneres o análisis de vulnerabilidades son considerados ilegales si se realizan sin el consentimiento de la organización.

La red 4G de pruebas, tiene el nombre **oaxtest**, y las tarjetas SIM tienen la leyenda **LAB_TEST**

![](/assets/images/colte-open5gs/articles/2/1_img.png "Smartphone Conectado a la Red"){: height="150px" width="250px" margin-right:auto"} ![](/assets/images/colte-open5gs/articles/2/2-img.png "Prueba de Velocidad"){: height="150px" width="250px" margin-right:auto"}

Sin contar con las políticas de control de tráfico de red que Open5gs recomiendas en su documentación, una persona con los conocimientos puede obtener información relevante como lo son, identificar los dispositivos conectados a la red.

![](/assets/images/colte-open5gs/articles/2/3-img.png){: height="150px" width="250px" style="display:block; margin-left:auto; margin-right:auto" }

##### Equipos de tectados(imagen previa):

- 10.45.0.1 – Core 4G
- 10.45.0.2 – Smartphone 1
- 10.45.0.3 – Smartphone 2


Si le logran identificar subredes dentro de la red principal, pueden ser analizadas, si no se cuentan con las restricciones de algún cortafuego o herramienta de protección de redes.

![](/assets/images/colte-open5gs/articles/2/4-img.png){: height="150px" width="250px" style="display:block; margin-left:auto; margin-right:auto" }

En la imagen anterior, desde el smartphone se pueden identificar los puertos de comunicación abiertos que pueden ser vulnerados, así como la posible información del software que está haciendo uso de este puerto de comunicación.

Un ejemplo del análisis del nucleo del sistema 4G es el siguiente, con el cual se puede obtener la información del puertos abiertos equipo y los programas utilizados así como la versión en algunos casos.

![](/assets/images/colte-open5gs/articles/2/5-img.png){: height="150px" width="250px" style="display:block; margin-left:auto; margin-right:auto" }


Una vez implementado las reglas del cortafuegos, podemos defender de poco las redes y los dispositivos conectados, me refiero a defender un poco, debido a que existen distintas técnicas  avanzadas para poder realizar  análisis y  obtener mayor información.

Teniendo protegido el host **(nucleo 4G)** con la IP de la subred **10.45.0.1** con la siguiente regla, al realizar un escaneo básico no se puede obtener mayor información del mismo:

```
sudo iptables -I INPUT -s 10.45.0.0/16 -j DROP
```

![](/assets/images/colte-open5gs/articles/2/6-img.png){: height="150px" width="250px" style="display:block; margin-left:auto; margin-right:auto" }


Para evitar analisis básicos a las redes externas con la siguiente regla: 

```
sudo iptables -I FORWARD -s 10.45.0.0/16 -d x.x.x.x/y -j DROP
```

![](/assets/images/colte-open5gs/articles/2/7-img.png){: height="150px" width="250px" style="display:block; margin-left:auto; margin-right:auto" }

El cortafuegos y cada una de las herramientas de seguridad también no estan exentas de vulnerabilidades, por lo que siempre es recomendado hacer uso de una serie de medidas para reducir o mitigar los riesgos que puedan existir.

---
## Conclusión

Un sistema puede ser protegido, pero no 100% seguro, debido a que día con día pueden ser solucionadas vulnerabilidades o surgir nuevas, por ello debemos de implementar y analizar de manera periodica nuestros sistemas informáticos.

En el caso de Open5gs, el sistema cuenta con una comunidad de desarrollo por lo que si se detecta alguna vulnerabilidad puede ser reportada y esperar a que sea solucionada, el caso de CoLTE al parecer  los desarrolladores no se encuentran dando soporte a la plataforma, por lo que es de cada persona u organización darle mantenimiento y desarrollo para adaptarlo a sus necesidades.

Estos análisis y medidas básicas pueden ser adaptadas para ser implementadas en redes de internet comunitarias, tanto sistemas cableados como inalámbricos, para la protección de la misma y de sus personas usuarias.

Es de gran importancia enfocarnos en la preparación y protección de nuestras infraestructuras autonomas, en entornos en donde organizaciones estan en defensa de los derechos humanos y/o el territorio deben de ser consideradas medidas de protección, dependiendo los activos que se deseen proteger, en los que las personas también nos convertimos en activos de importancia por lo que sabemos y/o conocemos no solo en lo tecnológico, tambien en los procesos del día a díá de nuestras organizaciones, comunidades o entidades.



---

> El único sistema completamente seguro es aquel que está apagado, encerrado en un bloque de cemento y sellado en una habitación rodeada de alambradas y guardias armados- **Bruce Schneier**

---

## Agradecimientos

> Un profundo agradecimiento a Fernanda Rosa Asistente de profesor de la Universidad de Virginia Tech por la donación de 2 equipos LimeSDR Mini y las antenas para la misma, con el objetivo de realizar investigaciones en cuestión de tecnologías de telecomunicación.

---
## Referencias
- [Open5gs Quickstart](https://open5gs.org/open5gs/docs/guide/01-quickstart/)
- [Activos de información](https://blog.redvoiss.net/que-son-los-activos-de-informacion-en-una-empresa)
- [Vulnerabilidades y Amanezadas Informaticas](https://www.dragonjar.org/vulnerabilidades-y-amenazas-informaticas.xhtml)
- [Escaneo de Vulnerabilidades](https://www.ibm.com/es-es/topics/vulnerability-scanning)
- [Vulnerabilidades](https://nvd.nist.gov/vuln)
- [Catalogo de vulnerabilidades](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)
- [Estandares de PCI DSS](https://docs-prv.pcisecuritystandards.org/PCI%20DSS/Standard/PCI-DSS-v4_0.pdf)




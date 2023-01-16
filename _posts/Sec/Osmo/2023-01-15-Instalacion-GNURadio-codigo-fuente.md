---
layout: single
title: Instalacion GNURadio 
excerpt: "En este articulo se presenta paso a paso los comandos seguidos para poder instalar GNURadio en su version 3.8, con la finalida de probar GR-GSM "
date: 2023-01-15
classes: wide
header:
  teaser: /assets/images/Sec/osmo/gnuradio-1.png
  teaser_home_page: true
  icon: /assets/images/Sec/osmo/gnuradio-1.png
categories:
  - security
  - networking
tags:
  - linux
  - seguridad
  - privacidad

---
![](/assets/images/Sec/osmo/gnuradio-1.png){: height="500px" width="500px" style="display:block; margin-left:auto; margin-right:auto" }



En este articulo se presenta paso a paso los comandos seguidos para poder instalar GNURadio en su version 3.8, con la finalida de probar GR-GSM



## Descargar Requisitos

`sudo apt install git cmake g++ libboost-all-dev libgmp-dev swig python3-numpy python3-mako python3-sphinx python3-lxml doxygen libfftw3-dev libsdl1.2-dev libgsl-dev libqwt-qt5-dev libqt5opengl5-dev python3-pyqt5 liblog4cpp5-dev libzmq3-dev python3-yaml python3-click python3-click-plugins python3-zmq python3-scipy python3-pip python3-gi-cairo`

## Instalar Volk

    cd
    git clone --recursive https://github.com/gnuradio/volk.git
    cd volk
    mkdir build
    cd build

    cmake -DCMAKE_BUILD_TYPE=Release -DPYTHON_EXECUTABLE=/usr/bin/python3 ../
    make
    make test
    sudo make install`

Después de haber instalado, el comando ldconfig para configurar los vinculos de enlazadores dinamicos creados en el momento de la instalación en caso de que no se hayan realizado en dicho proceso.


## Descargar código fuente

En este ejemplo se descarga el release 3.8.1.0

[Descargar tar.gz](https://github.com/gnuradio/gnuradio/archive/refs/tags/v3.8.5.0.tar.gz)


## Compilar Código fuente

    tar -xvf [nombrefichero.tar.gz]
    cd [directorioextraído]
    mkdir build
    cd build
    cmake -DCMAKE_BUILD_TYPE=Release -DPYTHON_EXECUTABLE=/usr/bin/python3 ../
    make -j3 (En este ejemplo el valor del 3 hace referenciaa que utilizara 3 CPU para compilar, en caso de que tu equipo cuente con 8 puedes utilizar -j8 )
    make test
    sudo make install 

    sudo ldconfig

    sudo volk_profile


## Recomendaciones

Agregar las variables de entorno para que el sistema identifique en donde están los binarios y librerias 
    
    echo 'export PYTHONPATH=/usr/local/lib/python3/dist-packages:usr/local/lib/python2.7/site-packages:$PYTHONPATH' >> ~/.bashrc
    echo 'export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH' >> ~/.bashrc
    echo 'export PYTHONPATH=/usr/local/lib/python3/dist-packages:usr/local/lib/python2.7/site-packages:$PYTHONPATH' >> ~/.profile
    echo 'export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH' >> ~/.profile
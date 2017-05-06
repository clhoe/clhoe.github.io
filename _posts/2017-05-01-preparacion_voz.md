---
layout: post
title: Preparación del sistema de reconocimiento y síntesis de voz
comments: true
author: admin
author_url: http://chloe.github.io
show: false
---

Uno de las funciones principales de una casa inteligente es la capacidad de hablar. Tanto para entender lo que le decimos como para responder a nuestras peticiones. En este artículo veremos como establecer la configuración básica del entorno de voz. Para ello usaremos el lenguaje de programación Python y supondremos que disponemos de un sistema operativo basado en Debian.

Para reconocer los comandos de voz usaremos la API de reconocimento de voz de Google: Google Speech API v2. Esta API nos permite realizar 50 peticiones al día gratuitamente. 

Empezaremos instalando los paquetes necesarios:

```
sudo apt-get install libportaudio2 libportaudiocpp0 portaudio19-dev
sudo pip install pyaudio
sudo pip install SpeechRecognition
```



---
layout: post
title: Interfaz de voz
comments: true
author: admin
author_url: #
show: false
---

Una de las funciones principales de una casa inteligente es la capacidad de hablar,  tanto para entender lo que le decimos como para responder a nuestras peticiones. En este artículo veremos como establecer la configuración básica del entorno de voz. Para ello usaremos el lenguaje de programación Python y supondremos que disponemos de un sistema operativo basado en Debian.

Empezaremos con la **síntesis de voz**. He probado diferentes herramientas disponibles para GNU/Linux, como pueden ser [Festival](http://www.cstr.ed.ac.uk/projects/festival/) o [eSpeak](http://espeak.sourceforge.net/). Sin embargo, la voz en castellano que más me ha gustado es la de Pico TTS. 

Instalaremos:

```shell

sudo apt-get install libttspico-utils alsa-utils

```

Con lo que ya podemos generar un audió a partir de texto y reproducirlo con:

```shell

pico2wave -l es-ES -w test.wav "Buenos días, ¿que tal estás?" 
aplay test.wav

```











Actualmente existen servicios de reconocimiento de voz de gran calidad, como los que ofrece [Google](https://cloud.google.com/speech/) o [Amazon](https://developer.amazon.com/alexa-voice-service). Pero en la medida de lo posible, me gustaría que este proyecto estuviese basado en software libre. Por lo tanto, aunque quizás en el futuro añada soporte para servicios de voz de terceros, partiremos del software [PocketSphinx](https://github.com/cmusphinx/pocketsphinx), de la [Carnegie Mellon University](http://www.cmu.edu/). Este softare y el modelo de voz en castellano no tiene la calidad de los servicios de Amazon y Google, pero nos sirve para empezar.

Empezaremos instalando los paquetes necesarios:

```shell

sudo apt-get install portaudio19-dev libpulse-dev
sudo pip install pyaudio
sudo pip install SpeechRecognition
sudo pip install pocketsphinx


```

Veamos como podemos usar el reconocimeinto de voz:



```python                                                                              

from pocketsphinx.pocketsphinx import Decoder                                    
                                                                                 
config = Decoder.default_config()                                                
config.set_string('-hmm', 'cmusphinx-es-5.2/model_parameters/voxforge_es_sphinx.cd_ptm_4000')
config.set_string('-lm', 'es-20k.lm.gz')                                         
config.set_string('-dict', 'es.dict')                                            
config.set_string('-logfn', '/dev/null')                                         
decoder = Decoder(config)                                                        
                                                                                 
# Decode streaming data.                                                         
decoder = Decoder(config)                                                        
decoder.start_utt()                                                              
stream = open('message.wav', 'rb')                                               
while True:                                                                      
  buf = stream.read()                                                            
  if buf:                                                                        
    print "Decode!"                                                              
    decoder.process_raw(buf, False, False)                                       
  else:                                                                          
    break                                                                        
decoder.end_utt()                                                                
                                                                                 
words=[seg.word for seg in decoder.seg()]                                        
                                                                                 
string=''                                                                        
for w in words:                                                                  
    if w in ["<s>", "</s>"]:                                                     
        continue                                                                 
    if w in ["<sil>"]:                                                           
        continue                                                                 
    string+=w+' '                                                                
                                                                                 
print string                                                                     
             
```





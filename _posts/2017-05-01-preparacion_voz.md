---
layout: post
title: Interfaz de voz
comments: true
author: admin
author_url: http://chloe.github.io
show: false
---

Uno de las funciones principales de una casa inteligente es la capacidad de hablar. Tanto para entender lo que le decimos como para responder a nuestras peticiones. En este artículo veremos como establecer la configuración básica del entorno de voz. Para ello usaremos el lenguaje de programación Python y supondremos que disponemos de un sistema operativo basado en Debian.

Para reconocer los comandos de voz usaremos la API de reconocimento de voz de Google: Google Speech API v2. Esta API nos permite realizar 50 peticiones al día gratuitamente. 

Empezaremos instalando los paquetes necesarios:

```shell
sudo apt-get install portaudio19-dev libpulse-dev
sudo pip install pyaudio
sudo pip install SpeechRecognition
```


```shell
sudo apt-get install libttspico-utils
sudo pip install pocketsphinx
```

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


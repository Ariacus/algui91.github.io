---
title: Convertir archivos mp4 a mp3 masívamente

layout: post.amp
permalink: /convertir-archivos-mp4-a-mp3-masivamente/
if_slider_image:
  -
  -
  -
  -
categories:
  - script
tags:
  - bash
  - ffmpeg
  - mp4 to mp3
format: aside
main-class: "linux"
color: "#2196F3"
---
Ya mostré cómo <a href="/renombrar-archivos-masivamente-en/" target="_blank">Renombrar archivos masivamente en GNU/Linux</a>. Hoy voy a enseñar un pequeño script que he tenido que crear para pasarme unos cuantos archivos en mp4 a mp3.

<!--ad-->

Es bastante sencillo a la vez que útil:

```bash
#!/bin/bash

IFS='
'
if  ! [ -d "mp3f" ]
then
   mkdir mp3f
fi
for i in `ls *.mp4`
do
   echo
   echo "****-----------------------------------------------------------------------------****"
   echo -e "ffmpeg -i $i -f mp3 -ab 320000 -vn ./mp3f/`basename "$i" .mp4`.mp3"
   echo "****-----------------------------------------------------------------------------****"
   echo
   ffmpeg -i $i -f mp3 -ab 320000 -vn ./mp3f/`basename "$i" .mp4`.mp3
done

IFS=' '


```

Hay que establecer la variable IFS al salto de línea, ya que por defecto es el espacio. Por lo tanto en el for, si hay un archivo con espacios, por ejemplo &#8220;Song of The Week.mp4&#8243;, el for considerará cada palabra un archivo distinto e intentará convertir **Song, of, The, Week.mp4.** por separado.

Dentro del for se usa ffmpeg para convertir el archivo a mp3, con un bitrate de 320kbps, **-vn** es para elmininar el vídeo.

Los archivos de salida se almacenan en una carpeta llamada **mp3f**, y uso **\`basename &#8220;$i&#8221; .mp4\`.mp3** para cambiar la extensión del archivo de **mp4** a **mp3**.

Por último, fijamos IFS a su valor predeterminado.

Otra opción es usar el programa _SoundConverter_.

{% include toc.html %}

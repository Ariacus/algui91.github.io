---
id: 58
title: Crear miniaturas de imágenes

layout: post
guid: http://elbauldelprogramador.org/crear-miniaturas-de-imagenes/
permalink: /crear-imagenes-en-miniaturas/
blogger_blog:
  - www.elbauldelprogramador.org
  - www.elbauldelprogramador.org
  - www.elbauldelprogramador.org
blogger_author:
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
blogger_permalink:
  - /2010/12/crear-imagenes-en-miniaturas.html
  - /2010/12/crear-imagenes-en-miniaturas.html
  - /2010/12/crear-imagenes-en-miniaturas.html
categories:
  - script
---
### Creé una Versión mejorada de este script [aquí][1]

Este script es muy simple, pero resulta muy útil cuando tenemos muchas imágenes y queremos hacer miniaturas, por ejemplo, para subirlas a nuestra web.

  
<!--more-->

  
Nota: Si tenéis una web, os aconsejo que creeis miniaturas para las imágenes que son enlaces por ejemplo, no useis las propiedades *height* y *width* de la etiqueta *img* para redimensionar una imagen grande, porque vuestra página tardará más en cargar, y además esa miniatura pesará lo mismo que la original.

Para ejecutar el script, hay que instalar imagemagick:

{% highlight bash %}<span class="comentario">#!/bin/bash</span>
FILES="$@"
<span class="bash">mkdir</span> miniaturas
<span class="bash">for</span> i in $FILES
<span class="bash">do</span>
 <span class="bash">echo</span> "Processing image $i ..."
 /usr/bin/convert -thumbnail 180x128 $i miniaturas/$i
<span class="bash">done</span>
{% endhighlight %}

Básicamente el script crea una carpeta que contendrá las miniaturas, y procesa las imagenes que le pasémos como parámetros al script, copiándolas a la carpeta creada. Para modificar el tamaño de las miniaturas, hay que cambiar *128&#215;128* por el valor que queramos.

Nota: El script hay que copiarlo en el directorio donde se encuentren las imágenes, en cuanto pueda lo modificaré para resolver esta deficiencia.

Ejemplo de uso

{% highlight bash %}$ cd Directorio/de/nuestras/imagenes
$ ./NombreDelScript *
{% endhighlight %}

El * significa que convertirá todas las imagenes del directorio



 [1]: http://elbauldelprogramador.com/crear-miniaturas-de-imagenes-mejorado/
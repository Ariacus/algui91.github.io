---
id: 1549
title: Visualiza las visitas a tu web al estilo Pong con Logstalgia

layout: post
guid: http://elbauldelprogramador.com/?p=1549
permalink: /visualiza-las-visitas-a-tu-web-al-estilo-pong-con-logstalgia/
categories:
  - Administración de Servidores
  - aplicaciones
  - opensource
tags:
  - grabar video logstalgia
  - instalar logstalgia
  - tutorial logstalgia
---
Hace poco he descubierto un programa muy curioso, **Logstalgia**, que a partir del fichero de log de acceso a una web crea una pantalla del juego Pong en la que cada línea del log representa una bola en el juego. Es una buena representación gráfica de lo que está pasando en el servidor web. Y su uso es bastante simple. Empecemos instalándolo:

{% highlight bash %}
# aptitude install logstalgia
{% endhighlight %}

Para usarlo localmente, basta con ejecutar el siguiente comando:  

{% highlight bash %}
tail -f /var/www/mySitio/log/access.log | logstalgia -1280x720 --sync
{% endhighlight %}

Por contra, si el servidor no es local, nos conectamos mediante ssh:

{% highlight bash %}
ssh usuario@dominio.com tail -f /var/www/mySitio/log/access.log | logstalgia -1280x720 --sync
{% endhighlight %}

Es posible guardar el un fichero la pantalla de logstalgia pasándole los siguientes parámetros:

{% highlight bash %}
logstalgia -1280x720 --output-ppm-stream output.ppm --sync
{% endhighlight %}

Y luego lo convertimos al formato de vídeo mp4:

{% highlight bash %}
ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i output.ppm -vcodec libx264 -preset ultrafast -pix_fmt yuv420p -crf 1 -threads 0 -bf 0 server.log.mp4
{% endhighlight %}

Os dejo un vídeo del tráfico de mi modesto blog:

<span class='embed-youtube' style='text-align:center; display: block;'></span>

#### Referencias

*Web oficial* **|** <a href="https://code.google.com/p/logstalgia/" target="_blank">Visitar sitio</a>
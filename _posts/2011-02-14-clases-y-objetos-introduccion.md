---
id: 136
title: 'Clases y objetos &#8211; Introducción'

layout: post
guid: http://elbauldelprogramador.org/clases-y-objetos-introduccion/
permalink: /clases-y-objetos-introduccion/
blogger_blog:
  - www.elbauldelprogramador.org
  - www.elbauldelprogramador.org
  - www.elbauldelprogramador.org
blogger_author:
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
blogger_permalink:
  - /2011/02/clases-y-objetos-introduccion.html
  - /2011/02/clases-y-objetos-introduccion.html
  - /2011/02/clases-y-objetos-introduccion.html
categories:
  - C
---
<div class="iconcpp">
</div>

Hace poco puse a vuestra disposición unos [tutoriales básicos de C++][1], y ahora voy a comenzar a explicar las clases y los objetos en C++:  
  
<!--more-->

## Introducción



En la programación orientada a objetos, los datos y el código que actúan sobre los datos se convierten en una única entidad denominada clase. La clase es una evolución del concepto de estructura, ya que contiene la declaración de los datos. Pero se le añade la declaración de las funciones que manipulan dichos datos, denominadas funciones miembro o, también, métodos. Además, en la clase se establecen unos permisos de acceso a sus miembros. Por defecto, en una clase los datos y las funciones se declaran como privados. Este ocultamiento de la información niega a las entidades exteriores el acceso a los miembros privados de un objeto. De este modo, las entidades  
exteriores acceden a los datos de una manera controlada a través de algunas funciones miembro.

Un objeto es semejante a cualquier otro tipo de variable. Primero lo declaramos reservando espacio en memoria, luego lo inicializamos, guardando en memoria un dato o un conjunto de datos. Posteriormente usamos esos datos guardados en memoria. Pero un objeto de una determinada clase no solamente sirve para guardar datos, sino que además puede manipular dichos datos, a través de las llamadas a las funciones miembro.

Suponemos que el lector está familiarizado con los aspectos básicos de un lenguaje de programación estructurado:

  * Los conceptos de variable, tipos de variables, alcance y duración de las variables.
  * El control del flujo de un programa.
  * El concepto de función: declaración, definición y llamada.

Además, introduciremos dos aspectos del lenguaje C++: los comentarios en una sola línea, y la entrada/salida, mucho más simples en C++ que en C.

## Comentarios



Recordaremos que el lenguaje C++ mantiene los delimitadores de comentarios empleados en el lenguaje C, pero añade comentarios en una sola línea.  


{% highlight bash %}<span class="comentario">//éste es un comentario de una sola línea</span>{% endhighlight %}



## Entrada/Salida



El archivo cabecera <var>STDIO.H</var> contiene la información relativa a las operaciones estándar de entrada/salida en el lenguaje C. Las funciones más utilizadas de esta librería son scanf y printf. Estas funciones pueden seguir siendo utilizadas en C++, pero su uso es más complicado, ya que hemos de pasar no sólo la variable, sino también el formato: ***%d*** para enteros, ***%f*** para números decimales, etc.

En C++, los detalles de la entrada/salida estándar están en el archivo *IOSTREAM.H*: la entrada se efectúa mediante el stream ***cin>>*** y la salida mediante el stream ***cout< <**</b>*. En los ejemplos que siguen se compara la entrada/salida en C++ y en C.

**Salida**

{% highlight bash %}float f=3.24;
cout < < “Numero real =”<<f<<’n’;
printf (“Numero real %f=n”, f);
{% endhighlight %}

**Entrada**

{% highlight bash %}int i;
cin&gt;&gt;i;
scanf (“%d”, &i);
{% endhighlight %}

* * *

#### Siguiente tema: [Clases y Objetos &#8211; Definir una Clase][2] {.referencia}



 [1]: http://elbauldelprogramador.com/primeros-pasos-en-c/
 [2]: http://elbauldelprogramador.com/clases-y-objetos-definir-una-clase/
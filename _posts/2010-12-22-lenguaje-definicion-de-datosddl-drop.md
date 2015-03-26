---
id: 66
title: 'Lenguaje Definición de Datos (DDL) &#8211; DROP'

layout: post
guid: http://elbauldelprogramador.org/lenguaje-definicion-de-datos-ddl-drop/
permalink: /lenguaje-definicion-de-datosddl-drop/
blogger_blog:
  - www.elbauldelprogramador.org
  - www.elbauldelprogramador.org
  - www.elbauldelprogramador.org
blogger_author:
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
blogger_permalink:
  - /2010/12/lenguaje-definicion-de-datosddl-drop.html
  - /2010/12/lenguaje-definicion-de-datosddl-drop.html
  - /2010/12/lenguaje-definicion-de-datosddl-drop.html
categories:
  - BaseDeDatos
tags:
  - clausulas del ddl
  - concepto de ddl
  - que es un ddl
  - sentencias dml y ddl
---
<div class="icosql">
</div>

En el anterior post vimos la orden [CREATE.][1]  
En este post explicaré el funcionamiento de la orden DROP.

#### Borrado de una tabla (estructura y datos)

{% highlight sql %}Drop table nombre_tabla [CASCADE CONSTRAINT];{% endhighlight %}

Al borrar una tabla, se borra tanto su estructura como sus datos, sus índices asociados y los privilegios concedidos sobre estas también se borran, las vistas creadas directa o indirectamente sobre esta tabla son desactivadas de forma automática por ORACLE pero no borradas.  
  
<!--more-->

  
Cada usuario puede borrar sus propias tablas, pero no puede borrar las de otro usuario al menos que tenga concedido un permiso adecuado.  
Si se hace referencia a la clave primaria de esta tabla mediante restricciones FOREIGN KEY o REFERENCES, la clausula CASCADE CONSTRAINT permite suprimir estas restricciones de integridad referencial en las tablas ‘descendientes’.

#### Borrado de los registros de una tabla

Con la orden TRUNCATE se eliminan todas las filas de una tabla y se puede liberar espacio utilizado por esta tabla. Es una orden del lenguaje DDL y por tanto no se puede anular. Tampoco activa disparadores DELETE por lo que es más rápido que una orden DELETE. Su sintaxis es:

{% highlight sql %}Truncate table nombre_table [{DROP | REUSE} STORAGE];{% endhighlight %}

Con DROP STORAGE se desasigna todo el espacio.  
Con DROP REUSE mantendrá reservado el espacio para nuevas filas.

* * *</p> 

#### Siguiente Tema: [Lenguaje Definición de Datos (DDL) &#8211; ALTER TABLE][2] {.referencia}



 [1]: http://elbauldelprogramador.com/lenguaje-definicion-de-datosddl-create/
 [2]: http://elbauldelprogramador.com/lenguaje-definicion-de-datos-ddl-alter/
---
id: 69
title: 'Lenguaje Definición de Datos (DDL) &#8211; Vistas'

layout: post
guid: http://elbauldelprogramador.org/lenguaje-definicion-de-datos-ddl-vistas/
permalink: /lenguaje-definicion-de-datos-ddl-vistas/
blogger_blog:
  - www.elbauldelprogramador.org
  - www.elbauldelprogramador.org
  - www.elbauldelprogramador.org
blogger_author:
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
blogger_permalink:
  - /2010/12/lenguaje-definicion-de-datos-ddl-vistas.html
  - /2010/12/lenguaje-definicion-de-datos-ddl-vistas.html
  - /2010/12/lenguaje-definicion-de-datos-ddl-vistas.html
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

Las vistas son tablas virtuales ‘que contienen’ el resultado de una consulta SELECT, tienen la misma estructura que una tabla cualquiera, es decir, están organizadas por filas y columnas. Una de las principales ventajas de utilizar vistas procede del hecho de que la vista no almacena los datos, sino que hace referencia a una o varias tablas de origen mediante una consulta SELECT, consulta que se ejecuta cada vez que se hace referencia a la vista. De esta forma, cualquier modificación que se realice sobre los datos de las tablas de origen es inmediatamente visible en la vista, cuando ésta vuelva a ejecutarse. Su sintaxis es:  
  
<!--more-->

{% highlight sql %}CREATE [OR REPLACE] VIEW Nombre_vista
[(Lista de columnas)]
AS SELECT[...]{% endhighlight %}

La opción REPLACE, lo que hace es, reemplazar la vista en el caso de que esta ya exista. Las vistas se utilizan de forma análoga a las tablas, permitiendo realizar consultas sobre las vistas, también se pueden realizar sentencias [DML][1] sobre las vistas, sin embargo, las modificaciones, borrados e inserciones están restringidas a vistas que estén definidas sobre una única tabla.

{% highlight sql %}<span class="comentario">-- Vista para mostrar datos de departamentos y empleados</span>
CREATE OR REPLACE VIEW my_emp_view AS
SELECT d.department_id, d.department_name,
  e.employee_id, e.first_name, e.last_name 
  FROM employees e 
  JOIN departments d ON d.manager_id = e.employee_id;{% endhighlight %}

**Vistas interactivas.** Son vistas que se definen sin utilizar el comando CREATE VIEW, sino directamente sobre el comando SELECT. Por ejemplo:

{% highlight sql %}SELECT cCodCli, cNomCli, Importe
     FROM Clientes C,(SELECT SUM (Cantidad*Precio) AS Importe
        FROM Articulos a, LinPedidos l
        WHERE a.cCodArt = l.cCodArt
        GROUP BY nPedido) LP,
   Pedidos P
     WHERE c.cCodCli = p.cCodCli AND p.nPedido = LP.nPedido ;{% endhighlight %}

En el caso anterior, LP actúa como una vista interactiva.

#### Borrado de una vista.

La orden para borrar una vista es DROP VIEW. Su sintaxis es:

{% highlight sql %}DROP VIEW Nombre_vista{% endhighlight %}



* * *</p> 

#### Siguiente Tema: [Lenguaje Definición de Datos (DDL) &#8211; Índices y secuencias][2] {.referencia}



 [1]: http://es.wikipedia.org/wiki/Lenguaje_de_Manipulaci%C3%B3n_de_Datos
 [2]: http://elbauldelprogramador.com/lenguaje-definicion-de-datos-ddl/
---
title: Tunning básico de SQL
layout: post.amp
permalink: /tunning-basico-de-sql/
categories:
  - BaseDeDatos
tags:
  - que es tuning sql
main-class: "BaseDeDatos"
color: "#009688"
---
<div class="icosql">
</div>

Bueno, hemos llegado al fin del temario de base de datos, todo lo que he ido escribiendo a lo largo de estos meses lo encontrarás en la página de [Base de Datos][1]. El último tema de este curso va a tratar de &#8220;Tunning básico de SQL&#8221;.  

<!--ad-->

## Tunning básico de SQL



Una de las tareas más importantes de las propias de un desarrollador de bases de datos es la de puesta a punto o tuning. Hay que tener en cuenta que las sentencias SQL pueden llegar a ser muy  
complejas y conforme el esquema de base de datos va creciendo las sentencias son más complejas y  
confusas. Por es difícil escribir la sentencia correcta a la primera.

Por todo ello después de tener cada uno de los procesos escrito, hay que pasar por una etapa de tuning en la que se revisan todas las sentencias SQL para poder optimizarlas conforme a la experiencia adquirida.

Tanto por cantidad como por complejidad, la mayoría de las optimizaciones deben hacerse sobre sentencias SELECT, ya que son (por regla general) las responsables de la mayor pérdida de tiempos.

A continuación se dan unas normas básicas para escribir sentencias SELECT optimizadas.

&#8211; Las condiciones (tanto de filtro como de join) deben ir siempre en el orden en que esté definido el índice. Si no hubiese índice por las columnas utilizadas, se puede estudiar la posibilidad de añadirlo, ya que tener índices de más sólo penaliza los tiempos de inserción, actualización y borrado, pero no de consulta.

&#8211; Evitar la condiciones IN ( SELECT&#8230;) sustituyéndolas por joins.

&#8211; Colocar la tabla que devuelve menor número de registros en el último lugar del [FROM][2].

&#8211; Una consulta cualificada con la cláusula DISTINTC debe ser ordenada por el servidor aunque no se incluya la cláusula [ORDER BY][3].



 [1]: http://bashyc.blogspot.com/p/bases-de-datos.html
 [2]: https://elbauldelprogramador.com/consulta-de-datos-clausula-from/
 [3]: https://elbauldelprogramador.com/consulta-de-datos-clausula-having-y/

{% include toc.html %}
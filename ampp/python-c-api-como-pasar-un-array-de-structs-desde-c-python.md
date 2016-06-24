---
title: 'Python C API &#8211; Cómo pasar un array de structs desde C a Python'

layout: post.amp
permalink: /python-c-api-como-pasar-un-array-de-structs-desde-c-python/
categories:
  - C
  - python
tags:
  - modulos python
  - python C API
  - structs c
image: 2013/03/Crear-un-módulo-para-python-con-la-Python-C-API-Parte-I.png
description: "Hace algún tiempo estaba desarrollando un módulo para python, con el cual aprendí bastante y sirvió para crear una serie de artículos sobre el tema (Crear un módulo para python con la Python C API). Uno de los problemas encontrados fue cómo conseguir pasar un array de structs desde C a Python, para resolver la duda pregunté, cómo no, en stackoverflow. Al final terminé respondiéndome a mí mismo, pero aprendí bastante, veamos cómo hacerlo."
modified: 2015-12-22T10:55:00+00:00
main-class: "dev"
color: "#E64A19"
---

<figure>
  <a href="/assets/img/2013/03/Crear-un-módulo-para-python-con-la-Python-C-API-Parte-I.png"><img src="/assets/img/2013/03/Crear-un-módulo-para-python-con-la-Python-C-API-Parte-I.png" title="{{ page.title }}" alt="{{ page.title }}" /></a>
</figure>

Hace algún tiempo estaba desarrollando un módulo para python, con el cual aprendí bastante y sirvió para crear una serie de artículos sobre el tema ([Crear un módulo para python con la Python C API][1]). Uno de los problemas encontrados fue cómo conseguir pasar un array de structs desde C a Python, para resolver la duda pregunté, cómo no, en <a href="http://stackoverflow.com/users/1612432/algui91" title="PErfil en SO" target="_blank">stackoverflow</a>. Al final terminé respondiéndome a mí mismo, pero aprendí bastante, veamos cómo hacerlo.

<!--ad-->

El struct a pasar puede ser cualquiera, pero en el ejemplo concreto era este:

```c
struct tcpstat
{
    inet_prefix local;
    inet_prefix remote;
    int     lport;
    int     rport;
    int     state;
    int     rq, wq;
    int     timer;
    int     timeout;
    int     retrs;
    unsigned    ino;
    int     probes;
    unsigned    uid;
    int     refcnt;
    unsigned long long sk;
    int     rto, ato, qack, cwnd, ssthresh;
};

```

La solución consiste en crear un objeto <a href="http://docs.python.org/3.2/c-api/list.html" title="C API doc" target="_blank">PyListObject</a> y un <a href="http://docs.python.org/3.2/c-api/structures.html#PyObject" target="_blank">PyObject</a>. Éste último lo usaremos como un diccionario y será donde iremos añadiendo los datos necesarios del struct. De esta forma estamos construyendo una lista cuyos elementos son diccionarios, algo así:

```python
[
   {'clave1': 'valor1',  # Diccionario 1, con dos elementos.
    'clave1_2': 'valor1_2'},
   {'clave2' : 'valor2'}, # Diccionario 2, con un elemento.
   {'clave3' : 'valor3',
    #........ : .......,
    'claven' : 'valorn'}, # Diccionario 3, con N elementos.
]

```

El código es el siguiente:

```c
PyObject *dict = NULL;
PyListObject *list;

list = (PyListObject *) Py_BuildValue("[]");

int i = 0;
for (i; i < stats_length; i++) {
    dict = Py_BuildValue("{s:i}", "LPort", stats[i].lport);
    PyList_Append(list, dict);
}

return (PyObject *) list;

```

En el ejemplo sólo se está almacenando un campo del struct, para almacenar más, simplemente habría que modificar la línea por:

```c
dict = Py_BuildValue("{"
                     "   s:i,"
                     "   s:i"
                     "}",
                        "Dir Local.", stats[i].lport,
                        "Dir Remota.", stats[i].rport);

```

Y continuar rellenando el diccionario según nuestras necesidades.

#### Referencias

*Python C API How to pass array of structs from C to Python* »» <a href="http://stackoverflow.com/questions/15786525/python-c-api-how-to-pass-array-of-structs-from-c-to-python/15833209#15833209" target="_blank">stackoverflow.com</a>



 [1]: https://elbauldelprogramador.com/crear-modulo-python-con-python-c-api-1/ "Crear un módulo para python con la Python C API (I) – Introducción"

{% include toc.html %}
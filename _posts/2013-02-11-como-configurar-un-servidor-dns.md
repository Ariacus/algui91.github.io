---
id: 1308
title: 'Cómo configurar un servidor DNS &#8211; Parte 1 (Introducción)'

layout: post
guid: http://elbauldelprogramador.com/?p=1308
permalink: /como-configurar-un-servidor-dns/
categories:
  - Administración de Servidores
  - Artículos
  - internet
tags:
  - A records
  - bind
  - bind tutorial dns
  - CNAME
  - como configurar un servidor dns
  - como crear dns localhost
  - como crear una zona primaria en dns
  - configura un servidor dns
  - configuracion de namedconf
  - configuración de servidores
  - configuracion dns servidor dedicado
  - configuracion refresco zona dns
  - configuracion servidor dns
  - configurar bind9
  - configurar dns
  - configurar servidor dns
  - debian
  - implementar servidor dns en debian
  - MX records
  - named.conf
  - servidor debian
  - servidor dns
  - servidor dns debian
  - servidores dns
  - soa correo
---
  * Cómo configurar un servidor DNS &#8211; Parte 1 (Introducción)
  * [Cómo configurar un servidor DNS &#8211; Parte 2 (La Zona Primaria)][1]
  * [Cómo configurar un servidor DNS &#8211; Parte 3 (Zona Inversa y DNS secundario)][2]

<p class="alert">
  En esta serie de artículos, intentaré explicar lo mejor posible el funcionamiento de los servidores DNS, y cómo configurar el tuyo propio. Habrá una parte más teórica sobre el funcionamiento del sistema, que es una traducción de un artículo en howtoforge.
</p>

<p class="alert">
  Ya que los artículos están basados en distintas fuentes de información que he ido recopilando, no sé de cuantas partes estará formada esta serie, así que la lista de arriba irá cambiando hasta que estén completos todos los artículos.
</p>

Debo reconocer que el tema de los DNS me ha dado muchos problemas, es algo que para mí ha sido dificil de entender. A base de leer y releer muchos artículos por internet, aprendí a configurar un servidor DNS manualmente. Hoy voy a explicar cómo.

En Linux, **BIND **es el encargado de gestionar los DNS, como su página de ayuda indica (*bind &#8211; bind a name to a socket*), asocia un nombre a un socket. Es importante que antes de continuar compruebes que la versión de **BIND **es superior a la 4. Lo ideal sería tener la 8 o 9. Puedes comprobarlo con el siguiente comando:

{% highlight bash %}$ nslookup -type=txt -class=chaos version.bind servidor
Server:     servidor
Address:    IP#53

version.bind   text = "VERSION"{% endhighlight %}

**BIND** tiene tres componentes, el primero es llamado *named* o *name-dee*, es un demonio que ejecuta el lado servidor del DNS.

El segundo componente es llamado *resolver library* o biblioteca de resolución, encargada de realizar peticiones a servidores DNS para intentar traducir un nombre a una dirección IP. El archivo de configuración para este componente es **resolv.conf**.

El tercer y último componente de **BIND** proporciona herramientas para probar el servidor DNS. Entre estas herramientas se encuentran comandos como **dig**, que se verá más adelante.  
  
<!--more-->

### ¿Cual es tu responsabilidad como parte del sistema DNS?

DNS es una [base de datos][3] distribuida. Cuando pagas por un dominio, debes configurar dos servidores de nombres, y éstos deben ser registrados en el sistema DNS.

La base de datos del sistema DNS tiene tres niveles. Al primer grupo de servidores se les llama “**servidores root**”. Al segundo, “**Top Level Domains (TLDs)**” o dominios de primer nivel. Cuando se necesita conocer la dirección de una web, el segundo componente de **BIND** (resolver library) realiza una petición, (De aquí en adelante lo llamaremos *resolver*).

Por ejemplo, supongamos que quieres encontrar a **google.com**. Tu resolver pide a los servidores root que identifique la IP de google.com. El servidor root responde, “*No lo sé, pero sí sé donde puedes encontrar la respuesta, comienza con los servidores TLD para COM*”.

Así, el servidor root envia la petición a un servidor COM. Éste último servidor dice: “*No tengo esa información, pero sé de un servidor de nombres que sí, tiene dirección 173.194.34.6 y nombre ns1.google.com. Dirígete a esa dirección y te dirá la dirección del sitio web google.com.”*

<img class="thumbnail aligncenter size-full wp-image-1334" alt="Esquema servidores DNS" src="/images/2013/02/dns.png" width="513" height="399" />

En la figura de arriba, la parte superior izquierda representa los servidores root. En la jerga DNS, éstos servidores reprensentan el comienzo del camino en el sistema DNS. Suelen representarse con un punto (“.”). En los archivos de configuración, el mapeo entre IP y nombre acabará en un punto. A lo largo de esta series de artículos quedará más claro este concepto.

Los servidores root son los principales de la base de datos distribuida. Poseen información sobre los **Top Level Domains (TLDs)** o dominios de primer nivel. En los TLDs se incluyen *com, net, org, mil, gov, edu etc*. Al contratar un nombre de dominio, es necesario elegir qué TLD se desea, este blog se encuentra en el espacio de nombres COM y se llama *elbauldelprogramador.com*.

### ¿Cómo responde el servidor DNS a las peticiones?

En este punto es donde **BIND** entra en acción. El primer componente que mencionamos, **named**; está presente en todos los servidores de nombres y es el encargado de responder a las peticiones de los resolvers. Lee sus datos del archivo de configuración* named.conf*. Éste fichero obtiene su información de unos ficheros a los que se les suele llamar *zone files* ó *ficheros de zona*. Existen multidud de ellos, pero un archivo de zona en particular mantiene una base de datos de registros que proporciona named con la mayoría de sus respuestas.

En la figura 2, *named* ha recibido una petición. Busca en su fichero de configuración *named.conf*, que busca en el archivo de zona primaria y pasa la información solicitada al resolver desde el exterior.

<div id="attachment_1335" style="width: 421px" class="wp-caption aligncenter">
  <img class="thumbnail size-full wp-image-1335" alt="Figura 2 - Respondiendo a una petición" src="/images/2013/02/config.png" width="411" height="185" />
  
  <p class="wp-caption-text">
    Figura 2 &#8211; Respondiendo a una petición
  </p>
</div>

### Usando Named.conf

El proceso *named* escucha en el puerto 53 en los sitemas Linux. Al recibir una petición para una dirección, busca en el primer archivo de configuración que conoce, *named.conf*. Tal y como se aprecia en la figura 2.

El fichero tiene la siguiente estructura:

{% highlight cpp %}// This is the primary configuration file for the BIND DNS server named.
//
// Please read /usr/share/doc/bind9/README.Debian.gz for information on the 
// structure of BIND configuration files in Debian, *BEFORE* you customize 
// this configuration file.
//
// If you are just adding zones, please do that in /etc/bind/named.conf.local

include "/etc/bind/named.conf.options";
include "/etc/bind/named.conf.local";
include "/etc/bind/named.conf.default-zones";{% endhighlight %}

Veamos el contenido de los tres archivos que incluye:

**named.conf.options**

{% highlight bash %}options {
    directory "/var/cache/bind";
};{% endhighlight %}

Aquí se definen el directorio por defecto para named.

**named.conf.default-zones**

{% highlight bash %}zone "." {
   type hint;
  file "/etc/bind/db.root";
};

zone "localhost" {
   type master;
    file "/etc/bind/db.local";
};

zone "127.in-addr.arpa" {
   type master;
    file "/etc/bind/db.127";
};

zone "0.in-addr.arpa" {
   type master;
    file "/etc/bind/db.0";
};

zone "255.in-addr.arpa" {
   type master;
    file "/etc/bind/db.255";
};{% endhighlight %}

**zone &#8220;.&#8221;** contiene los nombres y direcciones de los servidores root. Como se mencionó arriba, éstos servidores saben en qué servidores autorizados existe tu dominio &#8212; Siendo el primero los TLD (com, org, net etc) y el segundo el servidor autorizado para tu dominio.

**zone &#8220;localhost&#8221;**. Cada servidor de nombres administra su propio dominio loopback (127.0.0.1). El motivo de crear una zona para localhost es reducir tráfico y permitir que el mismo software funcione en el sistema como lo hace en internet.

El resto de las zonas son archivos de zonas inversas. Es una copia invertida de la base de datos definida en los otros archivos. Es decir, asocia una IP a un nombre, al contrario. Se pueden indentificar por la extensión **in-addr.arpa**.

En el siguiente artículo ser verá en detalle la estructura del archivo **named.conf.local**, en el que se definen nuevas zonas que corresponden a dominios que resolverá el servidor DNS. Así como a los archivos *pri.nombredominio.com* asociados a cada zona.

#### Referencias

*Traditional DNS Howto* **|** <a href="http://www.howtoforge.com/traditional_dns_howto" target="_blank">Visitar sitio</a> 



 [1]: /articulos/como-configurar-un-servidor-dns2/ "Cómo configurar un servidor DNS – Parte 2 (La Zona Primaria)"
 [2]: /articulos/como-configurar-un-servidor-dns3/ "Cómo configurar un servidor DNS – Parte 3 (Zona Inversa y DNS secundario)"
 [3]: http://elbauldelprogramador.com/bases-de-datos/
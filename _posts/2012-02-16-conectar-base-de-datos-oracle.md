---
id: 340
title: Conectar base de datos ORACLE a aplicación Java remotamente

layout: post
guid: http://elbauldelprogramador.org/conectar-base-de-datos-oracle-a-aplicacion-java-remotamente/
permalink: /conectar-base-de-datos-oracle/
blogger_blog:
  - www.elbauldelprogramador.org
  - www.elbauldelprogramador.org
blogger_author:
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
blogger_permalink:
  - /2012/02/conectar-base-de-datos-oracle.html
  - /2012/02/conectar-base-de-datos-oracle.html
if_slider_image:
  - 
  - 
share_data:
  - '[]'
  - '[]'
share_all_data:
  - '{"like_count":"0","share_count":"0","twitter":0,"plusone":3,"stumble":0,"pinit":0,"count":3,"time":1333551708}'
  - '{"like_count":"0","share_count":"0","twitter":0,"plusone":3,"stumble":0,"pinit":0,"count":3,"time":1333551708}'
share_count:
  - 0
  - 0
categories:
  - android
  - aplicaciones
  - BaseDeDatos
  - opensource
tags:
  - aplicacion consultar base de datos oracle en java
  - conectar java con oracle
  - curso android pdf
  - integrar oracle en android
---
<div class="separator" style="clear: both; text-align: center;">
  <img style="clear: left; float: left; margin-right: 1em; margin-bottom: 1em;" src="/images/2013/07/iconoAndroid.png" alt="" border="0" />
</div>

Hace bastante tiempo, publiqué una entrada sobre cómo [Conectar base de datos sql Server 2008 a aplicación Java remotamente][1], aunque tiene mucho tiempo, sigue siendo la entrada más visitada y más comentada del blog. Debido a ello, hace poco un lector contactó conmigo comunicándome que disponía de una implementación del código de conexión, pero en lugar de ser sobre sql server 2008, era para ORACLE. Desde ya darle las gracias a Edwin por colaborar (Al que podéis seguir en <a href="https://plus.google.com/u/0/b/108003822606696308728/110549682438236698342/posts" target="_blank">G+</a> y [twitter][2]) y a continuación os dejo la implementación junto con la explicación de cómo hacerlo:

  
<!--more-->

> Esta colaboración la hago porque me he sumado al esfuerzo y apoyo de Alejandro Alcalde, ya que él  
> implementó la conexión de android con SQLSERVER 2008. En vista de que en internet casi no hay información  
> de que alguien lo ha realizado, y para empeorar las cosas hay muchas afirmaciones de que no se puede conectar  
> android a ninguna base de datos, solamente con un webservice (y no se recomienda otra forma que no sea esta).  
> Yo encontré nada más el post de Alejandro Alcalde, a partir de ahí pude realizar la conexión y me di a la  
> tarea de implementarlo para oracle y mysql también y compartirlo con quien lo necesite.
> 
> (En este post tratamos el caso de oracle).
> 
> Escenario:
> 
> * Base de datos Oracle 10g.
> 
> * Android API 2.2.
> 
> * Eclipse Helios Service Release 2.
> 
> NOTA: Antes de lograr que funcionara tuve problemitas con encontrar la clase jdbc adecuada para conectarme. Probé las siguiente sin éxito:  
> ojdbc5.jar y ojdbc6.jar. En el caso de la ojdbc5.jar yo la utilizo en una aplicación que hice anteriormente, conectándome a la misma base de datos, pero no me funciona con android en este caso.
> 
> Así que en el tercer intento con ojdbc14.jar acerté.
> 
> A continuación les detallo puntualmente la implementación de la clase:
> 
> 1. Agregar al buildpath la librería ojdbc14.jar. La cual pueden bajar del siguiente enlace:
> 
> http://www.oracle.com/technetwork/database/enterprise-edition/jdbc-10201-088211.html
> 
> 2. Clase de conexión:
> 
> El esquema SCOTT viene con datos de prueba, cuando instalé mi base de datos oracle.

{% highlight java %}package org.pkg.conn;

import java.io.PrintWriter;
import java.io.StringWriter;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
public class ConnOracle {
 private java.sql.Connection con=null;
 private java.sql.Statement stmt;
 public ResultSet resultado;
 //consulta: sentencia sql. modo: 1-&gt;insert, delete, update. 2-&gt;select.
 public String consultaOrcl(String consulta, Integer modo)
 {
  String regs ="";
  StringWriter sw = null;
  PrintWriter pw = null;
  try
  {
   String driver = "oracle.jdbc.OracleDriver";
   String ulrjdbc = "jdbc:oracle:thin:SCOTT/PASSWORD@SERVER_IP:1521:SERVICE_NAME";
   Class.forName(driver).newInstance();
   con = DriverManager.getConnection(ulrjdbc);
   stmt = (Statement) con.createStatement();
   //modo=1 -&gt; insert,update,delete; modo=2 -&gt; select
   if (modo == 1)
   {
    stmt.executeUpdate(consulta);
   }
   else
   {
    resultado = (ResultSet) stmt.executeQuery(consulta);
   }
   while (resultado.next())
            {
          regs = regs + "EMPNO: " + resultado.getString(1) + " ENAME: "+ (resultado.getString(2)) + " JOB: "+ (resultado.getString(3))+"n";
            }
   try
   {
    resultado.close();
    stmt.close();
    con.close();
   }
   catch (SQLException e )
   {
    sw = new StringWriter();
    pw = new PrintWriter(sw);
    e.printStackTrace(pw);
    return "rn" + sw.toString() + "rn";
            }
  }
  catch (Exception ex)
  {
   sw = new StringWriter();
   pw = new PrintWriter(sw);
   ex.printStackTrace(pw);
   return "rn" + sw.toString() + "rn";
  }
  return regs;
 }
}{% endhighlight %}

Tenéis el código disponible en [PasteBin][3] también.



 [1]: /2011/04/conectar-base-de-datos-sql-server-2008.html
 [2]: https://twitter.com/muymuynica
 [3]: http://pastebin.com/embed_js.php?i=zU4sfhzv
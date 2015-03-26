---
id: 285
title: 'Programación Android: StrictMode'

layout: post
guid: http://elbauldelprogramador.org/programacion-android-strictmode/
permalink: /programacion-android-strictmode/
blogger_blog:
  - www.elbauldelprogramador.org
  - www.elbauldelprogramador.org
blogger_author:
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
blogger_permalink:
  - /2011/11/programacion-android-strictmode.html
  - /2011/11/programacion-android-strictmode.html
share_count:
  - 0
  - 0
share_all_data:
  - '{"like_count":"0","share_count":"0","twitter":0,"plusone":0,"stumble":0,"pinit":0,"count":0,"time":1333551805}'
  - '{"like_count":"0","share_count":"0","twitter":0,"plusone":0,"stumble":0,"pinit":0,"count":0,"time":1333551805}'
share_data:
  - '[]'
  - '[]'
categories:
  - android
  - opensource
tags:
  - curso android pdf
  - StrictMode Android
---
<img id="logo" class="icono" border="0" height="128" width="128" src="" name="droid" />

[Android][1] 2.3 introdujo una funcionalidad de depuración llamada *StrictMode*. Según Google, usaron esta característica para hacer cientos de mejoras a sus aplicaciones Android. Lo que hace el *StrictMode* es informar de las violaciones de políticas relacionadas con los [hilos][2] y la máquina virtual. Si se detecta dicha violación, obtenemos una alerta que nos lo indica. Junto a la alerta tendremos también una traza de la pila de ejecución (Stack Trace), donde podremos comprobar el lugar en el que se produjo la violación. En ese momento, podemos forzar el cierre de la aplicación o simplemente escribirla en el log y dejar que la aplicación continue su ejecución.

Actualemente hay dos tipos de políticas disponibles para usar con *StrictMode*. La primera de ellas es referente a los hilos y está destinada principalmente a ejecutarse en el hilo principal, también conocido como el hilo de UI (User Interface).

  
<!--more-->

No es una buena práctica hacer lecturas/escrituras a disco desde el hilo principal, como tampoco lo es realizar accesos a red. Google ha añadido al código del disco y de red *hooks* o *ganchos*, que son algoritmos abstractos que invocan a métodos abstractos. Por lo tanto, si activamos StrictMode para uno de nuestros hilos, y ese hilo realiza cualquiera de las dos acciones mencionadas anteriormente, seremos informados. Podemos elegir sobre qué aspectos de la política de hilos queremos ser informados, así como el método por el cual se nos informará. Normalmente las que usaremos serán accesos a disco y red. En cuanto al método por el que seremos informados, pordemos elegir: Escribirlo en el LogCat, mostrar un [diálogo][3], hacer un destello en la pantalla, escribir en el archivo log de DropBox o forzar el cierre de la aplicación. Normalmente se usa el LogCat o forzar el cierre. A continuación vemos un ejemplo de como configurar StrictMode para políticas de hilos:

{% highlight java %}StrictMode.setThreadPolicy(new StrictMode.ThreadPolicy.Builder()
          .detectDiskReads()
          .detectDiskWrites()
          .detectNetwork()   // or .detectAll() for all detectable problems
          .penaltyLog()
          .build());
{% endhighlight %}

Podemos ver que la calse Builder hace que configurar StrictMode sea bastante fácil. Todos los métos que definen las políticas devuelven una referencia al objeto Builder. El último, *build()*, devuelve un objeto *ThreadPolicy*, que es el argumento que *setThreadPolicy()* espera recibir. *setThreadPolicy()* es un método estático, por ello no es necesario instanciar un objeto de tipo *StrictMode*. En el ejemplo anterior, se ha configurado la política para que avise de lecturas-escrituras a disco, acceso a red y vamos a ser informados a través del logCat. Podemos usar *detectAll()* para ahorrarnos escribir los otros métodos de detección. También podemos usar el método *penaltyDeath() para forzar el cierre de la aplicación una vez escrita la alerta StrictMode al LogCat.*

Debido a que con el código de arriba hemos activado el StrictMode, no necesitamos seguir activándolo. Por lo tanto, podemos habilitar StrictMode al principio del método *onCreate()* de nuestra [actividad][2] principal, ya que se ejecuta en el hilo principal, avisándonos de todo lo que pase en ese hilo.

StrictMode también dispone de *VmPolicy*, que comprueba pérdidas de memoria si un objeto SQLite finaliza antes de que haya sido cerrado, o si cualquier objeto que pueda ser cerrado ha finalizado antes de ser cerrado. Las VmPolicy se crean de una forma similar como se muestra a continuación. Sin embargo, hay una diferencia entre éstas y ThreadPolicy, que no pueden usar alertas a través de diálogos.

{% highlight java %}StrictMode.setVmPolicy(new StrictMode.VmPolicy.Builder()
          .detectLeakedSqlLiteObjects()
          .penaltyLog()
          .penaltyDeath()
          .build());
{% endhighlight %}

Ya que la configuración ocurre en un hilo, quizás nos sorprenda que el código se está ejecutando en el hilo principal, pero la traza de la pila (Stack Trace) está ahí para ayudarnos a averiguar en qué punto se produjo la violación. Una vez detectada la violación, podemos resolverla moviendo el código que la produce a un hilo en segundo plano propio. O por otra parte podemos decidir que el código está bien en esa parte y no moverlo.

Algo importante que debemos hacer a la hora de distribuir nuestra aplicación es apagar el StrictMode. Hay varias formas de conseguir esto. La más sencilla es eliminar las llamadas a los métodos, pero hacer esto dificulta seguir desarrollando la aplicación. Podemos declarar un booleano al nivel de la aplicación y comprobar su valor antes de llamar a StrictMode. De este modo, al enviar la aplicación a producción, simplemente daríamos a esta variable el valor false y de se llamará nunca a StrictMode.

Un método más elegante para resolver este problema, es usar el atributo *debuggable* en nuestro [AndroidManifest][4]. Este atributo se coloca en el tag *<application></application>* de la forma *android:debuggable*. Una vez activado este atributo, puede fijarse como verdadero o falso dependiendo de si queremos depurar la aplicación o no. Podemos comprobar el estado de este atributo como se muestra más abajo. De modo que cuando esté activado, tendremos StrictMode activo, y cuando no lo esté, no.

{% highlight java %}//Devuelve si la aplicación está en modo debug o no
ApplicationInfo appInfo = context.getApplicationInfo();
int appFlags = appInfo.flags;
if ((appFlags & ApplicationInfo.FLAG_DEBUGGABLE) != 0){
   //Aquí configuraríamos el StrictMode
}
{% endhighlight %}

Si usamos eclipse como IDE, el plugin ADT configura el atributo debuggable automáticamente. Es decir, cuando usamos el emulador o un dispositivo real, eclipse fija el atributo debuggable a verdadero, lo que nos permite usar StrictMode y depurar nuestra aplicación. Cuando exportamos la aplicación para crear una versión de producción, eclipse lo fija a falso. Hay que tener cuidado con esto, ya que si nosotros añadimos el atributo a mano, eclipse no lo cambiará.

StrictMode no funciona en versiones Android anteriores a la 2.3. Si queremos usarlo con versiones anteriores, podemos usar técnicas espejo para llamar indirectamente a los métodos de StrictMode:

{% highlight java %}try{
   Class strictMode = Class.forName("android.os.StrictMode");
   Method enableDefaults = strictMode.getMethod("enableDefaults");
   enableDefaults.invoke(null);

}catch(Exception e){
   //Este dipositivo no soporta StrictMode
   Log.v("StrictMode", "no soportado, ignorando...");
}
{% endhighlight %}

El código de arriba determina si la clase StrictMode existe, y si existe, llama a *enableDefaults()*. En caso de no existir la aplicación no finalizará, puesto que hemos tratado la excepción y el bloque catch se invocará con una excepción del tipo *ClassNotFoundException*.

Si el StrictMode no está disponible para nuestra aplicación, se lanzará un error del tipo *VerifyError* al intentar acceder a él. Si envolvemos a StrictMode en una clase y capturamos el error, lo prodremos ignorar si StrictMode no está habilitado. A continuación vamos a ver un ejemplo creando esta clase.

{% highlight java %}import android.content.Context;
import android.content.pm.ApplicationInfo;
import android.os.StrictMode;
 
public class StrictModeWrapper{
    public static void init(Context context){
       ApplicationInfo appInfo = context.getApplicationInfo();
       int appFlags = appInfo.flags;
       if ((appFlags & ApplicationInfo.FLAG_DEBUGGABLE) != 0){
          StrictMode.setThreadPolicy(new StrictMode.ThreadPolicy.Builder()
                    .detectDiskReads()
                    .detectDiskWrites()
                    .detectNetwork() 
                    .penaltyLog()
                    .build());
           StrictMode.setVmPolicy(new StrictMode.VmPolicy.Builder()
                     .detectLeakedSqlLiteObjects()
                     .penaltyLog()
                     .penaltyDeath()
                     .build());
       }  
    }
}
{% endhighlight %}

Como se puede apreciar, simplemente hemos metido todos los ejemplos que vimos anteriormente en una clase. Ahora para configurar StrictMode tenemos que hacer lo siguiete:

{% highlight java %}try{
   StrictModeWrapper.init(this);

}catch(Throwable throwable){
   Log.v("StrictMode", "no soportado, ignorando...");
}
{% endhighlight %}

*this* es el contexto local del objeto desde el que llamemos al método init, por ejemplo podría ser desde el método *onCreate()* de nuestra [actividad][2] principal.

# Ejemplo de uso

### Mediante el penaltyLog():

<div class="separator" style="clear: both; text-align: center;">
  <a href="http://3.bp.blogspot.com/-IdweIPhRyVQ/TsOuiL-BQQI/AAAAAAAABx8/X2hvK1eoP00/s1600/Screenshot-1.png" imageanchor="1" style="margin-left:1em; margin-right:1em"><img alt="android development" border="0" height="260" width="400" src="http://3.bp.blogspot.com/-IdweIPhRyVQ/TsOuiL-BQQI/AAAAAAAABx8/X2hvK1eoP00/s400/Screenshot-1.png" /></a>
</div>

### Mediante penaltyDialog():

<div class="separator" style="clear: both; text-align: center;">
  <a href="http://1.bp.blogspot.com/-osWDkvmrHug/TsOu3XOySgI/AAAAAAAAByI/2TbyygetI14/s1600/Screenshot.png" imageanchor="1" style="margin-left:1em; margin-right:1em"><img border="0" alt="android" height="302" width="400" src="http://1.bp.blogspot.com/-osWDkvmrHug/TsOu3XOySgI/AAAAAAAAByI/2TbyygetI14/s400/Screenshot.png" /></a>
</div>

* * *

#### Siguiente Tema: [Proveedores de Contenido &#8211; Introducción][5] {.referencia}





 [1]: /search/label/android
 [2]: /fundamentos-programacion-android_17/
 [3]: /programacion-android-interfaz-grafica_11/
 [4]: /fundamentos-programacion-android_16/
 [5]: /programacion-android-proveedores-de/
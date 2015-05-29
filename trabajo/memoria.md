#María Orgaz Martínez y Carmen Arcos Aneas#
## ¿Qué es memcached y cómo usarlo? ##

### ¿Qué es memcached? ###

 
**Memcached** es una herramienta desarrollada por la empresa Danga Interactive, que mantiene el proyecto bajo licencia BSD y que fue pensado para incrementar la velocidad de aplicaciones web dinámicas, reduciendo la carga de la base de datos.

Memcached se ejecuta en uno o varios equipos que actuará(n) como servidores y de los cuales, entre otros parámetros, se puede ajustar el tamaño de la memoria que queremos que utilice para ello.

Una vez instalados los servidores, las aplicaciones a través de una API podrán guardar elementos en la caché, recuperarlos o borrarlos, todo esto de una forma muy sencilla.

Memcached permite almacenar los resultados devueltos de una consulta a la base de datos, pero además de estos datos, se puede almacenar cualquier información que se nos ocurra, por ejemplo resultados de cálculo, información de sesión de los usuarios,etc.

Memcached tiene versiones para Linux y se distribuye bajo licencia de software libre permisiva. Tambien cuenta con versiones para Windows y MacOS, si bien no son oficiales, cuenta con el apoyo de varios desarrolladores de la comunidad.

### Funcionamiento ###

El uso de Memcached reduce el número de consultas en la base de datos. Cuando uno hace uso de este sistema para la creación de páginas web, lo primero que hay que hacer es consultar al servidor de Memcached si tiene almacenada la información solicitada. Si éste tiene la información la devuelve y si no debemos hacer la petición a la base de datos e introducir los datos en Memcached, para que puedan ser accedidos directamente la próxima vez que los necesitemos.

Su funcionamiento se basa en una tabla hash, distribuida a lo largo de varios equipos, donde va almacenando los valores asociados a una clave única para cada elemento. Conforme ésta se va llenando, los datos que más tiempo llevan sin ser utilizados se borran para dar espacio a los nuevos. Normalmente, las aplicaciones comprueban primero si pueden acceder a los datos a través de Memcached antes de recurrir a un almacén de datos más lento, como puede ser una base de datos.

Cada dato introducido en Memcached tiene un tiempo de expiración; pasado ese tiempo, el servicio Memcached lo elimina, dejando ese espacio para el almacenamiento de otro objeto.

![](https://github.com/MariaOrgaz/swap1415/blob/master/trabajo/imagenes/1.png)

### ¿Quiénes usan memcached? ###

El desarrollo inicial y sus posteriores mejoras nacen como una necesidad de incrementar velocidades de respuesta para las peticiones web, en sitios de tráfico masivo.

Actualmente, su uso continúa expandiéndose, a medida que el proyecto toma mayor fuerza con la ayuda de varios desarrolladores de la comunidad Open Source que revisan y agregan nuevas capacidades al proyecto.

Este sistema es empleado por varios de los sitios más activos y visitados de la red, como YouTube, Reddit, Playdom, Zynga, Facebook y Twitter.


Para solucionar el problema que supone acceder a los datos almacenados sin poner en peligro la velocidad del portal y optimizar el tráfico de entrada y salida, estos portales utilizan Memcached.

Memcached almacena datos en el caché y evita así tener que acceder a la base de datos de MySQL para recuperarlos más adelante. De esta manera se mejora y garantiza la velocidad de carga del portal.


### Instrucciones usadas por memcached###

Algunos comandos de almacenamiento implican el envío de un valor de expiración (relativo a un ítem o a una operación solicitada por el cliente) al servidor. En todos los casos, el valor real enviado podría ser en tiempo Unix (un valor de tipo integer con el número de segundos transcurridos desde el 1 de enero de 1970), o el número de segundos comenzando desde el instante actual. En este caso, el número no debe exceder de 60* 60* 24* 30 (que corresponde al número de segundos en 30 días).

Si se excede el valor del tiempo de expiración, el servidor lo considerará como tiempo Unix en lugar del número de segundos desde el instante actual.

Si el valor de la expiración es 0 (el predeterminado), el ítem nunca caducará (aunque puede ser eliminado del servidor para hacer sitio a otros ítems).


**addServer(string $host , int $port [, int $weight = 0 ])**

Agrega el servidor especificado al grupo de servidores. No se establece la conexión en ese momento. Si vamos a añadir más de un servidor es recomendable usar addServers(), pues así la actualización de las estructuras internas se realizará una sola vez.
Es posible añadir el mismo servidor varias veces pues no hay control sobre la duplicación de estos. En su lugar podemos usar la opción "weight" para aumentar la ponderación de selección de dicho servidor.



    <?php
    $m = new Memcached();
    
    /* Añadimos dos servidores, el segundo con más probabilidad de ser seleccionado */
    $m->addServer('mem1.domain.com', 11211, 33);
    $m->addServer('mem2.domain.com', 11211, 67);
    ?>



**delete(string $key [, int $time = 0 ])**

Esta función elimina la clave de memcached. Podemos añadir un parámetro de tipo time que indicará el tiempo en segundos que deseamos que el servidor rechaze añadir o reemplazar comandos para esta clave.
Durante ese tiempo el elemento se pone en una cola de eliminación y no es posible recuperarla mediante el comando get. Tras ese tiempo el elemento se elimina de la memoria. Por defecto este parámetro es 0, es decir se borrará inmediatamente y si ordenamos su posterior almacenamiento podremos hacerlo.

    
    <?php
    $m->delete('key1');
    ?>


**flush([ int $delay = 0 ] )**

Invalida todos los elementos existentes en caché inmediatamente o después del tiempo especificado.

    <?php
    /* Invalida todos los elementos en 10 segundos */
    $m->flush(10);
    ?>

**set(string $key , mixed $value [, int $expiration ] )**

Guarda el valor dado por value en un servidor de memcached bajo la clave especificada por key. El parámetro expiration se puede emplear para controlar cuándo se considera que ha expirado el valor.

**replace(string $key , mixed $value [, int $expiration ])**

Es similar a set(), pero la operación fallará si la clave dada por key no existe en el servidor.

**get(string $key)**

Devuelve el ítem que fue previamente guardado bajo la clave dada por key.

**getStats()**

Devuelve un array que contiene el estado de todos los servidores de memcached disponibles.

**fetch()**

Obtiene el siguiente resultado de la última petición.

**fetchAll()**

Obtiene todos los resultados restantes de la última petición.

### Ventajas de usar memcached ###

- Reducir los niveles de carga de los servidores. Ésta es la primera gran ventaja del uso de este sistema, ya que gracias a él se reducen el número de consultas a la base de datos.

- Memcached permite ajustar el espacio de memoria dedicado, dado que se ejecuta como servidores montados sobre uno o varios equipos, permitiendo indicar la cantidad de memoria destinada para el almacenamiento de información.

- Este sistema ofrece la posibilidad de almacenar lo que se quiera, a criterio del desarrollador. Lo que facilita que se puedan cachear cosas tan diversas como resultados de cálculos o consultas a base de datos complejas, información de sesiones de los usuarios, etc.

- La comunicación que se produce entre clientes y el servidor es muy sencilla, y basada en comandos.

- Ofrece la posibilidad de controlar el tiempo de vida de un objeto, indicando el “tiempo de expiración” asociado al mismo, en el momento de realizar una operación de almacenamiento.


### Instalación ###
**1.Pre-requisitos** 
	
- Tener instalado Ubuntu 14.0
- Tener instalado LAMP (PHP +MySQL + Apache)

Desde la versión 0.2.0 de Mencached se requiere la versión 5.2.0 o superior de PHP.

**2.Instalación de mencached** 
La instalación de memcached se realiza de forma diferente dependiendo del sistema:

- Ubuntu/Debian: `sudo apt-get install memcached`

- RedHat/Fedora: `yum install memcached`

- FreeBSD: `portmaster databases/memcached`  

Veamos el proceso detallado de instalación en Ubuntu 14.04.2 :

Instalamos Memcached:

Para la instalación de memcached usamos la librería php5-memcached.

`sudo apt-get install php5-memcached memcached`

![](https://github.com/MariaOrgaz/swap1415/blob/master/trabajo/imagenes/foto%20(2).png)

Para comprobar que se ha instalado correctamente creamos un archivo llamado info.php con el siguiente contenido:

    <html>
    <body>
    <?php phpinfo();
    ?>
    </body>
    </html>

Desde el navegador comprobamos la ejecución ejecutando la url:
`localhost\info.php`

![](https://github.com/MariaOrgaz/swap1415/blob/master/trabajo/imagenes/foto%20(4).png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/trabajo/imagenes/foto%20(3).png)

Es muy importante activar el paquete. Accedemos al archivo memcached.ini que encontramos en la ruta /etc/php5/conf.d

y descomentamos la línea que contiene el siguiente texto:
`extension=memcached.so`

Reiniciamos Apache para actualizar los cambios:

`sudo /etc/init.d/apache2 restart`

###Ejemplo de uso###

En el siguiente ejemplo hacemos uso de una base de datos creada en mysql con el nombre "productos". En dicha tabla podemos encontrar la tabla "usuariosProductos" con el siguiente contenido:

![](https://github.com/MariaOrgaz/swap1415/blob/master/trabajo/imagenes/foto%20(1).png)


    <?php
      require_once('config.php');

      //Creamos el objeto de la clase Memcache
      $memcache = new Memcached();
      $memcache->addServer('localhost', 11211) or die ("Could not connect");

      //Conectamos a la base de datos
      try {
      $conexion = new PDO('mysql:host=localhost;dbname=productos', 'root', 'contraseña');
      $conexion->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
      } catch ( PDOException $e ) {
      die($e->getMessage());
      }

      //Creamos la consulta y la key
      $sql = "SELECT usuario,producto FROM usuariosProducto WHERE id=1";
      $key = md5($sql);

      //Comprobamos si los datos están en memoria
      $resultado = $memcache->get($key);

      //En caso contrario buscamos en la base de datos
      if (!$resultado) {
	    $resultado = $conexion->query($sql);
	    $resultado = $resultado->fetchAll();
	    $memcache->set($key, $resultado, time() + 3600);
	    $mem = $memcache->getStats();
      }

      //cerramos la conexión con la bd
      $conexion = null;
    ?>
    
Tras una primera ejecución obtenemos que el objeto no está en memcached
![](https://github.com/MariaOrgaz/swap1415/blob/master/trabajo/imagenes/foto%20(5).png)

Si volvemos a ejecutar la orden obtendremos el resultado deseado

![](https://github.com/MariaOrgaz/swap1415/blob/master/trabajo/imagenes/foto%20(6).png)

### Referencias ###

[http://www.maestrosdelweb.com/memcached-un-alivio-para-las-bases-de-datos/](http://www.maestrosdelweb.com/memcached-un-alivio-para-las-bases-de-datos/)

[http://pressroom.hostalia.com/white-papers/memcached](http://pressroom.hostalia.com/white-papers/memcached)

[http://es.wikipedia.org/wiki/Memcached](http://es.wikipedia.org/wiki/Memcached)

[https://www.digitalocean.com/community/tutorials/how-to-share-php-sessions-on-multiple-memcached-servers-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-share-php-sessions-on-multiple-memcached-servers-on-ubuntu-14-04)

[http://www.juntadeandalucia.es/servicios/madeja/contenido/recurso/265](http://www.juntadeandalucia.es/servicios/madeja/contenido/recurso/265)

[http://luauf.com/2009/06/08/%C2%BFque-es-memcached/](http://luauf.com/2009/06/08/%C2%BFque-es-memcached/)

[http://php.net/manual/en/book.memcached.php](http://php.net/manual/en/book.memcached.php)

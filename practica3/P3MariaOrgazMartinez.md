## Práctica 3. Balanceo de carga ##

### María Orgaz Martínez y Carmen Arcos Aneas ###

**1. Instalar nginx**

Importamos la clave del repositorio software: 

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica3/imagenes/Captura%20de%20pantalla.png)

Editamos el fichero `/etc/apt/sources.list`:

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica3/imagenes/Captura%20de%20pantallaa.png)

Y ahora instalamos el paquete nginx: 

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica3/imagenes/Captura%20de%20pantalla3.png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica3/imagenes/Captura%20de%20pantalla4.png)

**2. Balanceo de carga usando nginx**

Vamos a ir modificando el fichero `/etc/nginx/conf.d/default.conf`, de tal forma que quede como se muestra en la siguiente imagen:

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica3/imagenes/contenido.png)

A continuación modificamos el contenido del archivo index.html de la máquina 1 para que su contenido sea: "otro contenido" y así diferenciarla de la máquina 2:

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica3/imagenes/cambioindex.png)

Lanzamos el servicio usando la IP del balanceador y probamos que funciona pues unas veces muestra el contenido de index.html de la máquina 1 y otras el de la máquina 2:

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica3/imagenes/comprobacion.png)

**3. Configurando nginx**

Hemos establecido que la máquina 1 tendrá el doble de carga que la máquina 2 y por defecto las peticiones son mediante round-robin.

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica3/imagenes/Captura%20de%20pantalla%20(21).png)

**4. Instalando y configurando haproxy**

Instalamos haproxy con el comando indicado

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica3/imagenes/Captura%20de%20pantalla7.png)

Y cambiamos la configuración inicial del fichero `/etc/haproxy/haproxy.cfg` como se indica:

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica3/imagenes/Captura%20de%20pantalla111.png)

Comprobamos el funcionamiento lanzando el servicio haproxy. Para ello previamente debemos parar Nginx.

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica3/imagenes/haproxy.png)






## Práctica 2 - SWAP ##

### María Orgaz Martínez y Carmen Arcos Aneas ###

**1. Instalar la herramienta rsync**
	
Procedemos a instalar la herramienta rsync a través del comando **apt-get**

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica2/imagenes/foto1.png)

Y como podemos ver, ya lo teníamos instalado.

Ahora vamos a verificar su correcto funcionamiento clonando la carpeta con el contenido del servidor web.
Previamente hemos activado 

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica2/imagenes/foto2.png)

Comprobamos que se ha clonado bien

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica2/imagenes/foto3.png)

**2. Acceso sin contraseña para ssh**

Tras generar la clave mediante el comando:
`ssh-keygen -t dsa`

y copiar dicha clave:
`ssh-copy-id -i .ssh/id_dsa.pub  maria@192.168.159.98 `

 vemos que ya podemos conectarnos a la máquina1 usando el comando:
`ssh 192.168.159.98 -l root`

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica2/imagenes/foto4.png)

Para ejecutar comandos los añadimos al final del último comando.

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica2/imagenes/foto5.png)

**3. Programar tareas con crontab**
Hemos establecido una tarea en la que la máquina actualiza el contenido del directorio `/var/www` en el minuto 0 de cada hora.

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica2/imagenes/foto6.png)



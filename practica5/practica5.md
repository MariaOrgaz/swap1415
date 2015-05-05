# Práctica 5 - Replicación de bases de datos MySQL#

**María Orgaz Martínez y Carmen Arcos Aneas**

##Creando una Base de Datos##

Accedemos a mysql mediante el comando
(`mysql -u root -p`)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(1).png)

Creamos la base de datos e introducimos algunos datos tal y como se muestra en la siguiente imagen:

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(2).png)

##Replicando la BD con mysqldump##

Antes de nada debemos evitar que se acceda a la BD que vamos a replicar con el comando (`FLUSH TABLES WITH READ LOCK`)

a continuación guardamos los datos de la base de datos con (`mysqldump contactos -u root -p > /root/contactos.sql`)

Por último desbloqueamos las tablas que habíamos bloqueado con
(`UNLOCK TABLES`)

En la siguiente foto podemos ver la realización de este proceso:

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(3).png)

Ahora nos vamos a la máquina esclavo para copiar dicho archivo 
(`scp root@192.168.159.98:/root/contactos.sql /root/`)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(4).png)

Ahora ya podemos acceder a mysql en la segunda máquina e importar la BD completa creando la BD y restaurando los datos contenidos en la BD:

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(5).png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(6).png)

##Replicando la BD mediante configuración maestro-esclavo##

Primero modificamos el archivo /etc/mysql/my.cnf en ambas máquinas tal y como se indica en el guión:

Añadimos:
server-id=1 (2 en la máquina2)
log_bin = /var/log/mysql/bin.log
log_error = /var/log/mysql/error.log

Comentamos:
bind-address 127.0.0.1 

Una vez hecho esto reiniciamos el servicio con:
(`/etc/init.d/mysql restart`)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(7).png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(8).png)

Ahora vamos a la máquina2, iniciamos mysql y ejecutamos la siguiente sentencia

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(9).png)

Tras esto hacemos uso del comando (`SHOW MASTER STATUS`) para obtener los datos de la base de datos a replicar.

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(10).png)

Para darle a la máquina esclava los datos del maestro usamos lo siguiente
(`CHANGE MASTER TO MASTER_HOST='192.168.159.98', MASTER_USER='esclavo', MASTER_PASSWORD='esclavo', 
MASTER_LOG_FILE='.000002', MASTER_LOG_POS=107, MASTER_PORT=3306;`)

Podemos comprobar que todo es correcto pues si arrancamos el esclavo y hacemos (`SHOW SLAVE STATUS\G`) la variable Seconds_Behind_Master es distinto de null

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(11).png)

Ya podemos desbloquear las tablas en la máquina maestro y probar a añadir más datos

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(12).png)

Comprobando que los cambios se actualizan en la máquina esclava

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica5/imagenes/practica5%20(13).png)
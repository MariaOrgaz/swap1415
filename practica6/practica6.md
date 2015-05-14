#Practica 6 - Discos en RAID #

###María Orgaz Martínez y Carmen Arcos Aneas###

##Configuración del RAID por software##

Vamos a usar nuestra máquina virtual llamada Rendimiento y en primer lugar le añadimos dos discos del mismo tipo y capacidad:

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica6/imagenes/Captura%20(1).png)

Arrancamos la máquina y ejecutamos la siguiente orden para instalar el software necesario:
 `sudo apt-get install mdadm`

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica6/imagenes/Captura%20(2).png)

Ejecutando el comando `sudo fdisk -l` vemos la información relativa a los discos, y vemos que no están particionados correctamente.

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica6/imagenes/Captura%20(3).png)

Procedemos a crear el RAID1:
`sudo mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc`

A continuación le damos formato:
 `sudo mkfs /dev/md0`

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica6/imagenes/Captura%20(4).png)

Y creamos el directorio donde se montará la unidad del RAID:

`sudo mkdir /datos`
`sudo mount /dev/md0 /datos`

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica6/imagenes/Captura%20(5).png)

Comprobamos el estado del RAID:
`sudo mdadm --detail /dev/md0`

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica6/imagenes/Captura%20(6).png)

Por último configuramos el sistema para que monte el dispositivo RAID creado al arrancar el sistema.
Para ello primero ejecutamos la orden `ls -l /dev/disk/by-uuid`

Para saber el UUID del RAID

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica6/imagenes/Captura%20(7).png)

Y ya podemos editar el archivo `/etc/fstab`

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica6/imagenes/Captura%20(8).png)

##Simulando un fallo con mdadm##

Forzamos un fallo en una unidad con el comando `mdadm --manage --set-faulty /dev/md127 /dev/sdc`

vemos los detalles de la unidad con
`mdadm --detail /dev/md127`

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica6/imagenes/Captura%20(9).png)

Tras esto podemos seguir accediendo a la carpeta /Datos

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica6/imagenes/Captura%20(10).png)

volvemos a añadir el disco al array
`mdadm /dev/md127 -a /dev/sdc`

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica6/imagenes/Captura%20(11).png)
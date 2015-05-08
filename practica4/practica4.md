#Práctica 4 - Comnprobar el rendimiento de servidores web#

##María Orgaz Martínez y Carmen Arcos Aneas##

**Apache Benchmark**

Comprobamos el rendimiento de una sóla máquina servidora, haciendo peticiones a la IP de la máquina1, en este caso 192.168.159.98. También hacemos esto para la granja web. 
Para ello vamos ejecutaremos el comando ab con el que realizaremos 100000 peticiones que realizaremos concurrentemente de 100 en 100:

`ab -n 100000 -c 100 http://ip_maquina/prueba.html`

Foto

Realizamos 5 mediciones y sacamos la media y la desviación estándar.

**máquina 192.168.159.98**
![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/Cap1.png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/Cap2.png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/captura%20(7).png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/2.png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/3.png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/4.png)

**Httperf**

Abriremos un total de 27000 conexiones TCP y las hará a 150 peticiones por segundo. Con la opción timeout indicamos el número de segundos que el cliente esperará la respuesta del servidor.

`httperf --server ip_maquina --port 80 --uri /prueba.html --rate 150 --num-conn 27000 --num-call 1 --timeout 5 `

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/captura%20(1).png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/5.png)


![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/6.png)


![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/7.png)

**Openload**

Pondremos como número de clientes simultáneos a simular 900

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/openload.png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/captura%20(6).png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/8.png)

![](https://github.com/MariaOrgaz/swap1415/blob/master/practica4/imagenes/9.png)


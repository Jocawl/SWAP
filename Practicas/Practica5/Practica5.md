# Práctica5. Replicación de bases de datos MySQL

En esta práctica vamos a realizar una copia de seguridad de nuestra base de tados MySQL, la opción que usaremos es la de réplica maestro-esclavo.
En nuestro caso la máquina2 hará la función de maestro y la máquina1 de esclavo.

## 1. Crear un BD e insertar datos

Crearemos una BD(en la máquina2) de pruebas en MySQL e insertaremos algunos datos. Así tendremos datos para poder realizar las copias de seguridad.

Entramos en la interfaz de comandos de MySQL con la orden:
~~~
mysql -uroot -p
~~~
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica5/inicioBD.PNG?raw=true)
Y creamos la BD e insertamos algunos datos.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica5/crearTablas.PNG?raw=true)
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica5/insertardatos.PNG?raw=true)

## 2. Replicar una BD con mysqldump

Para realizar la copia de seguridad, en la máquina2, haremos:
~~~ 
mysql -u root –p
mysql> FLUSH TABLES WITH READ LOCK;
mysql> quit
~~~
Ahora usaremos la herramienta mysqldump en el servidor principal:
~~~
mysqldump ejemplodb -u root -p > /tmp/ejemplodb.sql
~~~
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica5/copiarBD.PNG?raw=true)
Desbloqueamos las tablas con:
~~~
mysql -u root –p
mysql> UNLOCK TABLES;
mysql> quit
~~~ 
Nos vamos a la máquina1, la esclavo, para realizar la copia del archivo.SQL que contiene los datos salvados desde la máquina principal (la máquina2).
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica5/copiadb.PNG?raw=true)

Creamos la BD en la máquina esclavo
~~~
mysql -u root –p
mysql> CREATE DATABASE ‘ejemplodb’;
mysql> quit
~~~
Y después restauramos los datos contenidos en la BD con:
~~~
mysql -u root -p ejemplodb < /tmp/ejemplodb.sql
~~~

## 3. Replicación de BD mediante una configuración maestro-esclavo

Cambiamos la configuración de mysql del maestro y del esclavo. Para ello editamos el archivo
*/etc/mysql/mysql.conf.d/mysqld.cnf*
Dejaremos las siguientes lineas en este estado:
~~~
#bind-address 127.0.0.1
log_error = /var/log/mysql/error.log
server_id = 1
~~~
En el esclavo es similar exceptuando el identificador del servidor que contendrá el valor 2 en vez de 1. Guardamos la configuración de ambos archivos y reiniciamos los servidores con la orden *sudo service mysql restart*.

En el maestro creamos un usuario y le damos permisos de acceso para la replicación.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica5/crear-usrEsc.PNG?raw=true)
Volvemos a la máquina esclava, entramos en mysql y le damos los datos del maestro. 
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica5/conf-esclavo.PNG?raw=true)
Iniciamos el esclavo con ~~~ mysql> START SLAVE;~~~ y desbloqueamos las tablas para que puedan meterse nuevos datos en el maestro: ~~~mysql> START SLAVE;~~~
Para asegurarnos que todo va bien mostramos el estado del esclavo, si la variable*"“Seconds_Behind_Master"* es distinta de "null" todo va perfectamente.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica5/conf_succ.PNG?raw=true)
Ahora insertamos en la máquina maestro algún dato para revisar que se clona en la esclavo.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica5/comprobacionEsclavo.PNG?raw=true)
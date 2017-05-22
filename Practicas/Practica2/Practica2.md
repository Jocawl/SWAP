# Práctica 2. Clonar la información de un sitio web

## 1. Comprimir la información y copiarla en otra máquina por medio de ssh.
En la máquina que contiene la información que queremos copiar ejecutamos el siguiente comando:
~~~
tar czf - directorio | ssh equipodestino 'cat > ~/tar.tgz'
~~~
En la siguiente imágen tenemos la ejecución del comando en la máquina 2.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica2/tar.PNG?raw=true)

Ahora comprobamos en la máquina 1 que se ha copiado la información.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica2/comprobacionTar.PNG?raw=true)

## 2. Instalando la herramienta rsync
Instalamos en ambas máquinas, si no la tenemos, la herramienta rsync. Podemos hacerlo con el siguiente comando:
~~~
sudo apt-get install rsync
~~~

Como vamos a usar la herramienta en modo usuario, haremos propietario de la carpeta /var/wwww a dicho usuario.
~~~
sudo chown user:user –R /var/www
~~~

Ahora copiamos la carpeta del servidor web de la máquina 1, por ejemplo, para probar su funcionamiento.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica2/rsync.PNG?raw=true)

Y comprobamos que hemos copiado la información de la máquina 1.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica2/ComprobacionRsync.PNG?raw=true)

## 3. Acceso sin contraseña para ssh
En la máquina que queremos usar como respaldo de la principal ejecutamos el siguiente comando:
~~~
ssh-keygen -b 4096 -t rsa
~~~

![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica2/ssh-keygen.PNG?raw=true)


A continuación deberemos copiar la clave pública al equipo remoto (máquina principal)
añadiéndola al fichero ~/.ssh/authorized_keys, que deberá tener permisos 600:
~~~
ssh-copy-id maquina1
~~~

Si el comando anterior nos falla debemos ejecutar el siguiente comando, y volver a ejecutar posteriormente el comando anterior.
~~~
chmod 600 ~/.ssh/authorized_keys
~~~
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica2/CopiaClavePublica.PNG?raw=true)

Como se muestra en la imagen anterior, no hemos tenido problemas al copiar la clave pública en la máquina 1 y hemos podido acceder a ella por ssh desde la máquina 2.

## 4. Programar tareas con crontab
Modificamos el archivo /etc/crontab para añadir una tarea que nos sincronize ambas máquinas cada hora.

![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica2/crontab.PNG?raw=true)

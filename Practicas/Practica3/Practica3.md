# Práctica 3. Balanceo de carga
Lo primero que necesitamos es configurar una nueva máquina virtual y asegurarnos que el puerto 80 no está siendo utilizado, para eso realizaremos una instalación sin añadir apache.

## 1. El servidor web nginx como balanceador
Para realizar la instalación de nginx, he ejecutado las siguientes órdenes:
~~~
sudo apt-get update && sudo apt-get dist-upgrade && sudo apt-get
autoremove

sudo apt-get install nginx
sudo systemctl start nginx
~~~

Una vez se ha instalado correctamente, accederemos a configurarlo como balanceador modificando el fichero */etc/nginx/conf.d/default.conf*.

Borraremos toda la información que contenga ese fichero y lo dejaremos con el contenido siguiente:
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica3/ConfNginx.PNG?raw=true)

Para lanzar el servicio nginx, una vez configurado, ejecutaremos:
~~~
sudo systemctl start nginx
~~~
Para comprobar que funciona correctamente realizamos peticiones sobre la ip del balanceador:
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica3/repartiendocarga.PNG?raw=true)

## 2. Balanceo de carga con haproxy
Antes de instalar haproxy detendremos la ejecución de nginx para que no esté ocupado el puerto 80.
~~~
sudo service nginx stop
~~~
A continuación, ejecutaremos ~~~ sudo apt-get install haproxy~~~ para instalar haproxy. Después procederemos a configurarlo modificando el fichero */etc/haproxy/haproxy.cfg*.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica3/haproxy_conf.PNG?raw=true)

Una vez configurado, lanzamos el servicio haproxy con el comando:
~~~
sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg
~~~
Para comprobar que funciona mandaremos peticiones sobre la ip de la máquina, como hicimos con nginx.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica3/funcionaHaproxy.PNG?raw=true)

## 3. Someter a una alta carga el servidor balanceado
Vamos a someter ambos balanceadores, nginx y haproxy, a una alta carga haciendo uso de la herramienta Apache Benchmark.

Ejecutaremos el siguiente comando desde el anfitrión o desde otra máquina virtual y compararemos los resultados obtenidos.
~~~
ab -n 1000000 -c 100 http://192.168.5.132/hola.html
~~~
Resultado obtenido con haproxy.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica3/ab_haproxy.PNG?raw=true)

Ahora detenemos haproxy, iniciamos nginx y volvemos a ejecutar la herramienta
~~~
service haproxy stop
service nginx start
~~~
Resultado obtenido con nginx.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica3/ab_nginx.PNG?raw=true)
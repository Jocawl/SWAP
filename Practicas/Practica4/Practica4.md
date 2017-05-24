# Práctica 4. Asegurar la granja web
# 1. Instalar un certificado SSL autofirmado para configurar el acceso por HTTPS
Para generar el certificado SSL autofirmado debemos activar el módulo SSL de Apache, generar los certificados y especificar la ruta a los certificados en la configuración.
Ejecutaremos, en modo root, los siguientes comandos:
~~~
a2enmod ssl
service apache2 restart
mkdir /etc/apache2/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout
 /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt
~~~
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica4/instalarcertificado.PNG?raw=true)                                             

Editamos el archivo de configuración del sitio default-ssl:
~~~
nano /etc/apache2/sites-available/default-ssl.conf
~~~
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica4/default-ssl.PNG?raw=true)
A continuación, activamos el sitio default-ssl y reiniciamos apache:
~~~
a2ensite default-ssl
service apache2 reload
~~~
Una vez reiniciado Apache, accedemos al servidor web mediante el protocolo HTTPS.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica4/accesohttps.PNG?raw=true)

## 2. Configuración del cortafuegos
Vamos a borrar la configuración que trae iptables y configurar nosotros mismos el cortafuegos, para ello realizaremos un script en el que escribiremos las reglas que queremos que tenga nuestro cortafuegos.

Lo primero, veremos que el tráfico por HTTP y HTTPS está abierto.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica4/estadoinicial.PNG?raw=true)

Ahora, pondremos una configuración por defecto, que será no aceptar tráfico.
~~~
# (2) establecer las políticas por defecto (denegar todo el tráfico):
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP
~~~
El script para establecer la configuración quedaría:
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica4/conf_iptables.PNG?raw=true)

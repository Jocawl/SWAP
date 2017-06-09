# Práctica 6. Discos en RAID

El objetivo de esta práctica es configurar dos discos en RAID 1 por software. Partiremos de una de las máquinas que ya tenemos con Ubuntu server instalado y le añadiremos, estando apagada, dos discos más con el mismo tamaño (5GB en nuestro caso).

## 1. Configuración del RAID por software
Instalamos, si no lo tenemos ya, el software necesario para configurar el RAID:
~~~
sudo apt-get install mdadm
~~~
Buscamos la información de ambos discos:
~~~
sudo fdisk -l
~~~
Ahora crearemos el RAID 1, usando el dispositivo */dev/md0, indicando el número de dispositivos a usar, dos en nuestro caso, y su ubicacioón:
~~~
sudo mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc
~~~
Una vez creado el dispositivo, le damos formato y creamos el directorio donde montaremos la unidad RAID:
~~~
sudo mkfs /dev/md0

sudo mkdir /dat
sudo mount /dev/md0 /dat
~~~
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica6/dar-formatoMD0.PNG?raw=true)

Comprobamos el estado del RAID con:
~~~
sudo mdadm --detail /dev/md0
~~~
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica6/estadoMD0.PNG?raw=true)

Para finalizar el proceso, conviene configurar el sistema para que monte el dispositivo RAID creado al arrancar el sistema. Para ello debemos editar el archivo */etc/fstab* y añadir la línea correspondiente para montar automáticamente dicho dispositivo.

Obtenemos el UUID del dispositivo md0 y lo añadimos a dicho archivo:
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica6/UUID-md0.PNG?raw=true)

El archivo quedará de la siguiente forma:
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica6/ficheroFSTAB.PNG?raw=true)

Una vez que el dispositivo RAID está funcionando, procederemos a provocar un fallo en uno de los discos.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica6/fallo_disco.PNG?raw=true)

Luego retiramos "en caliente" el disco que está marcado como que ha fallado.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica6/hot_remove.PNG?raw=true)

Y añadiremos, también "en caliente", un nuevo disco para reemplazar al que hemos retirado.
![imagen](https://github.com/Jocawl/SWAP/blob/master/Practicas/Practica6/añadir_disco.PNG?raw=true)
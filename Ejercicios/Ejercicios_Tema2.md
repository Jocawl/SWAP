# Tema 2-Alta disponibilidad y escalabilidad
## Ejercicio T2.1:
### Calcular la disponibilidad del sistema si tenemos dos réplicas de cada elemento(en total 3 elementos en cada subsistema).
Para calcular la disponibilidad de cada elemento del sistema,dando igual las veces que esté replicaco, he usado la fórmula: $As = Ac_{n-1}+((1-Ac_{n_1})*Ac_n)$
Estos son los datos que reflejan la disponibilidad de cada elemento del sistema:

| Componente  | Availability |
| :---------  | :----------: |
| Web         | 85%          |
| Application | 90%          |
| Database    | 99.9%        |
| DNS         | 98%          |
| Firewall    | 85%          |
| Switch	  | 99%          |
| Data Center | 99.99%       |
| ISP         | 95%          |

Y estos la del sistema con todos los elementos, menos el data center, replicados 1 vez:

| Componente  | Availability |
| :---------  | :----------: |
| Web		  | 97.75%       |
| Application | 99%          |
| Database    | 99.9999%     |
| DNS         | 99.96%       |
| Firewall    | 97.75%       |
| Switch	  | 99.99%       |
| Data Center | 99.99%       |
| ISP         | 99.75%       |

La disponibilidad podemos calcularla como: $ As=Ac1*Ac2*...*Acn$. Sin tener elementos repetidos la disponibilidad del sistema sería **59.87%**, al replicar los elementos nos sube la disponibilidad a **79.10%**.

Ahora calcularé la disponibilidad de cada elemento si lo tenemos replicado 2 veces, que quedaría reflejado en la siguiente tabla:

| Componente  | Availability |
| :---------  | :----------: |
| Web		  | 99.6625%     |
| Application | 99.9%        |
| Database    | 100%         |
| DNS         | 99.999%      |
| Firewall    | 99.6625%     |
| Switch	  | 99.9999%     |
| Data Center | 99.99%       |
| ISP         | 99.987%      |

Para terminar el ejercicio, calculamos con la fórmula anterior, la disponibilidad del sistema completo replicado 2 veces, la cuál es **99.20%**

## Ejercicio T2.2:
### Buscar frameworks y librerias para diferentes lenguajes que permitan hacer aplicaciones altamente disponibles con relativa facilidad.

![imagen](https://github.com/Jocawl/SWAP/blob/master/Ejercicios/django.jpg?raw=true)

**Django** es un framework de desarrollo web de código abierto, escrito en Python, que respeta el patrón de diseño conocido como Modelo-vista-controlador. La meta fundamental de Django es facilitar la creación de sitios web complejos. Django pone énfasis en el re-uso, la conectividad y extensibilidad de componentes, el desarrollo rápido y el principio ***No te repitas***.

![imagen2](https://github.com/Jocawl/SWAP/blob/master/Ejercicios/jquery.png?raw=true)

**jQuery** es una biblioteca multiplataforma de JavaScript que permite simplificar la manera de interactuar con los documentos HTML, manipular el árbol DOM, manejar eventos, desarrollar animaciones y agregar interacción con la técnica AJAX a páginas web.

jQuery, al igual que otras bibliotecas, ofrece una serie de funcionalidades basadas en JavaScript que de otra manera requerirían de mucho más código, es decir, con las funciones propias de esta biblioteca se logran grandes resultados en menos tiempo y espacio.

![imagen3](https://github.com/Jocawl/SWAP/blob/master/Ejercicios/angular.png?raw=true)

**AngularJS** nn framework Javascript para el desarrollo de aplicaciones web en el lado del cliente, lanzado por Google. Se utiliza para crear y mantener aplicaciones web de una sola página. su objetivo es aumentar las aplicaciones basadas en navegador con capacidad MVC, en un esfuerzo para hcer el desarrollo más sencillo.
## Ejercicio T2.3:
### ¿Cómo analizar el nivel de carga de cada uno de los subsistemas en el servidor?

## Ejercicio T2.4: 
### Buscar ejemplos de balanceadores software y hardware(productos comerciales).

### Buscar productos comerciales para servidores de aplicaciones.

### Buscar productos comerciales para servidores de almacenamiento.
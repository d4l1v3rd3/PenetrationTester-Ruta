<p align="left"><img height=100px width=100px src="https://github.com/user-attachments/assets/28eba669-a8dd-418a-bc8d-cc7c8e147edc"></p>

<h1 align="center">MODELO OSI</h1>

<p  align="center" ><img width=300px height=400px src="https://github.com/user-attachments/assets/0ed81f3c-4c32-4cc2-9321-b7f40434fbf9"></p>

# ÍNDICE

- [¿QUÉ ES EL MODELO OSI?](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Otros/ModeloOSI.md#qué-es-el-modelo-osi)
- [CAPA 7 APLICACIÓN](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Otros/ModeloOSI.md#capa-7-aplicacion)
- [CAPA 6 PRESENTACIÓN](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Otros/ModeloOSI.md#capa-6-presentacion)
- [CAPA 5 SESIÓN](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Otros/ModeloOSI.md#capa-5-sesion)
- [CAPA 4 TRANSPORTE](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Otros/ModeloOSI.md#capa-4-transporte)
- [CAPA 3 RED](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Otros/ModeloOSI.md#capa-3-red)
- [CAPA 2 ENLACE DE DATOS](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Otros/ModeloOSI.md#capa-2-enlace-de-datos)
- [CAPA 1 FÍSICA](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Otros/ModeloOSI.md#capa-1-física)

## ¿Qué es el modelo OSI?

El modelo OSI (Sistema de interconexión abierto) es fundamental en redes. Este modelo critico provee como funcionar a toda la red, dispositivos, como se envia, recibe e interpreta los datos.

Uno de los beneficios del modelo OSI es que todos los dispotivios pueden tener funciones diferentes y diseños en la red mientras tiene comunicación con otros dispositivos. Los dato se mandan a través de la red y sigue uniformemente el modelo OSI para entenderse entre ellos.

El modelo OSI consiste en capas que se ilustran en un diagrama. Cada capa manda información diferentes y tiene responsabilidades diferentes.

<p align="center" ><img src="https://github.com/user-attachments/assets/cd3b326f-e833-4255-a9c5-7b7dccc66ec7"></p>
 
# CAPA 7 APLICACION

La capa aplicación de el modelo osi es lo más familiar que nos encontramos. POrque todas las aplicaciones tienen este protocolo y sus reglas para determinar como el usuario interactura y recibe y envia datos.

Cada aplicacion que los clientes envian como, emails, navegadores, archivos en red, como filezilla se da gracias a una GUI (graphical user interface) los usuarios interactuan para recibir y enviar datos.

Otros protocolos incluyen DNS (Domain Name System) que nos da un nombre por una ip.

# CAPA 6 PRESENTACION

La capa 6 del modelo osi es donde la estandarización empieza a comenzar. Porque los desarrolladores de softwarte pueden desarrollar cualquier software como un cliente de email o etc.

Esta capa hace que traspasen los datos a la capa de aplicación. Recibiendo el rodenador para que eentienda que la infromación que se le envía la pueda entender el mismo ordenador. Por ejemplo cuando envias un correo, el usuario lo recibe pero el contenido del email es el mismo.

# CAPA 5 SESION

Una vez que los datos se han enviado y traducido correctamnete de la capa 6, la capa 5 crea la conexión entre ordenadores o al destinatario. Cuando la conexión se entabla, se crea una sesion.

La capa 5 sincroniza dos ordenadores para estar seguros de que estan en la misma pagina nates de enviar nada. Una vez que chekea eso, se divide en pequeños chunks de adtos para ir mandandolos (paquetes) todos al mismo tiempo.

Esto es muy beneficioso porque si la conexion se pierde, solo los chunks que faltan se tienen que enviar para que la información llegue.

Los datos se pueden pasar en diferentes sesiones no tiene porque ser unica.

# CAPA 4 TRANSPORTE

La capa 4 del modelo osi es vital para transmitir los datos a traves de la red, puede ser un poco dificl de entender. Cuando los datos se envian entre dispositvios, tenemos dos diferentes protocolos

- TCP
- UDP

Vamos con TCP. Transmission Control Protocol. Esto protocolo esta disechado con garantica. Este protocolo reserva una conexion constante entre dos dispositivos para un tiempo exacto, coge los datos y los recibe.
También incorpora checking de error en el diseño. A si garantiza que los datos se pasan perfectamente

## VENTAJAS:

- Garantia de transpaso de datos
- Sincroniza con dos dispositivos entre ellos
- Fiabilidad

## DESVENTAJAS:

- Requiere la conexion entre dos dispositivos. Si un pequeño chunk no se recibe, todos los datos se quitan.
- Una baja conexion hace que el ordenador tarde mucho mas en recibir los datos.
- TCP es bastante mas lento que UDP porque hace mas trabajo para que todo funcione.

Normalmente se usa para situaciones de archivos, emails. Se usa para completar los daots

![image](https://github.com/user-attachments/assets/a7191feb-978f-4682-afb3-103a7718f05f)

Ahora vamos con UDP (User Datagram Protocol) Este protocolo es como el heramano no avanzado del TCP. En las cosa sque tiene, no te chekear erores y no es tan fiable, todos los datos se envian por UDP lleguen o no. 

## VENTAJAS:

- UDP es mucho más rápido de TCP
- UDP sale de la capa de aplicacion como software y decide como va a enviar los paquetes
- UDP no quita la conexión aunque no sea continua

## DESVENTAJAS:

- UDP no le importa si llega o no los datos
- No es nada flexibe para los desarrolladores de software
- En conexiones con mala calidad es una experiencia horrible para el usuario.

![image](https://github.com/user-attachments/assets/22003126-1c78-48c0-94e4-5efb806276fa)

UDP esta bien en muchas situaciones para mandar paquetes pequeños. Por ejemplo, los protocolos como ARP o DHCP pero para archivos o vide streaming.

# CAPA 3 RED

La capa numero 3 del modelo osi (capa de red) es donde la magia con el router y el asambleaje de datos tiene lugar (de los pequeños chunks a los largos), el routing simplemente determina la forma mas optima de transmitir ese tipo de datos.

Algunos protocolos de las capas determinan exactamente la forma más optima o la ruta para mandar los datos y recibirlos, deberiamos saber como se crea en el modelo de red, estos protocolos incluyen OSPF (Open Shortest Path First) y RIP (Routing Information Protocol) El factor decide como ruta es cogido  y decidida siguiento esto:

- Que ruta es la más corta? Los menos dispositivos por los que los paquetes tienen que pasar para llegar
- Que ruta es más rentable? Se pierden paquetes llendo por algun lado?
- Que ruta esta fisicamente conectada) Conexión de cobre (lento) Fibra (rápido)

En esta capa, todo funciona con la IP como 192.168.1.100. Los router son capaces de identificarse por la IP.

# CAPA 2 ENLACE DE DATOS

La capa de enlace de datos utiliza una transmision fisica por dirección. Recibe los paquetes de al red incluyendo la ip del ordenador o dispositivo, y añade una dirección física (MAC) (Media Acess Control) Dentro de cada red de ordenadores hay un NIC (Network Interface Card) que da una MAC unica.

La MAC la ponen los creadores para identificarla, no se puede cambiar (si que se puede) cuando la información en la red es fisica se manda la información

Adicionalmente, el trabajo entre presentar los datos y la transmisión la hace esta capa también.

# CAPA 1 FÍSICA

Esta capa es la más facil de entender, porque simplemente es la conexión entre componentes fisicos. Como dispositivos electricos o etc. 

![image](https://github.com/user-attachments/assets/c3e2cf0c-3bcf-4a50-9add-895394e2363e)

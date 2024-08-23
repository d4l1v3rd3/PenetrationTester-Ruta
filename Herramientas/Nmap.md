<p align="left"><img height=100px width=100px src="https://github.com/user-attachments/assets/28eba669-a8dd-418a-bc8d-cc7c8e147edc"></p>

<h1 align="center">NMAP</h1>

<p align="center"><img src="https://github.com/user-attachments/assets/5e7bad69-1e3e-4057-92db-bdbb3b6aab6f"></p>

# ÍNDICE

- [INTRODUCCIÓN](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/Nmap.md#introducción)
- [SUBREDES](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/Nmap.md#subredes)
- [TCP FLAGS]()
- [ENUMERAR TARGET](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/Nmap.md#enumerar-target)
- [DESCUBRIR HOST ACTIVOS](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/Nmap.md#descubrir-hosts-activos)
- [MASSCAN](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/Nmap.md#masscan)
- [REVERSE DNS](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/Nmap.md#usar-reverse-dns)
- [SPOOFING Y DECOYS]()
- [DETECCIÓN DE SERVICIOS]()
- []()


# INTRODUCCIÓN

Cuando queremos atacar una red, lo primero que haremos es buscar una herramienta eficiente que nos responda a esto: 

- Que sistemas están encendidos?
- Que servicios estan funcionando en ese sistema?

Lo dividiremos en 4 partes:

- Descubrimiento de host
- Scaneo de puertos básicos
- Scaneo de puertos avanzado
- Post escaneo de puertos.

1 - ARP SCAN : Este escaneo usa consultas ARP para descubrir hosts activos.
2 - ICMP SCAN : Usa consultars ICMP para identificar host activos.
3 - TCP/UDP SCAN : Manda paquetes TCP a los puertos o UDP para determinar que hosts estan activos.

Introduciremos dos escaneos, "arp-scan" y "masscan" 

Nmap fue creado por Gordon Lyon, un experto en ciberseguridad y programador de código abierto. En 1997. Nmap es código abierto y bajo la licencia GPL. Es una herramienta estandarizada para mapear redes, identificar host activos, y descubirr servicios que estan corriendo.

Los scripts de Nmap funcionan dependienco la funcionalidad, pudiendo buscar vulnerabilidades. Usualmente Nmap funciona a sí:

![image](https://github.com/user-attachments/assets/c2d79b85-0668-484c-87b5-c708ff2e55a8)

# SUBREDES

Vamos a ver el termino y como funcionan las redes. Simplemente es un segmento de la red, un grupo de ordenadores conectados usando un medio. Normalmente un cable Ethernet con un Switch o un AP de Wifi. 

Tenemos una IP de red, una subnetwork y usualmente es la equivalencia a una o mas segementos de red conectados entre ellos y configurados por el mismo router.

Estos segmentos se refieren a una instalación fisica, mientras que una subred es una conexión lógica.

![image](https://github.com/user-attachments/assets/0ed08d74-17cd-44c3-a4be-9f15a3e73f61)

Como vemos en la figura tiene dos tipos de subnets:

- Subnet con /16 con una mascara de 255.255.0.0
- Subnet de /24 con una mascara de 255.255.255.0

Si queremos entender como funcionan un poco más estudiar como funciona las LANS

Una parte de reconocerlo, es descubir la infromación de los hosts sobre la subnet. Si tu estas conectado en la misma subnet, el escaner de ARP (Address Resolution Protocol) descubre host activos. Requiere conseguir la MAC para la comuniocacion sea posible.

Si tu Network es tipo A, podemos usar solo ARP apra descubrir toda la subnet. Suponiendo que son targets diferentes, todos los paquetes pasar por el router que es el "gateway"

# TCP FLAGS

Nmap soporta diferentes tipos de escaneo TCP. Para entender la diferencia entre otros escaneo de puertos, nosotros necesitamos entender la cabecera TCP. Las cabeceras tienen 24 bytes en el primer segmento. Para seguir esta fgura RFC 793. 

![image](https://github.com/user-attachments/assets/33793e8a-f470-4d54-8c41-542596fe10a6)

- URG : Ugent flag indica el punto de los datos que vienen urgentes, este segmento TCP con la flag URG procesa inmediatamente y envia los datos.
- ACK : Acknowledgement flag indica en numero significante.
- PSH : Push flag pregunta al TCP para pasar los datos a la aplicación.
- RST : Reset flag se usa pra resetear la consexión. Otro dispositivo, como el firewall, vanda conexiones TCP se usa cuando los datos quieren enviarse al host y no hay servicio.
- WYN: Synchronize flag se inicializa en la fase 3 con un apreton de manos y secuencia de numero con otro host.
- FIN : No se envia mas datos.


# ENUMERAR TARGET

Antes de empezar a enumerar un target, deberemos especificar como queremos escanear. Generalmente hablando, puedes dar una lista, un rango o una subnet. Especificar el target

- list : IP_máquina scanme.nmap.org example.com (Escanea 3 IPS)
- range : 10.11.12.15-20
- subnet : IP_máquina/30

También podemos dar un arhicov con una lista de objetivos

```
nmap -iL lista.tst
```

Si queremos chekear la lista de hosts que hemos escaneado, podemos usar "nmap -sL target" esta opción nos da la lista del escaneo que ha hecho, sin embargo, nmap hace un reverse-DNS para todos los targes que consigue sus nombres.

Dando valiosa información al Pentester (SI queremos que no haga esto añadimos -n)

# DESCUBRIR HOSTS ACTIVOS

Vamos a volver a ver las capas TCP/IP empezamos por como usuarlas

- ARCP la capa de enlace
- ICP de la capa de red
- TCP de la capa de transporte
- UDP de la capa de transporte

![image](https://github.com/user-attachments/assets/5c2be6e2-69ed-4397-be28-a12fe886ca15)

Antes de discutir como hacer escaneos debemos detallar, y entender los cuatro protocolos. ARP se usa por un proposito: 

Mandar un parquete a la dirección broadcast en un segmeto de red preguntarndo al rodenador con la dirección ip especifica y el respondiendo con la mAC

ICP tiene muchos tipos. puede usar (echo) o (ECHO reply)

Si queremos hacer un ping en la misma subnet, utilizaremos mejor ARP que ICP

OTra cosa es TCP y UCP los transportes, para propositos de escaneo de red, un escaner puede mandar y crear paquetes especificos de TCP o UDP puertos y checkear como responden los targets. Este metodo es eficiente cuando los paquetes ICMP Echo estan bloqueados.

## DESCUBRIMIENTO DE HOST USANDO ARP

Imagina que queires saber los hosts que hay y que estan funcionando. Lo esencial es utilizar tiempo para escanear y también hosts online y que no esten en uso. Hay varias formas de descubrir estos hosts

1 - Necesitaremos privilegios para escanear en una local network siendo "root" o "sudo"
2 - Cuando los usuarios priviligeados intentar escanear fuera de la res, Nmap usa ICP, TCP ACk por el puerto 80, TCP SYN para sincronizar por el puerto 443.
3 - Cuando el usuario no tiene privilegios se intenta escanear fuera de lar ed, utiliza 3 tipos de TCP mandando paquetes SYN del puerto 80 y 443

Nmap por defecto usa ping para escanear los host activos, procede a escanear despues de elo. SI tu quieres descubrir hosts online sin necesidad del puerto, podemos usar 
```
nmap -sn target
```

Los escaneos ARP son posibles si tu estas en la misma subred de los target. por Ethernet o Wifi, necesitamos saber la MAC de los sistemas que queremos comunicarnos. La MAC es necesarioa porque linkea la cabeza, la cabecera contiene la informacion de la MAC de destino.

Para conseguir la MAC es necesario mandar peticion ARP al sistema operativo, solo funciona si estamos en la misma subnet

```
nmap -PR -sn target
```
Donde -pr indica que solo quieres paquetes ARP. 

En este caso por ejemplo una ip "10.10.210.7" usa paquetes ARP para descburir los host de la misma subred. Manda peticiones a los ordeandores, y los devuelve el mismo

![image](https://github.com/user-attachments/assets/ee596508-5496-428e-b323-a531a308af90)

Podemos ver si quremeos ver como los paquetes se generar por "Wireshark" viendo el trafico de red, viendo la MAC inico a la destino.

![image](https://github.com/user-attachments/assets/f358af19-410d-4b68-8ac2-01d98bb0b052)

Hablando de los paqutes ARP podemos mencionar que existen este tipo directamente "arp-scan" esto da muchas funciones

```
arp-scan --localnet
arp-scan -l
sudo arp-scan -I eth0 -l
```

## DESCUBRIMIENTO DE HOST POR ICMP

Si quisieramos podriamos hacer un ping a cada IP de la red y ver como responde a este ping, simple no?

No siempre es lo mejor, porque muchos firewall bloquean ip

Pra usar ICMP echo y descubrir hosts podemos añadir "-PE" y recordar "-sn" si no queremos un puerto especifico. 

![image](https://github.com/user-attachments/assets/e64f2953-f8f4-4efd-a7e9-dc5631e1e797)

En el ejemplo, vamos a escanear una red
```
nmap -PE -sn ip/24
```
Esto mandara paquetes ICMP y lo devolveran dando a sí la MAC, generalmente hablan entre sí, no te saldrá la MAC a no ser que estemos en la misma subred. 

Podemos hacer el mismo escaneo pero sin que nos salga la mAC
```
sudo nmap -PE -sn 10.10.68.220/24
```

Si tu miras los pquetes con wireshar veras paquetes con protocolo ICP y su respuesta dentro de la subnet

![image](https://github.com/user-attachments/assets/8bc3b0a8-62fa-461f-887b-fbdac270839f)

Porque los paquetes ICMP suelen ser bloqueados, se puede utilizar "-PP" para tardar un tiempo entre pings

![image](https://github.com/user-attachments/assets/8a1da0ff-0a3d-418f-9f97-0cc6ee37d7dc)

```
nmap -PP -sn ip
```

Similar a la otra saldran en wireshar paquetes ICMP

Otro tipo seria por mascara de red con "-PM" 

![image](https://github.com/user-attachments/assets/b1b41d5b-6c49-42fa-8488-bafdab574634)

Esto intenta descubrir los hosts con la marca de red, utilizamos
```
nmap -PM -sn ip
```
nosotros sabemos que el host esta archivo, y  nos dvuelve que no. es necesario aprender que muchos paquetes pueden ser bloqueados y no por ello esta inactivo

pero podemos ver por Whireshark que tenemos respuesta aunque este bloqueado.

## DESCUBRIMIENTO DE HOST USANDO TCP Y UDP

Podemos mandar paquetes SYN (sincronizados) por el puerto TCP, 80 y esperar una respueta. Un puerto abierto suele ser con SYN/ACK y un puerto cerrado RST. EN este caso chekearemos si tenemos una respuesta. ESpecficando el peurto que queremos

![image](https://github.com/user-attachments/assets/8862bd30-2511-44a9-a088-a7b5b38c2844)

Si queremos que namp use TCP SYN ping deberemos utilizar la opción "-PS" siguiente el numero de puerto, rango lista o combinación. Por ejemplo "-PS21" mientras que "-PS21-25" coge todos esos puertos
También podemos puerto por puerto "PS80,443,8080"

Los usuarios privlegiados pueden andar paquetes y no tienen que completar el handhsake si el puerto esta abierto

![image](https://github.com/user-attachments/assets/3b1e29e9-6b76-40c9-a03f-49d5726549fc)

```
nmap -PS -sn ip
```

Por whireshar veremos TCP

![image](https://github.com/user-attachments/assets/0870146f-7d24-440d-98fe-4b6f98f9478d)

### TCP ACK PING

Esto manda un paquete ACK usando los privilegios de nmap y si no es privilegiado lo manda 3 veces

El puerto por defecto es el 80 es igual que el anterior "-PA" 

![image](https://github.com/user-attachments/assets/8a8b455c-bcbe-465b-a720-2ba7dc98cbab)

```
sudo nmap -PA -sn
```
![image](https://github.com/user-attachments/assets/9c27cb45-f074-4444-9385-70315d1ac6d9)

### UDP PING

Lo contrario a TCP es UDP y podemos hacer exactamente lo mismo y mirar target avaliable

![image](https://github.com/user-attachments/assets/23d7c77e-c648-47d4-9b79-4e44fc3b3b14)

La sintaxis es la misma
```
sudo nmap -PU -sn ip
```

![image](https://github.com/user-attachments/assets/77e711e1-f3f2-46e3-a214-effbfa894535)

## NULL SCAN

El null scan no envia ninguna flag, "-sN" manda paquetes tcp sin flags a puertos abiertos

![image](https://github.com/user-attachments/assets/c8009824-ce01-49bc-b8af-8b7aedda79fb)

Siembargo si el servidor responde con con RST el puerto esta cerrado. 

![image](https://github.com/user-attachments/assets/735adcfe-0ce3-458f-adf5-2176959199da)

## FIN SCAN

El scaneo FIN manda paquetes TCP con una flag FIN. POdemos elegir con "-sF" similar a un paquete TCP abierto, 

![image](https://github.com/user-attachments/assets/c0a5e57d-2d82-47bf-b62f-d7e6ac509cca)

![image](https://github.com/user-attachments/assets/7d144c0f-bde6-4d18-9315-8af599c27dc1)

## XMAS SCAN

El escaneo Xman, coge las flags FIN, PSH y URG simultaneamente. "-sX"

Como un null scan y fin scan, recibe los paquetes RST y mira si el puert oesta cerrado

![image](https://github.com/user-attachments/assets/ad070e54-8740-4950-9862-0651c765d4fe)

![image](https://github.com/user-attachments/assets/117409d5-e92b-408f-9d08-78d8ea69869a)


# SPOOFING Y DECOYS

EN muchas redes, podemos escanear dispositivos usando la ip y spoofearla y igual con la MAC. Un escaneo puede ser beneficioso para garantizar las capturas. Si intentamos escanear un dispositivo random de la red usando el spoofeo de ip,  las chances son que no nos respondera y no tendremos buenos resultados
```
nmap -S spoofed_id 10.10.66.241
```
Nmap craftea paquetes usando la ip "spofeada" y la target para responde paquetes. Para este escaneo necesitamos monitorizar el escaneo de red para saber si llega

![image](https://github.com/user-attachments/assets/d135ba6f-e4b9-422e-a437-7b791ec0ae90)

1. El atacante manda paquetes con la ip spoofeada (robada) ala ip target
2. La target devuelve los paquetes a la ip spoofeada
3. El atacante captura los paquetes enviados a la ip y ve los puertos

4. EN general esperamos para especificar una red "-e" y para quitar el escaneo de ping "-Pn"

```
nmap -S spoofed_Ip ip
```

```
nmap -e Net-interface -Pn -S spoofe_ip ip
```

Si estas en la misma subnec y quieres especificar algo 
```
--spoof-mac spoofed_mac
```
![image](https://github.com/user-attachments/assets/aad9620e-91d7-4641-95df-ae677752f2f3)



### MASSCAN

Masscan se usa igual que nmap, sin embargo para abar el escaneo de la red, hay que pararlo masscan es mas agresivo

```
masscan ip -p443
mascan ip -p80,443
masscan ip --top-ports 100
```

```
apt install masscan
```

## USAR REVERSE-DNS

Nmap por defecto siempre hace un DNS reverso. POrque los hostnames revelan mucho y esto puede ayudar. si no queremos hacerlo "-n" 

Por defecto mira los hosts activos y podemos usar "-R" para mandar tambien a los hosts desconectado si queremos usar un dns especifico 
```
--dns-servers DNS_server
```

![image](https://github.com/user-attachments/assets/8a9a9719-f5b6-4c9a-9cd1-10a5f41c9b18)

![image](https://github.com/user-attachments/assets/3d4282f7-3803-4070-8069-5e9a009ce05e)

# UDP SCAN

EL protocolo UDP, no necesita un handshake para la conexión ganarantizando que no tiene porque responder al estar desactivado para hacer este protocolo utilizaremos

```
nmap -sU ip
```

![image](https://github.com/user-attachments/assets/6cd2b8c4-24b5-44c6-8478-cbe4782871ab)

![image](https://github.com/user-attachments/assets/967f11f2-75a5-4189-88ee-336875dfa89e)

# ZOMBIE SCAN

Spoofeando la ip podemos conseguir mucha información. Sin embargo, solo funciona para redes especificas. Requiere que el monitorizar la red, etc. Habiendo limitaciones

El escaneo zombie, requiere conexión a lared. Practicamente buscas un host zombie, que te indice y lo spoofeas. 

```
nmap -sI zombie_ip ip
```

![image](https://github.com/user-attachments/assets/4b9492ac-97c1-4448-b6f4-7cab60c3172e)

![image](https://github.com/user-attachments/assets/56ffbe45-2934-4ff9-9e1d-db459f24b1a2)

# DETECCIÓN DE SERVICIOS

Una vez hemos descubierto los puertos por Nmap, tenemos la disponibilidad de detectar los servicios que corren.

Añadiendo "-sV" en el nMap nos da información sobre la versión de los puertos abiertos. Podemos controlar la intensidad con "--version-intensity LEVEL" donde los rangos son del 0-9

Lo vas completo es 

```
-sv --verison-light
-sv --version-all
```

# DETECCIÓN SISTEMA OPERATIVO

Nmap puede detectar el (OS) gracias a "-O"

```
nmap -sS -O ip
```

El sistema escanea los sistemas operativos y su kernel.

## Traceroute

Si queremos buscar los routers que hay entre el target y tu añadimos "--traceroute" 

```
nmap -sS --traceroute ip
```
## SCRIPTS

Todos que tenemos nmaps, tenemos una carpeta de /scripts en el que nos encontramos con:

![image](https://github.com/user-attachments/assets/93dc1730-8f56-4d0b-87d4-d14ba7d0ad77)

```
nmap --scripts=default o -sC
```
Si por ejemplo encontramos un ftp y queremos buscar algo de ahi 

```
--script "ftp*"
```

# ESPECIFICACIONES

- Listado de puert : -p22,80,443
- Rango de puertos : -p1-1203
- Todos los puertos : -p- (65535)
- Más comunes : -F (100 puertos más comúnes)
- Tiempo de escaneo : -T<0-5>
  - paranoid (0)
  - sneaky (1)
  - polite (2)
  - normal (3)
  - aggresive (4)
  - insane (5)
-Control de paquetes : --min-rate=20

![image](https://github.com/user-attachments/assets/05065722-b6b1-41d4-b11c-e5be555a1546)

Gracias a --reason nos especifica como ha encontrado los puertos 

![image](https://github.com/user-attachments/assets/d83531ec-b5e7-4dbd-9d0f-f1301362b461)

![image](https://github.com/user-attachments/assets/c2ea0cb9-ecef-4554-9b27-e63abb36acaf)

![image](https://github.com/user-attachments/assets/aa0e76c7-0433-4652-9bbd-524c9cc6f111)


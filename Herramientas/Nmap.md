#by d4l1

<p align="center"><img src=""></p>

<h1 align="center">NMAP</h1>

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

## SUBREDES

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

## ENUMERAR TARGET

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

## DESCUBRIR HOSTS ACTIVOS

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
Esto mandara paquetes ICP y lo devolveran danddo a sí la MAC





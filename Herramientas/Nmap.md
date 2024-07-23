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




# Introducción

Whireshark es un open-source, una cross-platform de red teniendo la capacidad de leer paquetes de red teniendo la capacidad de snifear y investigar trafico real y inspectar paquetes de red (PCAP).

Es comunmente usado siendo uno de los mejores paquetes de analisis. Veremos lo básico de Wireshark y como se usa como paquete fundamental de analisis.

# Descripción General

Whireshar es un potente analista de trafico. Tiene multiples propositos:

- Detectando problemas de red, como una red tiene fallos de carga puntos y congestión.
- Detectando anomalias de seguridad, como rango de host, usos anormales de puertos, trafico sospechoso.
- Investigar protocolos y aprender detalles, Ver codigos de respuesta y datos de payload.

Wirehsar no es un IDS (intrusion detection System). Nos deja a nosotros como analistar descubrir e investigar paquetes. No podemos modificar.

## Interfaz y datos

Nombre | Descripción
--- | ---
Barra de herramientas |La barra de tareas contiene los menus y atajos de teclado para el snifeo de paquetes, procesamiento, filtrado, etc.
Archivos recientes | Lista de los arhicvos listando o investigados anteiromente
Captura por filtros y interfaces | Caputa de filtros y puntos por los que puedes sniffear (interfaces de red) (eth0 ens33)
Barra de tareas | estado, perfil, paquetes de informacion

![image](https://github.com/user-attachments/assets/609b9af9-4cd7-44b8-a67e-28feb978f374)

## Cargar archivos PCAP

```
File - dropping file
```

Cuando metemos un archivo en mi caso el hhp1.pcapng

![image](https://github.com/user-attachments/assets/9ae8fbd6-b266-4bbe-b856-85630437f8c4)

Podemos ver numeros, paquetes, detalles etc. Vamos ver formatos:

- Lista de paquetes : Es el sumario de todos los paquetes (soruce and destination, protocol y packet info) si vemos o clickamos en la lista podemos investigar en cada paquete
- Destalles panel : Detalles del protocolo usado para mandar el paquete
- Bytes : Los paquetes se manda en bytes para ello wireshark los representa en código ASCII.

## Color de los paquetes

Whireshar, utiliza paquetes de colores para ordenar o condicionar diferentes protocolos y anomalias en cada caputr.

Esto nos ayuda sobre lo que esta pasando por e analisis. Podemos crear reglas de color o eventos dependiendo de los filtros usados.

Whireshar tiene dos tipos de paquetes o metodos de color: Reglas temporales que solo duren para la sesion en cuestión o reglas permanentes.

```
menu - view - coloring rules - create
```

![image](https://github.com/user-attachments/assets/060d8515-8d80-44e5-a2d2-4053db018eed)

## Sniffeo de tráfico

Podemos pulsar en el boton de "shark" ![image](https://github.com/user-attachments/assets/daef7b55-7e9e-4658-ad99-bccfcaa597af)

Para empezar con la busqueda de paquetes y pulsar en el boton de "stop" para parar hay empezaremos a recolectar paquetes de red.

## Unir paquetes de red

Whireshar tiene la capacidad de unir dos paquetes "pcap" en uno solo 

```
file - merge
```

## Ver detalles de un fichero

Conocer los detalles es muy necesario, especialmente cuando trabajamos con muchos archivos "pcap" aveces nos da información de detalles (como hash, tiempo de captura, comentarios, inferfaz y estadisticas

```
statistics - Capture File properties - pcap icon located on the left bottom
```

# Disección de paquetes

La disección de paquetes es investigar los detalles y decodificaciones posibles de los protocolos de los archivos. Hay una larga lista de protocolos.

## Detalles de paquetes

Puedes clicar en un paquete en la lista y abrir los deatlles

![image](https://github.com/user-attachments/assets/a80d9962-d6c6-4027-9496-bdc8ff4622bf)

Los paquetes consisten en 5 a 7 capas basados en el modelo OSI. Vamos aver un paquete HTTP

![image](https://github.com/user-attachments/assets/cd820cbb-3280-4b0a-90f7-b7a8e5590f09)

Podemos ver todas las capatas 

- Frame (capa 1): Nos enseña todos los detalles de un paquete en la capa física del modelo OSI

![image](https://github.com/user-attachments/assets/7a3c2543-3e78-469e-86bd-29e782e2e19e)

- Source [MAC] (Capa 2): Nos enseña el codigo y la destinación de la MAC.

![image](https://github.com/user-attachments/assets/1c20554e-f71a-495b-9b3b-91ff20764a3f)

- Source [IP] (Capa 3): Nos dive el destino de la Ipv4 de la capa de red

![image](https://github.com/user-attachments/assets/8963d5b8-06d9-44d8-96cd-da145ce5770d)

- Protocol (Capa 4): Nos dice los detalles del protocolo (UDP/TCP) y los puertos de destino, capa transporte

 ![image](https://github.com/user-attachments/assets/466e15e2-515c-423d-bf4b-3a890f356dcd)

- Protocol errors
- Application Protocol (capa 5): No enseña el protocollo usado, HTTP, FTP, SMB, capa aplicación

![image](https://github.com/user-attachments/assets/92c24b24-596e-4f81-b1b6-0577cffc3335)

# Navegación entre paquetes

Whireshark calcula el numero de los paquetes investigados asumiento un único numero por paquete. Esto ayuda a los analistas a procesar grandes capturas de paquetes y volviendo fácilmente a cualqueir punto

## Ir al paquete

Si nos dirigimos a 

```
go - go to packet
```

Podemos dirigirnos exactamente al paquete o frame en cuestión

## Buscar paquetes

Otro apratado es 
```
edit - find packet
```

Nos aseguramos de encontra dicho paquete obbuscar por un paquete particular por ejemplo que ponga "download"

## Marcar paquetes

Marcar paquete nos ayuda a buscar un punto especifico

```
edit - mark/unmark packets
```

![image](https://github.com/user-attachments/assets/8581a189-4618-497c-8b12-4e04a3d28b61)

## Comentario de paquetes

Similar al marking de antes

![image](https://github.com/user-attachments/assets/bb13cd94-d1a0-4fe1-b652-7e9609cd9d8d)

## Exportar paquetes

Los archivos que contienen miles de paquetes en un simple archivo, lo podemos exportar o exportar paquetes especificos

```
file - export
```

![image](https://github.com/user-attachments/assets/77abd405-beed-4724-8183-28022cd7c044)

## Exportar objetcos (archivos)

Wireshar extrae archivos para analisis de seguridad, es vital descubrir archivos que hemos guardado para una investigación

## Formato de tiempo

Wireshar lista paquetes que estan capturados, esa investigación es una bunea opción

```
view - time display format
```

![image](https://github.com/user-attachments/assets/d5519c45-d139-441a-9545-d85446f252b4)

## Información adicional

Wireshark detecta dependiendo que protocolos y anomalias o problemas. Pudiendo ser falsos positivos o negativos

![image](https://github.com/user-attachments/assets/ba241543-6453-4860-96aa-529a8b34a651)

Go to packet number 39765
Look at the "packet details pane". Right-click on the JPEG section and "Export packet bytes". This is an alternative way of extracting data from a capture file. What is the MD5 hash value of extracted image?

# Filtrado de paquetes

Whireshark tiene un un motor de filteros

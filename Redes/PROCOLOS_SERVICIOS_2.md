<h1 align="center"> PROTOCOLOS Y SERVICIOS 2 </h1>

# INTRODUCCIÓN

En este apartado veremos como funcionan dichos protocolos y como aprovecharnos de ello como atacantes

1. Ataque de Sniffing
2. Man-in-the-middle
3. Password Attack
4. Vulnerabilidades

En la perspectiva de seguridad, deberemos pensar como protegernos, considerando la triada de la seguridad.

CONFIDENCIALIDAD, INTEGRIDAD Y DISPONIBILIDAD (CIA) 

- Confidencialidad : Se refiere a que el contenido y la comunicación solo puede ser accesible por las partes interesadas.
- Integridad : La idea que de los datos mandados, sean consistentes y esten completos en la llegada.
- Diponibilidad: Se refiere a que el servicio que necesitemos este accesible.

![image](https://github.com/user-attachments/assets/e66068bc-2e2c-450d-9dbd-4575869111f9)

# SNIFFING ATTACK

Este aaaque refiere el uso de la red para la captura de paquetes, recolectando información sobre el target. Cuando el protocolo se comunica en texto claro, los datos se intercambian y se capturan en terceras partes para analizarlos. Una simple red que captura paquetes y recibe información relevante.

Un ataque de sniffeo se puede producir usando Ethernet teniendo los permisos necesarios.

1. TCPDUMP : open source CLi
2. Whireshark
3. Tshark

Especializadas en capturar contraseñas o mensajes.

Considera que accedes a los emails por POP3 puedes conseguir todoo

```
sudo tcpdump port 110 -A
```

![image](https://github.com/user-attachments/assets/b8ebbb6a-dcbf-4db2-9750-31efbae04faa)

![image](https://github.com/user-attachments/assets/41929c9f-b598-476f-b3b8-3f04e7156c15)

# MAN-IN-THE-MIDDLE

Este ataque ocurre cuando la victima que comunida con alguien y en medio hay un atacante.

![image](https://github.com/user-attachments/assets/210dd782-1585-4b7e-9f57-2aed8a13fc8c)

Este ataque es relativamente simple porque solo necesitas estar en medio y ver la información que se traspasa se puede utilizar Ettercap o Bettercap.

Necesitas texto claro como un FTP,SMTP y POP3

# TRANSPORT LAYER SECURITY

En esta tarea, aprenderemos sobre una solución rapida de confidencialidad y integridad de los paquetes. Por ejemplo para protegerse contra ataques "MITM" 

SSL (Secure Sockets Layer) empieza con WWW para las nuevas aplicaciones, para aplicaciones de compra. 

Todos los protocolos comunes iban en texto claro, haciendo posible la captura de apquetes.

![image](https://github.com/user-attachments/assets/a35675a3-cf31-4766-aab5-5334d98a0acf)

Ahora hay una conexxion ente todo esto haciendolo más seguro y por ello ahora se le llaman HTTPS, SFTP, FTPS

![image](https://github.com/user-attachments/assets/9753f378-8305-4199-a694-d328dc8a3254)

1- Entablar conexion TCP al servidor web
Mandar consutlas HTTp

![image](https://github.com/user-attachments/assets/062ecf55-3fd5-49c2-9683-2b31eb4d983e)

# SSH

Secure SHell (SSH) crea una forma segura para controlar administrativamente un sistema remoto. 

1. Confirmas la identidad
2. Cambias mensajes encriptados
3. Modificas y detectas

Para usar SSH necesitamos un usuario y contraseña y escucha en el puerto 22

```
ssh mark@ip
```

![image](https://github.com/user-attachments/assets/86923be5-2f93-48f7-b386-4ac1a04d43fe)

![image](https://github.com/user-attachments/assets/102028b6-d661-41b1-b5d4-17af2276eaa6)

En un MIT no podrías ver nada. (si no tienes la clave pública)

Podemos usarlo para descargar archivos por SCP (Secure Copy Protocol)

```
scp mark@10.10.44.74:/home/mark/archive.tar.gz ~
```

![image](https://github.com/user-attachments/assets/81bbd455-5c01-425a-9059-c34ac3932fef)

# PASSWORD ATTACK

Nosotros vimos como funcionan los ataques por MITM y como se pueden utilzar en un SSH ahora iremos por los ataque sde contraseña

Muchos protocolos necesitan autenticarse. Como POP3 o HTTPS.

![image](https://github.com/user-attachments/assets/67888565-aea9-4fb5-a9e8-6e611cb0ade7)

Si tenemos un diccionario, o un generador o un ataque por fuerza bruta.

como un rockyou podemos utilziar hydra

```
hydra -l username -P wordlist.txt server service
```

- -l useranme:
- -P wordlist
- server
- service

```
  hydra -l mark -P /usr/share/wordlists/rockyou.txt 10.10.44.74 ftp
  hydra -l mark -P /usr/share/wordlists/rockyou.txt ftp://10.10.44.74
hydra -l frank -P /usr/share/wordlists/rockyou.txt 10.10.44.74 ssh
```



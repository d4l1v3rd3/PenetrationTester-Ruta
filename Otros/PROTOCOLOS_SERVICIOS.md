<h1 align="center"> PROTOCOLOS Y SERVICIOS  </h1>

# INTRODUCCIÓN

Veremos que es un protocolo para que esta diseñado, que hace y en que nivel trabaja, como nos comunicamos, como entendemos la GUI, etc.

# TELNET

EL protocolo Telnet funciona en nivel aplicacion conectandose virtualemtne a la terminal de un ordenador. Usando Telent, el usuario se logea a otro ordenador por terminal (consola) para ejecutar programas. Pudiendo hacer tareas administrativas remotamente.

Telnet es relativamente simple. EL usuario se conecta, le dice un usuario y contraseña. Si la autenticación es correca, accedes remotamente al terminal. La conexión entre los dos ordenador no va encriptada, siendo targets muy fáciles para los atacantes.

Un servidor Telnet escucha en el puerto 23.

1. Primero, le pregunta el username
2. Luego la contraseña
3. El sistema chekea
4. Dentro de la terminal

![image](https://github.com/user-attachments/assets/299ebfea-cc29-4939-a310-8a28534a28e6)

![image](https://github.com/user-attachments/assets/17b8f71a-0c46-499d-8422-19903d0b29b0)

# HTTP

Hypertext Transfer Protocol (HTTP) se usa para transferir web. Se conecta  a un servidor web usando una consulta HTTP. 

El cliente manda una consulta al "index.html" , y luego le devuelve todos los archivos necesarios.

![image](https://github.com/user-attachments/assets/ebb10b6f-eb24-41cc-827f-cb2775545f0b)

HTTP manda y recibe datos no encriptados, como Telnet o Netcat para comunicarse con el "web browser" 

En el sieguiente ejemplo, vamosa  ahcer una consulta y vamos a intentar contectar usando telnet al puerto 80

```
telnet ip 80
```

![image](https://github.com/user-attachments/assets/f958b686-2769-483e-b410-d90e090a0653)

Las opciones populares que utilizan HTTP:

- Apache
- IIS
- Nginx

Navegadores web:

- Chrome by Google
- Edge by Microsoft
- Firefox by Mozilla
- Safari by Apple

# FTP

File transfer Protocol (FTP) se usa para transferir archivos entre ordenadores o sistemas eficientes.

FTP solo manda y devuelve datos en texto claro, se puede usar Telnet (netcat) para comunicarnos con FT.

1. Nos conectamos al servidor FTP usando cliente telnet. El servidor FTP escucha en el puerto 21.
2. Ponermos un usuario "USER frank"
3. Una contraseña "PASS 231"
4. Y ya logeados

![image](https://github.com/user-attachments/assets/2e66f140-f279-4a2c-a2dc-27a88b7909d2)

![image](https://github.com/user-attachments/assets/ae5ceaa5-5cca-4b63-bff7-e421402665aa)

Para descargar archivos
```
get archivo
```

Software que se usa:

- vsftpd
- ProFTPD
- uFTP

# SMTP

Se usa para los servicios de conexión de Email en Internet. Hay varias configuaraciones para los servidores, pero simplemente tu envias un email o si te envian tenemos un exchanges que accede a internet y de ahi los desacrgamos.

Tiene estos componentes:

1. MSA (mail submission Agent)
2. MTA (Mail Tranfer Agent)
3. MDA (Mail Delivery Agent)
4. MUA (Mail User Agent)

![image](https://github.com/user-attachments/assets/e2126c30-feec-41b6-a9df-736aefd92a1c)

1. MUA, como cliente, manda un mensaje y se conecta al MSA para mandar el mensaje
2. EL MSA recibe el mensaje, chekea errores y transfiere al servidor MTA
3. EL MTA manda el mensaje al mismo recipiente del MTA y lo envia al MSA
4. El MTA hace como MDA
5. EL recipiente del uusario es el MDA

![image](https://github.com/user-attachments/assets/a00b2f39-6f89-45d6-b387-e711b0cd12a5)

```
mail from:
rcpt to:
```
# POP3

Post Office Protocol version 3 (POP3) Se usa para descargar mensajes de email del MDA server, el cliente se conecta al servidor POP3, se autentifica y se descarga los mensajes o no.

![image](https://github.com/user-attachments/assets/ac4cf79a-e7a8-4363-8a43-dfc7e9ebe473)

El ejemplo vemos que la sesion POP3 se conecta por Telnet. Primero el usuario se conecta al puerto por defecto (110) se autentifica con usuario y contraseña y lo mira.

![image](https://github.com/user-attachments/assets/171081e4-6428-4479-971d-68d02ec40ede)

# IMAP

Internet Message Acess Protocol (IMAP) es mas sofisticado que el POP3. IMAP hace posible la sincronizacion entre el email de muchos clientes y el servidor.

![image](https://github.com/user-attachments/assets/e772c0a4-3c79-4a0f-b877-0960da0b1dd3)

![image](https://github.com/user-attachments/assets/87aad763-1a47-47ba-8b39-63de89802cc3)




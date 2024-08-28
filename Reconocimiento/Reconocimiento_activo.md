<h1 align="center">Reconocimiento Activo </h1>

<p align="center"><img src="https://github.com/user-attachments/assets/f3e1d9fb-6a6d-4880-8ffe-c70cecad9b2e"></p>

# ÍNDICE

# INTRODUCCIÓN

En este apartado nos centraremos en herramientas como "ping" "traceroute" "telnet" "nc" para conseguir información de la red, sistemas y servicios.

El reconocimiento activo requite que haya contacto con el target. Este contacto puede ser por telefono, visitandolo, alguna pertenencia etc. (Ingenieria Social)

Checkear puertos como el ssh, firewall.

Información de los clientes que navegan por laIP, tiempo de conexión, duración de conexión y otras cosas. Sin embargo no todas las conexiónes son sospechosas. Es posible hacer reconocimiento con la actividad del cliente.

Considerando que el navegador tiene muchisimos usuarios que entran diariamente. 

En esta rom aprenderemos herramientas en el navegador, framework y las herramientas anteriormente mencionadas.

# NAVEGADOR WEB

El navegador web es una herramienta muy buena.

El navegador web se conecta en:

- TCP puerto 80, HTTP
- TCP puerto 443, HTTPS

Son los puertos predefinidos de las web pero podriamos tener web es "http://127.0.0.1:8334 (localhost) o diferentes.

Mientras estamos navegando podemos pulsar "Ctrl+Shift+I" para abrir las herramientas de desarrollador, viendo ficheros, cookies, etc..

![image](https://github.com/user-attachments/assets/371deed2-4661-447a-99e2-edb931d66c7e)

Addons interesantes para Firefox o google.

- FoxyProxy: Servidor proxy que se usa para acceder a una web.
- User-Agent Switcher and Manager: Disponibilidad de acceder a una web con un sistema operativo o navegador, por ejemplo iniciar con un iphone o etc.
- Wappalyzer: Nos da información sobre las tecnicas que se utilizan en la web. Como extensiones, frameworks etc.

![image](https://github.com/user-attachments/assets/3ef7a05a-7831-486b-bb7c-e1133b95e409)

# PING

Ping, se usa para chekear si tenemos conectividad en otra red o dispositivo, pero se puede utilizar para mas cosas.

Simplemente manda un paquete a un sistema remoto y el se lo devuelve.

```
ping ip
ping -c 10 ip
ping -n 10 ip
```

(Internet Control Message Protocol) ICMP 

![image](https://github.com/user-attachments/assets/e066ff07-00db-4106-8757-11c49caf74cd)

En el ejemplo vemos cositas, como los milisegundos que tarda

![image](https://github.com/user-attachments/assets/afe4527a-a8bc-4489-9e72-b23bdada5319)

En este caso no tenemos conectividad

# TRACEROUTE

El comando "traceroute" hace simplemente "traces the route" coge los paquetes del sistema a otro host. EL proposito es encontrar la IP de los router para ver a donde llegan los paquetes. Este comando revela el numero de router que hay entre los dos sistemas. Es interesante ver el numero de saltos (router" entre el sistema y el target.

Sin embargo, puede cambiar mucho

```
traceroute ip
```

No es la forma de directamente de descubrir la ruta. Pero nos puede ayudar. Nos fijamos en el (TTL) es la cabecera de las IPS. dice el tiempo, indica el numero maximo de router/hops que psan.

Cuando el route recibe el parque, el TTL incrementa antes del siguiente router. El valor del TTL es 1 inicialmente y su valor pasa de 64 o 60 dependiendo de los routers.

![image](https://github.com/user-attachments/assets/017ce158-caaa-4fb9-9acb-3a9c91875271)

Sin embargo el ttl llega a 0 se descartara, se enciara un mensaje ICMP al remitente. 

El sistema ttl establece en 1 antes de enviarlo al router. EL primer router de la ruta disminuye el ttl en 1 y da como resultado 0

![image](https://github.com/user-attachments/assets/a940e5e1-a481-4613-b0b5-02354af3bfa0)

En Linux , "traceroute" manda paquetes UDP causando que el primer orute sea TTL=0 y manda ICMP hasta reverla la IP

![image](https://github.com/user-attachments/assets/3f5a2e2c-622d-4562-a130-b8ad18d6a699)

Tenemos 14 lineas que reperesentan cada salto. EL sistema manda 3 paquetes con un tll 1 y esos paquetes se conviernet 

![image](https://github.com/user-attachments/assets/a453adf4-5dcd-4122-a9a3-bb27e60872aa)

# TELNET

TELNET (teletype Network) desarrollado en 1969 para la comunicación entre sistemas remotos via linea de cmoandos (CLI), el comando "telnet" usa el dicho protocolo. El puerto por defecto es el 23. Para una perpectiva de seguridad, telnet manda todos los datos, inclutendo usuario y contraseña, en texto claro.

Para eso se creo "SSH"

Sin embargo, telnet, es simple, se usa para otros propositos

```
telnet ip puerto
```

Vamos a descubrir información, escuchando en el puerto 80 nos queremos comunicar con el protocolo HTTP. 

![image](https://github.com/user-attachments/assets/3334d949-8d7f-4f60-9afa-f235dc64dab2)

# NETCAT

Netcat o simplemente "nc" es un conjunto de aplicaciones diferentes con valor en penteset. Netcar soporta los protocolos TCP y UDP. Esta funcion hace que se pueda conectar a puertos abiertos. 

```
nc ip puerto
```

![image](https://github.com/user-attachments/assets/50d433df-9082-4bc5-8cc5-2fe39b3e2135)

En el terminal podemos ver el puerto usado el header etc.

![image](https://github.com/user-attachments/assets/23408bdc-03ea-4137-b768-0d77035e422a)

# SUMARIo

![image](https://github.com/user-attachments/assets/831310f7-2b0a-4a1c-9e55-687ad297a206)

![image](https://github.com/user-attachments/assets/33bc2003-b66e-4352-9075-d0c50b8bce1c)


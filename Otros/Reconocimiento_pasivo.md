<h1 align="center">RECONOCIMIENTO PASIVO</h1>




# ÍNDICE

# INTRODUCCIÓN

En esta sección definiremos el reconocimiento pasivo , nos centraremos en las herramientas esenciales de dicho reconocimiento. Nosotros aprenderemos los tres comandos más importantes:

- whois : Consulta a los servidores "WHOIS"
- nslookup : Consulta a los servidores "DNS"
- dig : Consulta a los servidores "DNS"

Todos estos datos son publicos y no alertar al target.

- DNSDumpster
- Shodan.io

Antes de nada deberemos conocer a nuesro objetivo.

En el reconocimiento pasivo, tenemos que ver todo el conocimiento público que este disponible. Accedemos a recursos publicos o directamente hablamos con el target. 

![image](https://github.com/user-attachments/assets/8925f4b3-2616-4e4e-a75c-a272a03bafeb)


- Mirar DNS o dominios publicos
- Mirar anuncion trabajo website, etc
- Leer noticias sobre el target.


# WHOIS

WHOIS es un protocolo de consulta y respuesta que sigue las expecificaciones "RFC 3912". El servidor de WHOIS esta en escucha por el puerto 43 (TCP). El dominio se registra por ellos mismos. 

Información que obtenemos:

- Regsitrar : Registro
- Contacto o información del registro: Nombre, organización, dirección, numero, etc.
- Creación, Actualización y datos expirados: Cuando se registro el dominio, cuando se actualizo y cuando se tiene que renovar.
- Name Server: Nombre que tiene para resolver dicho dominio.

Para conseguir esta información, nosotros necesitamos usar como cliente "whois". Muchos servicios proveen información whois, sin embargo generalmente es mucho mas rapido y conveniente utilizar su mismo cliente. 

```
whois nombre
```

![image](https://github.com/user-attachments/assets/96036972-b599-4bbb-99af-ec1d2944c534)

Podemso ver bastante información, primero su nombre con su información. el mantenimiento, de donde esta el dominio.

Información sobre el registro, contacto, nombre, privacidad, etc..

# NSLOOKUP Y DIG

Encontrar la ip del dominio usando "nslookup" 

```
nslookup nombre_dominio
```

![image](https://github.com/user-attachments/assets/ddee41e1-f8c1-45bf-aaa8-22f6d811898a)

```
nslookup -type=A tryhackme.com 1.1.1.1

nslookup -type=a tryhackme.com 1.1.1.1
```
![image](https://github.com/user-attachments/assets/1b5a6869-7f31-4498-bfef-93af26586018)

Los archivos A y AAAA devuelven IPv4 y IPv6, respectivamente. Estos lookup nos ayuda mucho a los penetration tester. Nosotros con el nombre de dominio obtenemos la IPv4.

```
nslookup -type=MX tryhackme.com
```
![image](https://github.com/user-attachments/assets/349603b8-b2cc-4bb4-85aa-d04b8775eedf)

Vemos el email en uso que usa con configuración de móvil. MX busca los Mail exchange servers, le decimos al servidor de mail que vengan de @tryhackme, y nos conectamos y vemos los validos.

Google da una lista de servidores de correo, esperamos que ninguno de estos servidores tenga vulnerables, sin embargo suelen siempre estar seguros.

Otra pieza de información para continuar con el reconocimiento es "-type=txt" que nos da otro tipo de información.

Para DNS mas avanzados tenemos la funcionalidad de "dig" "Domain Information Groper" 

```
dig dominio MX
```
![image](https://github.com/user-attachments/assets/5bc23874-4ee7-4df9-806c-70cd18f90d07)

En comparación con "nslookup" "dig" nos devuelve otra información.

# DNSDumpster

Las herramientas de DNS, como nslookup y dig, puede buscar subdominmios. El dominio inspeccionado obviamente inclute diferentes subdominios con buena información. 

Aprenderemos a conseguir información de ellos. Hay posibilidad de que estos dominios no se actualicen regularmente.

Utilizamores [DNSDumpster](https://dnsdumpster.com). Si buscamos cualquier dominiio en este motor, nos dara información del DNS y tablas con diferente información.

![image](https://github.com/user-attachments/assets/0bdf80a3-06f8-4554-9b65-066ae763d10e)

También nos puede representar la información graficamente. 

![image](https://github.com/user-attachments/assets/e9d9a6ed-bae1-4e4f-8cb5-e52a297e631e)

# SHODAN.IO

Cuando empezamos una tarea como penetration tester contra targets especificos, utilizamos mucho [shodan.io](https://www.shodan.io) nos ayuda dandonos información sobre los clientes de lared, como es de activa etc.

Shodan intenta conectar con cada dispositivo que este conectado a la red, a la diferencia que otros motores de búsqueda. Recoleta información y nos dice el servicio base de datos, etc.

![image](https://github.com/user-attachments/assets/470efd89-ed05-497a-825c-d99388ed49ad)

Nos devuelve el servidor web, etc.

- Ip
- Compañia de host
- Localización geográfica
- Tipo de servidor y su versión

# SUMARIO

![image](https://github.com/user-attachments/assets/3f1e30d6-686f-4c84-b8d3-bb8f89d7ba34)



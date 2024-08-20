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


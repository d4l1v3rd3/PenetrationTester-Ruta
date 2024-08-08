<h1>INTRODUCCIÓN AL HACKING WEB</h1>

# ENTEDER UNA APLICACIÓN

En este apartado entenderemos como ver una aplicación web manualmente y encontrar fallos de seguridad utilizando herramientas del navegador. Más tarde, herramientas automaticas.

- Ver código
- Inspeccionar elementos de página
- Ispeccionas elemento JS
- Red

## EXPLORAR LA PÁGINA WEB

Como penetration tester, nuestro rol es ver como funciona una página o una aplicación web, haciendo que descubramos potenciales vulnerabilidades y podamos probar exploits.

Buscando pequeñas partes de una web podemos saber rutas absolutas por defecto. Por ejemplo:

![image](https://github.com/user-attachments/assets/0067d98c-6145-433f-a83c-626d462683a4)

## VER EL CÓDIGO DE LA PÁGINA

El código fuente es un codigo que un humano es capaz de leer.

El código se hace normalmente en HTML (HyperText Markup Language), CSS (Cascading Style Sheets) y JavaScript, y le dice al navegador como se va a ver al usuario, como quieres que se vean los elementos interactivos con JavaScript.

### ¿Como ver la código de la página?

- Mientras veamos la web y pulsemos "View Page Source"
- Muchos navegadores puedes hacer un "view-source:ip"
- En tu navegador dentro de las herramientas de navegador ver la código.

### VEAMOS EL CÓDIGO

![image](https://github.com/user-attachments/assets/a920850e-2027-43e3-ab9f-d2df9c346937)

### HERRAMIENTAS DE DISEÑADOR - INSPECTOR

HERRAMIENTSA DE DISEÑADOR

TOdos los navegadores incluyen herramientas de diseñador, esta herramienta se hace para que los desarrolladores puedan debugear estar aplicaciones web y ver que esta pasando el la página web, nosotros como penetrarion tester tenemos que entender la aplicación.

ABRIR LAS HERRAMIENTAS

Simplemente "VIew Site" 

INSPECTOR

![image](https://github.com/user-attachments/assets/e39a3ac4-3e30-4fb4-8e9d-7dcc3e079c64)

El código de la pagina no tiene porque siempre represnetar la pagina web, esto esp porque CSS, JS pueden interactuar para cambair el contenido y estilo de la página, necesitas ver esto para saber lo que esta pasando exactamente o en el tiempo. E

El elemento Inspector te asiste a darte una presentacion en el momento, lo que este pasando en la web. 

Es bueno ver una pagina web en el momento, porque podemos editar e interactuar con los elementos, ayudando bastante.

### HERRAMIENTAS DE DISEÑADOR - DEBUGGER

En el panel del desarrolador tenemos un debugger de JS, y es excelente para ver el código de JS. En google es "Sources"

![image](https://github.com/user-attachments/assets/88727970-7d7e-40d1-90c9-ef10103cd002)

![image](https://github.com/user-attachments/assets/31d268fd-c86a-431d-9ea1-fdb6938ca859)

### HERRAMIENTAS DE DISEÑADOR - NETWORK

Las herramientas de diseñador que se usan ver la trazabilidad de los paquetes que se hacen al mandar una consulta en una web. Si clicamos en la pagina web y refrescamos veremos todo lo que hace.

![image](https://github.com/user-attachments/assets/438408e3-454c-4575-aafc-f0f045d5cebd)

# DESCUBRIR CONTENIDO

Antes de anda en el contexto de la seguridad en las aplicaciones web, que es el contenido? El contenido son muchas cosas, archivos, videos, imagenes, backups etc.

Cuando hablamos del contenido y descubrirlo, no hablamos de lo que vemos en la página web, queremos referirnos a lo que no tenemos acceso publico.

Hay muchas formas de conseguir dicho contenido. Manualmente, automatizado o con OSINT.

## DESCUBRIMIENTO MANUAL - ROBOTS.TXT

EN muchos sitios manualmente podemos mirar el archivo "Robots.txt"

Este documento nos dice las paginas que no quieren enseñar en los motores de busqueda o que los banee especificamente.

Estás páginas suelen ser portales de administración o archvios de los customers de la página. El archivo contiene una lista de localziaciones de la pagina que no quieren que vea un penetration tester.

![image](https://github.com/user-attachments/assets/c1ba30e4-afe0-4257-a1b3-8f9ea624745e)

## DESCUBRIMIENTO MANUAL - FAVICON

EL favicon es el pequeño icono que sale en la dirección del navegador de la página 

![image](https://github.com/user-attachments/assets/04f72bbc-c2a8-48ad-b7f3-3c2fd8d9bda5)

A veces cuando utilizamos frameworks para crear la web, el favicon es una parte que se instala desde fuera, y el desarrollador no cambia o customiza, OWASP tiene una base de datos con los frameworks iconos que debemos chekear a nuestros targets.

[OSWAP DATA BASE](https://wiki.owasp.org/index.php/OWASP_favicon_database)

![image](https://github.com/user-attachments/assets/e0e62038-9629-46a1-b621-0ccb1ee1866a)

## DESCUBRIMIENTO MANUAL - Sitemap.xml

Igual que el archivo robots.txt, restringe que los motores de busqueda se puedan ver, dando una lista de todos los archivos de la web listandos en el motos de busqueda. Aveces contiene areas que la web son más dificiles de encontrar o antiguas páginas.

![image](https://github.com/user-attachments/assets/bb24a7e7-b8fe-46c0-8036-dc543f59743d)

## DESCUBRIMIENTO MANUAL - CABECERAS HTTP

Cuando hacemos una consulta en una pagina web o servidor, el servidor nos devuelve cabeceras HTTP. Estas cabeceras contienen información util para que el software del servidor web y lenguajes.

```
curl ip -v
```

![image](https://github.com/user-attachments/assets/0cea508a-8142-426e-b3c8-9f2f4f28ee1f)

## DESCUBRIMIENTO MANUAL - STACK FRAMEWORK

Una vez sabemos el framework que utiliza la web, como el ejemplo del favicon o mirando los sources de la web, copyright, creditos o noticias, podemos localizar el framework.

De aqui podemos aprender sobre el software y su información.

## OSINT - GOOGLE HACKING / DORKING

Hay muchos recursos disponibles sobre descubrir infromación sobre un target, referidos del OSINT, recolectar información

GOOGLE HACKING / DORKING utiliza el motor de busqueda avanzado de google, customizando la ocnsulta, para ver un dominio exacto (site:pepino.com) o ver terminos exactos (site:pepino.com admin) 

![image](https://github.com/user-attachments/assets/ff4b0b42-c1d3-4382-927c-b228c21ccb59)

## OSINT - WAPPALYZER

Wappalyzer es una herramienta en linea y una extensión del navegador que nos ayuda a identificar con que tecnologias estan trabajando las paginas webs, como framewokrs, CMS, procesadroes de pago , etc..

## OSINT - WAYBACK MACHINE

La Wayback machine (https://archive.org/web/)  es un archivo historico de todas las webs antes de los 90s. Puedes buscar el dominio, y ver como se veia el servicio y los contenidos

## OSINT - GITHUB

Para entender GitHUB, deberemos entender GIT. GIt es una version de control del sistema hace que puedas ver los cambios de las lineas de los projectos. Trabajar en equipo mucho mas facil porque puedes editar y ver lo editado.

Cuando los usuariosh acer cambios, los commits, repositorios etc.

Githbus es hosteado en una vesion de GIT en internet. Los repositorios pueden ser privados o publicos y tienes muchas formas de acceder y de controlar

Podemos utilizar github para buscar una complañia o nombres y asi sacar informacion.

## OSINT - S3 BUCKETS

S3 Buckets se guardan y los proveedores son Amazon AWS; mucha gente guarda archivos y contenido estatico de las paginas web para acceder sobre HTTP o HTTPS.

EL creador de los archivos tiene acceso o permisos para poder hacer archivos publicos o privados. Aveces para acceder a dichos archivos se puede poner que nunca sean publicos.

El formato de S3 es https://name.s3.amazonaws.com donde el nombre lo decide el creador.

S3 Buckets se pueden ahcer muchas cosas, buscar urls, codigo, repositorios, automatizar procesos, etc.


## DESCUBRIMIENTO AUTOMATICO

Una herramienta utomatizada descubre contenido pero no manualmente.

Este proceso es automatizado porque lo hace por 100 o 1000 o 100000 de consultas a un servidor web, las chekea y te dice si existe o no

Una wordlist es un texto claro que se usa para muchas cosas, contraseñas, contenido, directorios, dominios, etc.. Una famosa es la SecList

Las herramientas automaticas hay muchas como, ffuf,ditb y gobuster
```
ffuf -w rutawordlist -u url
```
```
dirb http://ip ruta
```
```
gobuster dir -uurl http://ip -w ruta
```

# ENUMERACION DE SUBDOMINIOS

La enumeración de subdominios es el proceso para buscar subdominios validos dentro de un dominio, pero porque se hace esto? Simplemente intentamos agrandar la superficie para buscar vulnerabilidades en otrosp untos.

Exploraremos tres diferentes formas de enumerar: fuerza bruta, OSINT y virtual host

## OSINT - CERTIFICADOS SSL/TLS 

Cuando un SSL/TLS (Secure socket layer/Transport layer security) se crear para un dominio por una entidad de certficiacion (CA).

Hay los publicos creados para un dominio. EL proposito de estos certificados es parar a los malos para que las páginas no seguras no tengan este tipo de certificados.

Nosotros usamos este servicio como ventaja para descubrir subdominios a traves de sitios como "https://crt.sh" o ui.ctsearch son bases de datos que te dicen los certificados y todos los resultados historicos.

[crt](https://crt.sh)

![image](https://github.com/user-attachments/assets/4669d951-2d08-41e7-8c0a-cbb139037328)

## OSINT - MOTORES DE BUSQUEDA

Los motores de búsqueda contienen trillones de links a muchas paginas web, esto es un excelente recurso para buscar nuevos subdominios.

Usar avanzados metodos de busqeuda como en google, con filtros como site:, puede ayudarnos mucho 

```
site:www.domain.com
site:*.tryhackme.com
```

## DNS FUERZA BRUTA

Hacer fuerza bruta por DNS es el metodo de enumeracion que intenta, por 100 o por 10000000 diferentes subdominios posibles predefinidos en una lista que se usa para definirlos.

Este metodo necesita muchas consultas, nosotros automatizamos la herramienta para que haga el proceso rapido y sencillo.

## OSINT - Sublist3r

Para aumentar el proceso del descubrimiento del subdominio por OSINT, nostros utilizamos metros que nos ayuda la lista de herramientas como "Sublist3r" 

## VIRTUAL HOSTS

Muchos subdominios no estas hosteados publicamente accesibles a los DNS, muchas versiones de desarrollador de la web o aplicacion o el portal del administrador, el DNS tiene DNS privados que se ponen en el famoso archivo /etc/hosts referiendose a la IP y dominio.

Porque los servidores web tienen multiples webs del servidor que pueden hacer consultas los usuarios, los servidores que quiere el cliente se le llaman los Host header. Nosotros utilizamos esto mismo para monitorear las respuestas y descubrir nuevas websites

Como el DNS de fuerza bruta, automatiza el proceso con una wordlists esto es lo mismo.

```
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP
```

- El parámetro -w : especifica la wordlist que queremos usar
- El parámetro -H : añade o edita la cabecera "Host header"
- FUZZ : es el espacio donde debería ir el subdominio.

```
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.224.132 -fs {size}
```

- El parámetro -fs : se usa para ocultar resultados como el tamaño de la página o palabras que contiene.

# BYPASS DE AUTENTICACIÓN



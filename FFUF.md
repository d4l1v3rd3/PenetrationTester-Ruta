# INICIO
En este apartado estudiaremos la herramienta "ffuf" con la que podremos escanear directorios, extensiones, vhosts, php, valores, etc.

ffuf nos provee de un uso automatico para una página web, su lista, las respuestas, etc.

## WEB FUZZING

Aprenderemos lo primero a buscar directorios de una pagina web. 
Nos encontramos con una pagina web sin link y que no nos da ningun tipo de información, para ello la opción "fuzz"

## FUZZING

El temrino "fuzzing" se refiere aun tipo de tecnica que manda diferentes tipos de "input" para certificar como la intefaz interactua.
Si queremos utilizarlo para "SQL Injection" mandara aleatoriamente paquetes para ver como reacciona el servidor.
Normalmente se usan palabras predefinidias "wordlist" que se utilizan comunmente en las páginas web.

## WORDLISTS

Es muy similar a un diccionario de contraseñas, simplemente son las paginas mas utilizadas, normalmente se utiliza el repositorio [Seclists](https://github.com/danielmiessler/SecLists)
Puede estar ya en "/opt/useful/Seclists" y directorios y ficheros como "directory-list"

```
locate directory-list-2.3-small.txt
```

# FUZZING DE DIRECTORIOS
Vamos ahora a usar "ffuf" para buscar directorios en las páginas web.

## FUF

```
apt install ffuf -y
```
en caso de necesitar ayuda
```
ffuf -h
```
## DIRECTORY FUZZING
Una vez que sabemos esto necesitamos saber las opciones mas utilizadas "-w" para wordlist y "-u" para la URL.
Nosotros podemos asignar donde queremos que se utilice la wordlist utilizando "FUZZ" - añadiendo ":FUZZ"
```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ
```
Una vez que sabemos esto podemos cambiar la palabra "FUZZ" hasta en la URL
```
ffuf -w <SNIP> -u http://SERVER_IP:PORT/FUZZ
```
Así sería el comando final
```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ
```
Y podremos ver como "ffuf" testea mas de 90mil URLS en 10 segundos (dependiendo la potencia)

También podemos hacer que vaya mas rapido redirigiendo las peticiones "-t 200"

## PAGE FUZZING
Una vez entendido como funcionan las wordlist y las llaves deberemos aprender a localizar las paginas.

### EXTENSION FUZZING

Dentro de la web podriamos buscar que tipos de paginas se usan, (.html, .aspx, .php)
Una forma de identificar que tipo de servidor es, por las respuestas, por ejemplo, si es apache tendra php pero si es IIS utilizara .asp o .aspx

Para ello podemos utilizar la herramienta de extension de ffuf
```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt:FUZZ <SNIP>
```
Antes de empezar con el escaneo, deberemos indicar que tipo de extension queremos utilizar alfinal.
Nosotros podemos tener mas de dos llaves de palabra, sin embargo solo una extension
```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/blog/indexFUZZ
```
## PAGE FUZZING

Ahora que sabemos el concepto de keywords podemos utilizaf ffuf, para usar la extension .php, cambiaremos el FUZZ para utilizarla como extension.

```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php
```
## FUZZING RECURSIVO
Ahora sabemos directorios e extensiones, ahora deberemos hacer un recursivo, aunque tarde mucho mas.

### RECURSIVE FLAGS
Cuando iniciamos recursivamente automaticamente escanea todo y por debajo de ellos.
Se utiliza el argumento "-recursion" o especificar "-recursion-depth" o "-recursion-depth 1" para elegir cuantos directorios querriamos.
Cuando usamos recursion podemos especificar la extension "-e .php"

## RECURSIVE SCANNING

```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v
```
Con este escaneo tardariamos muchisimo pero es algo normal.

# DNS RECORDS

Ahora vamos a entender como funciona el DNS y porque aveces nos queremos conectar a una página pero no llegamos a nada, simplemente es porque en el directorio /etc/hosts no esta el DNS como publico para el servidor que nos queremos conectar.

# SUB-DOMAIN FUZZING
En esta sección aprederemos los subdominio (*.website.com)

## SUB-DOMAINS
Un sub-dominio es una página por debajo del dominio principal como "https://photos.google.com" de "google.com"
En este simple caso deberiamos checkear si esta en un DNS público.
En el repositorio SecLists hay bastantes especificaciones de estos como "/opt/useful/SeclLists/Discovery/DNS" y ficheros como "subdomains"
```
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.inlanefreight.com/
```
Ahora intentemos con "academy.htb" 
```
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://FUZZ.academy.htb/
```
Si no nos da nada simplemente no tiene subdominios. O que no son publicos.

# VHOST FUZZING
En la anteiorr sección aprendimos sobre el DNS, sobre los subdominios,cuando el DNS no es publico deberemos buscar y aprender sobre los Vhost Fuzzing.

## VHOST VS SUBDOMAINS
La gran diferencia ente esos dos esque el subdominio pertenece a la misma IP y el Vhost puede tener diferente o pertenecer a otra.

!!! LOS VHOST NO TIENEN PORQUE TNEER UN DNS PUBLICO !!!
En muchos casos los subdominios no son publicos, pero no podriamos visitarlos con el navegador y deberiamos identificarlo, la difercia esque si hicieramos un scaneo de vhost y podremos identificar los publicos y no publicos.

## VHOSTS FUZZING
Para escanear Vhost sin necesidad de añadirlos a /etc/hsots, podemos añadir los HTTP headers y especificar el "Host:" usando el argumento "-H" 
```
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb'
```
En caso de que pudiera ser otro puerto "http://academy.htb:port"

# FILTRAR RESULTADOS
Los resultados se filtran automaticamente por HTTP, siendo los códigos conocidos como 404 o 200.

## FILTROS
Gracias a ello tenemos la opcion de poder filtrarlos, por tamaño, palabras, codigo, etc.
En este caso imaginamos que queremos solo filtrar por tamaño y sabemos 900 entonces sería añadir el argumento "-fs 900"
```
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs 900
```
No nos olvidemos de añadir al DNS

## PARAMETRO GET EN FUZZING
Si nosotros hacemos un ffuf recursivo a una página encontraremos por ejemplo un admin.php y puede que no tengamos acceso por identificacion.
En caso de nosotros necesitar logearnos podemos utilizar el argumento "GET"

### GET REQUEST
Similar con los otros apartador , deberemos aprender este metodo en una URL usualmente utilizamos el simbolo "?"
```
http://admin.academy.htb:PORT/admin/admin.php?param1=key.
```
La diferencia sería remplazar el "param1" a "FUZZ" y empezar el escaneo normalmente dicha ruta esta en "opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt."

```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx
```
Sabiendo esto nos iremos a la URL y probaremos

## PARAMETRO POST EN FUZZING
La diferencia entre estos dos parametros, esque con el POST no lo podremos meter en la URL y necesitaremos campos como "data" 
Para añadir datos a ffuf utilizaremos el argumento "-d" y añadir "-X POST"
```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```
```
curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'
```
## VALUE FUZZING
Después de entender los dos parametros, necesitaremos leer lo que nos dice.
## CUSTOM WORDLIST
Si nosotros no queremos utilizar una wordlist ya creada predefinidamente, podemos nosotros agregar o quitar valores.
```
for i in $(seq 1 1000); do echo $i >> ids.txt; done
```
```
ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```
Esto lo utilizariamos para logearnos a una cuenta por ejemplo.

posteriormente deberemos hacer un curl









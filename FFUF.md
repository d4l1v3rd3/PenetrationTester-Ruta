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





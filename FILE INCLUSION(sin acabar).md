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

Por ejemplo una palciacion web descarga un avatar si nosotros ahcemos un codigo malicioso podriamos cambiar el archivo para que se guarda localmente y se ejecutara lo que nosotros queremoes.





# Introducción

Cross-site Scripting (XSS) es una de las vulnerabilidades más comúnes de las apliaciones web hoy en día. Injectan script maliciosos para ser ejecutados desde el navegador web. 

# Objetivos de aprendizaje

- Reflected XSS
- Stored XSS
- Dom-Based XDD
- Como protegerse de un XSS

# Terminología y tipos

Normalmente lo que se bypassea es el "Same-Origin Policy" (SOP) es un mecanismo de seguridad de los navegadores modernos para prevenir estos ataques y obtener información sensible.

## JS para XSS

Es necesario un conocimiento básico de JS para entender los exploits de XSS. Los XSS se atacan desde un lado cliente con un navegador web.

Por ejemplo si nos vamos a las herramientas del navegador, podemos probar cositas

![image](https://github.com/user-attachments/assets/7775beca-04d2-4b60-b79b-7eb332be19cd)

![image](https://github.com/user-attachments/assets/4c5be4f0-7f31-43a0-9dbd-0187eb0b4bdb)

## Tipos de XSS

- Reflected XSS: Este ataque se basa en el control del input como usuario, si queremos buscar un termino en particular y el resultado de la página nos sale eso significa "reflejado", y nos confirma que esta este ataque.
- Stored XSS: Este ataque se basa en almacenar el input en la web. Por ejemplo, si el usuario escribe una review de un producto se guarda en la base de datos y sale para los demás usuarios.
- Dom-based XSS: Este ataque explota vulnerabilidades relacionadas a los objetos modelos del documentos (DOM) para manipular páginas sin la necesidad de que se guarde o refleje.

# Causas de un XSS

Hay muchas razones porque se encuentran este tipo de vulnerabilidades en la web, abajo un lista

### Validación y sanitazción

Las aplicaciones que aceptan datos de usuario como formularios, o páginas dinamicas de HTML, pueden contener script malicioso y para ello hay que sanitizar y validar el input bien

### Falta de codificación

Los usuarios que utilizan caracteres para alterar el proceso de una web. Como <> o " o ', esto puede ser un caso de vulnerabilidad XSS

### Headers de seguridad impropios

Por ejemplo, una mala configuración de CSP, da politicas permisisvas para usar lineas o directivdas que hagan más fácil esto

### Frameworks y vulnerabilidades de lenguaje

### Librerias

# Implicaciones de XSS

- Sesion hijacking
- Phishing
- Ingenieria social
- Manipulación de contenido
- Filtración de datos
- Instalación de malware

# Reflected XSS

Este tipo de vulnerabilidad, donde el código malicioso esta reflejado en el navegador, via una URL crafteada o por un formulario. Considerando la busqueda conteniendo por ejemplo un

```
<script>alert(document.cookie)</script>
```

## Aplicación Web vulnerable

Por ejemplo un input

```
<script>alert('XSS')</script>
```

HTML-Encoded

```
&lt;script&gt;alert('XSS')&lt;/script&gt;.
```

## PHP

Código vulnerable

```
<?php
$search_query = $_GET['q'];
echo "<p>You searched for: $search_query</p>";
?>
```

Si no eres muy familiar con el $_GET contiene la variable de la URL. el parametro 'q' se refiere al parametro. Por ejemplo

http://Shop.net/search.php?q=table

La vulnerabilidad de causa porque el resultado de la página no esta sanitizado. El atacante puede añadir código malicioso a la URL, sabiendo que ha ejecutado. Por ejemplo, una prueba de concepto, es seguir la url

```
http://shop.thm/search.php?q=<script>alert(document.cookie)</script>
```

## Arreglar el código

```
<?php
$search_query = $_GET['q'];
$escaped_search_query = htmlspecialchars($search_query);
echo "<p>You searched for: $escaped_search_query</p>";
?>
```

Con esto todos los carácteres especiales son reemplazados y no ejecutados

# Stored XSS

El atacante injecta codigo malicioso dentro de la web. La web procesa los datos y los comenta, otros usuarios pueden acceder a dicho contenido

```
// Storing user comment
$comment = $_POST['comment'];
mysqli_query($conn, "INSERT INTO comments (comment) VALUES ('$comment')");

// Displaying user comment
$result = mysqli_query($conn, "SELECT comment FROM comments");
while ($row = mysqli_fetch_assoc($result)) {
    echo $row['comment'];
}
```

Como vemos el XSS y el SQL estan conectados sin sanitización

```
$comment = mysqli_real_escape_string($conn, $_POST['comment']);
mysqli_query($conn, "INSERT INTO comments (comment) VALUES ('$comment')");

// Displaying user comment
$result = mysqli_query($conn, "SELECT comment FROM comments");
while ($row = mysqli_fetch_assoc($result)) {
    $sanitizedComment = htmlspecialchars($row['comment']);
    echo $sanitizedComment;
}
```

# Dom-Based XSS

Dom tree

document
<!DOCTYPE html>
html
head
title
meta
meta
meta
style
body
div
h1
p
p
a

El arbol empieza por el nodo "document" luego lo tipico de html y posteriormente las tags

Alternativa podemos acecder a la consola de JS y manipular el trre por ejemplo crear nuevos elemntos o etc

```
let div = document.createElement("div");
let p = document.createElement("p");
div.append(p);

console.log(div.childNodes); // NodeList [ <p> ]
```







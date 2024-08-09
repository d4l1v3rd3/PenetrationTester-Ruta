<p align="right"><img height=100px width=100px src="https://github.com/user-attachments/assets/28eba669-a8dd-418a-bc8d-cc7c8e147edc"></p>

<h1 align="center">IDOR</h1>

# ÍNDICE

# INTRODUCCIÓN

IDOR (INSECURE DIRECT OBJECT REFERENCE) es un tipo de vulnerabilidad de control de acceso.

Este tipo de vulnerabilidad ocurre cuando un servidor web recibe inputs de los usuarios para poder recuperar objetos (archivos, datos, documentos), demasida confianza en los datos del usuario y no se valida en el lado de lservidor.

# EJEMPLO

Imagina que te registras en un servicio online, y tu puedes cambiar la configuración del perfil. imagina "http://online-service.thm/profile?user_id=1305" y puedes ver la información.

Curiosamente si intentamos cambiar el valor del id, no sorprendemos porque encontramos la vulnerabilidad y podemos chekear información de otros usuarios.

# BUSCAR IDORs EN IDS CODIFICACADAS

Cuando pasamos datos entre una pagina y otra se manda unos datos POST, consultas o cookies, el desarrollador web aveces coge datos y los codifica. 

Codificandolos te aseguras que el servidor web esta calificado para entender el contenido.

Codificar hace que cambies los datos binarios a ASCII usando "a-z, A-Z, 0-9 y =".

Los encoders mas comunes de una web es "base64" bastante fácil de sacarlo.

[BASE64](https://www.base64decode.org)

![image](https://github.com/user-attachments/assets/e532d462-f4e0-4d8f-9637-a645a535f595)

# BUSACR IDORs EN IDS DE HASH

El ID hasheado hace que todo se complique un poco mas, pero se puede sacar predicciones dependiendo la versión del hash.

Por ejemplo, el numero 123 es = 202cb962ac59075b964b07152d234b70 en md5

[CRACKSTATIONS](https://crackstation.net) - Base de datos con millones de hashes para ir probando.

# BUSCAR IDORs EN IDS NO PREDECIBLES

Si la ID no se puede detectar o sacar por estos metodos, tenemos otros metodos de detección.

Si creamos dos cuentas y cambiamos los numeros de ID entre ellos.

Si tu puedes ver otros usuarios usando el número ID has encontrado la vulnerabilidad.

# DONDE ESTAN LOCALIZADOS LOS IDORs

La vulnerabilidad no tiene porque esta siempre en la URL. El contenido del navegador puede enviar consultas AJAX y puede estar referido a un archivo JavaScript

Aveces estos endpoints no estan referidos.

Por ejemplo, tu llamas a /user/details para ver la información. Pero tu descubre que el parametro user_id puede ver la información.

/user/details?user_id=123




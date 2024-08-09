

<h1 align="center">SSRF</h1>

# ÍNDICE

# INTRODUCCIÓN

SSRF (Server-Side Request Forgery). Es una vulnerabilidad donde el atacante puede cambiar o editar consultas HTTP en los recursos que quiera.

## TIPOS DE SSRF

Hay dos tipos de SSRF vulnerabilidad, la primera es tonde los datos se dan al atacante. La segunda es Blid SSRF donde no devuleve información en la pantalla del atacante.

### IMPACTO

- Acceso no autorizado
- ACceso a datos de la organización
- Escalar en redes internas
- Autenticación con credenciales o tokens

## EJEMPLOS

![image](https://github.com/user-attachments/assets/ced5036a-bd53-4706-8908-5bbacecf4d9b)

# ENCONTRAR UN SSRF

Las vulnerabilidades potenciales se encuentran en las aplicaciones web. Un ejemplo

![image](https://github.com/user-attachments/assets/e1fb21a7-ce90-4aa6-95c6-02d8d2b4ef4f)

![image](https://github.com/user-attachments/assets/9491efd9-1120-473e-8280-647a0cd054b8)

![image](https://github.com/user-attachments/assets/0e8d8797-7c54-40f4-acfe-41c68a891101)

Algunos de estos ejemplos son faciles de explotar que otros, y aqui es donde empiezan los errores.

Si trabajamos con blind SSRF no veremos completamente nada, necesitamos un HTTP externo para ir viendo si tenemos respuesta "requestbin.com" o "Burp Suite"

# ELIMINAR DEFENSAS COMUNES DE SSRF

Muchos desarrolladores de seguridad implementan chekeos para que no se puedan hacer este tipo de consultar. Denegandolas 

### LISTA DE DENEGACIÓN

Una lista de denegación es donde todas las consultar que se acepten o deniegen pasaran por ahi. Una aplicación web puede tener lista de los sensibles endpoints.

Ips, dominios que son dea cceso publico o otras localizaciones.

Un endpoint especial por ejemplo es el 127.0.0.1, los atacantes intentar utilizar diferentes localhost como 0.0.0.0, 127.*.*.*, etc.
 
Es bastante beneficioso bloquear ips que contengan metadatos del servidor, información sensible. Un atacante puede hacer un bypass registrandose en un subdominio o en el mismo que este dentro del DNS 169.254.169.254

### LISTA PERMITIDA

La lista permitida, es la regla donde los parametros de la web se cumpliran. Los atacantes pueden crear reglas dentro del subdominio haciendo consultas específicas.

### OPEN REDIRECT

Si todos los bypasses no funcionan, podemos hacer trucos, Un open redirect es ir a por un endpoint del servidor donde la web te redirija automaticamente a otra website. Por ejemplo el link "https://website.thm/link?url=https://tryhackme.com"

Se crea un recuento de los numeros de los visitantes cada vez que se clika el link. Pero imagina que hay una vulnerabilidad potencial con reglas. El atacante utilizara esto para redirigir la consulta HHTP al dominio del atacante.

# PRÁCITCA

Vamos a utilizar un escenario ficticio.

Utilizaremos Acme IT Support, primero nos iremos a /private, 


![image](https://github.com/user-attachments/assets/4ed119fb-16ab-46f3-91ee-385617d1fcf7)

Lo segundo es la version de la pagina /customers/new-account-page para ver las especificaciones.

Crearemos una cuenta. 


![image](https://github.com/user-attachments/assets/c2c50d35-bf99-4c3e-8398-400f45086e2c)

![image](https://github.com/user-attachments/assets/e4a549d8-6a9f-458c-bc13-761b8236a284)

Vamos a probara  hacer una consulta cambiando el valor del avatar de privado y pasar la ip. 


![image](https://github.com/user-attachments/assets/9da73bd4-d30f-4195-b582-a7fb0ea97596)

![image](https://github.com/user-attachments/assets/dd8b27bd-3669-4761-8357-d8f16ae87157)

![image](https://github.com/user-attachments/assets/36d39f1a-bdb5-4b86-841c-adf6f7dae731)


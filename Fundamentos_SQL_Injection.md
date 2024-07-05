# SQL INJECTION FUNDAMENTALS

## INTRODUCCIÓN

Las aplicaciones más modernas utilizan bases de datos estructuradas en back-end. Como las bases de datos se usan para almacenar archivos y recoger datos de la aplicación web, del contenido actual y la información de los usuarios. Para hacer la aplicación web dinamica.

La aplicacion web interactua con la bases de datos en tiempo real. Utiliza request (HTTPS) de los usuarios, la aplicación web del back-end responde a sus preguntas. 

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/332c7199-9315-4962-8497-02c37a3b588b)

Cuando un usuario pide información la se construye una consulta en la base de datos, los usuarios maliciosos pueden hacer trucos par usarlo como si fuera el programador principal, y proveer acceso a la propia base de datos para hacer un ataque.

SQL Injection hace el ataque a las bases de datos MySQL

## SQL INJECTION (SQLi)

Muchos tipos de vulnerabilidades de injección son posibles con las aplicaciones web, HTTP injection, code injection y command injection.

El mas común sin embargo es SQL injection. Ocurre cuando un atacante malicioso intenta pasar el input y cambiar la consulta final de SQL de la aplicación web, haciendo una consulta que no iba a hacer principalmente.

Hay muchas formas de lograr esto, el atacante siempre hara una injección SQL con codigo haciendo que cambie la consulta original, el atacante injecta codigo fuera de los limites que se esperan, en los casos más básicos se injecta una (') o (") para escapar de los limites del input para injectar código SQL.

Una vez que el atacante injecta, se ejecuta una consulta que no era la normal. Finalmente recibes la respuesta de la base de datos.

## USOS DE CASOS Y IMPACTO

Una consulta SQL tiene un tremento impacto, especialmente si ilos privilegios de la base de datos son muy flexibles.

Primero, podemos coger secretos o información confidencial que no debería ser visible, como usuarios y contraseñas o información importante, con esto el atacante podría hacer cosas maliciosas. Otro ejemplo es acceder a lugares que estan bloqueados por otros usuarios, como el panel del admin.

Los atacantes intentan leer y escribir ficheros o directorios de la base de datos. 

### PREVECIÓN

Las SQL Injections casualmente es por mal codigo de la aplicación web o mal códiglo en la base de datos o privilegios. SANITIZATION Y VALIDACIÓN.

## DENTRO DE LAS BASES DE DATOS

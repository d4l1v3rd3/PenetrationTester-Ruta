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

Antes de aprender SQL INjection aprenderemos su estructura, con baes de datos y las consultas necesarias, las aplicaciones web contienen bases de datos con contenido e informacion relevante.

Hay muchos diferentes tipos de bases de datos, Database Management Systems (DBMS)

## DATABASE MANAGEMENT SYSTEMS

Un (DBMS) ayuda a crear, definir, hostear y mantener bases de datos. Varias tipos de DBMS son diseñados fuera de tiempo, basados en archivos, relaciones, NoSQL, grafichos, llaves, etc.

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/a1014378-2c15-4bbb-a9af-3144022c758f)

## ARQUITECTURA

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/e7a1ce03-0c78-4eb1-a711-3bc2dcf2522b)

Tier I: Usualmente consiste en el lugar del usuario y las interfaces de los programas. Consisten en alto niveles de interacción como el login.
TIER II: Donde se llama a las APIS y otras request, es el medio, donde se intercepta los eventos que se van a mandar al DMBS, finalmente la aplciacion especifica una libreria y los drives que se van a utilziar, pudiendo insertar, borrar, recibir, etc.

## TIPOS DE BASES DE DATOS

Las bases de datos generalmente se categorizan entre relaciones y no relacionales. Solo las relacionales utilizan SQL.

### BASES DE DATOS RELACIONALES

Es la base de datos más común, 

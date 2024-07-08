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

Es la base de datos más común,  usa un esquema, un tema, dedicada a estructurar los datos de la base de datos. Por ejemplo, nosotros podemos imaginar una compañia que vende productos para los consumidores y tenemos que saber que too el conocimiento de los productos, cantidad, etc. En el back-end por ejemplo la primera tabla enseñaria información básica del consumidor, la segunda los productos y su coste otra tabla para los datos de pago, etc..

Normalmente estas tablas estan asociadas con llaves que dan acceso a especificos datos columnas, o campos de la base de datos. Estas tablas, se llaman entidades, y se correlacionan entre ellas, por ejemplo la informacion de un consumidos como el ID puede sen un indicador para luego saber la dirección el nombre la información etc.

Sin embargo, cuando este proceso se integra en la base de datos, un concepto que se require es linkear las tablas con llaves, esto se llama (RDBMS) uchas compañias usan conceptos diferentes de las mismas.

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/a883a691-68c3-4b9c-986b-9a634349e5cd)

Tenemos el id de user_id en el post y nos devuelve los detalles que queremos gracias a que se correlacionan.

La relacion se llama esquema.

### BASES DE DATOS NO RELACIONALES

Una base de datos no relacional se llama (NoSQL) no usa tablas, ni columnas, ni keys etc, ni relaciones. Todo depende de los datos que se quieran guardar, se define una estructura de base de datos debe ser muy escalable y flexible.

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/aa3595b3-ab64-4ab8-9170-eef35e1b57a1)

```
{
  "100001": {
    "date": "01-01-2021",
    "content": "Welcome to this web application."
  },
  "100002": {
    "date": "02-01-2021",
    "content": "This is the first post on this web app."
  },
  "100003": {
    "date": "02-01-2021",
    "content": "Reminder: Tomorrow is the ..."
  }
}
```

Para que nos entendamos es como un diccionario.

# DENTRO DE MYSQ

## ESTRUCTURA SQL

La utilidad de mysql se usa para autentificar y interactuar con un MySQL/MariaDB base de datos. La extension -u es usada para poner usuario y -p para la contraseña.

```
mysql -u root -p
```

Si queremos también tenemos la disponibilidad e poner la contraseña directamente
```
mysql -u root -p pass
```
Cuando no especifícamos un host sera siempre el localhost por defecto si queremos especificar el host utilizaremos las extension -h y -P
```
mysql -u root -h docker.hackthebox.eu -P 3306 -p 
```
El puerto por defecto de mysql es (3306)

## CREAR UNA BASE DE DATOS

Una vez que nos metemos dentro de mysql, podemos empezar a mandar consultas y por ejemplo vamos a crear una base de datos
```
CREATE DATABASE users;
```
Una vez que se ha creado podemos comprobarlo
```
SHOW DATABASES;
```
## TABLAS

Las DBMS guardan los datos en formas de tabla, una tabla simplemente es un campo en horizontal y vertical, con intersecciones llamado columnas y filas

Los tpos de datos se definen por el valor y eso le ayuda la columna, pueden ser numeros, strings, datos,tiempos y datos binarios. por ejemplo si quisieramos crear una tabla llamada logins

```
CREATE TABLE logins (
    id INT,
    username VARCHAR(100),
    password VARCHAR(100),
    date_of_joining DATETIME
    );
```
Como podemos ver CREATE TABLE especifica el nombre de la tabla y luego entre parenteses especifica los nombres que va tener cada columna y los datos que va a contener.

```
mysql> CREATE TABLE logins (
    ->     id INT,
    ->     username VARCHAR(100),
    ->     password VARCHAR(100),
    ->     date_of_joining DATETIME
    ->     );
Query OK, 0 rows affected (0.03 sec)
```
Las consultas SQL a creado una tabla llamada login con 4 columnas, id, usenarme, password y date_of_joining y cada uno con sus datos id con integrales, username con caracteres que no superen los 100 y el ultimo solo con el tiempo.

En caso de querer verlo: 
```
SHOW TABLES;
```
Nos dice la lista de tablas que tenemos si queremos usar llaves para buscar algo exactamente o algo .
```
DESCRIBE logins;
```
## PROPIEDADES DE LAS TABLAS

Con el "CREATE TABLE" tenemos una propiedades de cada tabla y columna, por ejemplo id se auto incrementa usando "AUTO_INCREMENT" automaticamente incrementa la id por cada nuevo item que hemos añadido a la tabla
```
id INT NOT NULL AUTO_INCREMENT,
```
La constante "NOT NULL" se refiere a una columna en particular y que nuna va a requerir archivo, usamo UNIQUE para hacer algo unico, por ejemplo la columna usernmae.

```
username VARCHAR(100) UNIQUE NOT NULL,
```
Otra importante llave es "DEFAULT" se usa para especificar un valor por ejemplo con "date" queremos hacer el valor Now() y que nos devuelva el tiempo
```
   date_of_joining DATETIME DEFAULT NOW(),
```
Finalmente el más imoprtante es la propiedad de "PRIMARY KEY" se usa para identiciar el record de la tabla refiriendose a todos los datos importantes que se relacionen con otras tablas por ejemplo el elemento id
```
PRIMARY KEY (id)
```
Entonces esto quedaria a sí :

```
CREATE TABLE logins (
    id INT NOT NULL AUTO_INCREMENT,
    username VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(100) NOT NULL,
    date_of_joining DATETIME DEFAULT NOW(),
    PRIMARY KEY (id)
    );
```
## DECLARACIONES SQL

Ahora entenderemos como usar mysql apra la utilidad.

## INSERTAR UNA DECLARACION

La declaración "INSERT" da un nuevo valor a la tabla ya creada
```
INSERT INTO table_name VALUES (column1_value, column2_value, column3_value, ...);
```
La sintaxis son los mimos valores que hariamos creando una y si queremos añadir
```
INSERT INTO logins VALUES(1, 'admin', 'p@ssw0rd', '2020-07-02');

```

El ejemplo de como hacer un login esta muy bien pero sin eembargo con valores como id o datos se tienen que especificar losn ombre y los valores que se van a insertar.
```
INSERT INTO table_name(column2, column3, ...) VALUES (column2_value, column3_value, ...);
```
Si skipeamos las columnas nos saldrá un error

Podriamos hacer lo mismo para una tabla de login

```
INSERT INTO logins(username, password) VALUES('administrator', 'adm1n_p@ss');
```
## SELECCIONAR UNA DECLARACION

Ahora nosotros queremos insertar datos dentro de la tabla y recibir también datos de la tabla "SELECET" es lo que utilizaremos

```
SELECT * FROM table_name;
```
El asterisko (*) hace que seleccione todas las columnas y "FROM" se usa para denotar la tabla que queremos seleccionar.

```
SELECT column1, column2 FROM table_name;
```
La consulta que seleccionaremos para presentar los datos de la columna 1 y 2

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/b7d47941-2ff4-4ce9-9681-94f9aaced753)

## DECLARACION DROP

Podemos usar "DROP" para remover tablas y bases de datos para el servidor.

```
DROP TABLE logins;
SHOW TABLES;
```
Para borrar importante que borra para siempreee y sin confirmar

## DECLARACION ALTER

podemos usar "ALTER" para cambair el nombre de cualquier tabla o archivo o borrar o añadir columnas o ttablas existenter.
```
ALTER TABLE logins ADD newColumn INT;
```
Para renombrar una columnar utilizaremos "RENAME COLUMN"
```
ALTER TABLE logins RENAME COLUMN newColumn TO oldColumn;
```
Podemos cambiar una columna y los datos con "MODIFY"
```
ALTER TABLE logins MODIFY oldColumn DATE;
```
Y finalmente tambien podemos borrar
```
ALTER TABLE logins DROP oldColumn;
```
Podemos usar todo lo que queramos gracias a este comando

## DECLARACION UPDATE

La declaración haceq ue actualicemos la tabla especificando campos de ella misma dentro
```
UPDATE table_name SET column1=newvalue1, column2=newvalue2, ... WHERE <condition>;
```
```
UPDATE logins SET password = 'change_password' WHERE id > 1;
```

## RESULTADOS EN LAS CONSULTAS

## ORDENAR RESULTADOR

Podemos ordenar resultados gracias al "ORDER BY" y especificar una columna
```
SELECT * FROM logins ORDER BY password;
```
Por defecto, ordenar se hace en ascendente, pero nosotros necesitamos Ascendente a descendente.
```
SELECT * FROM logins ORDER BY password DESC;
```
Tambien es posible por columnas, utilizando un orden difernete que pueda duplicar valores de una columna

```
SELECT * FROM logins ORDER BY password DESC, id ASC;
```
## RESULTADOS LIMITE

En algunos casos nos encontraremos con muchos resultados, podemos limitarlos con "LIMIT"
```
SELECT * FROM logins LIMIT 2;
```
Si queremos limitar y empezar desde uno
```
SELECT * FROM logins LIMIT 1, 2;
```
## CLAUSULA WHERE

Para filtrar datos especificos tenemos las condiciones "SELECT" usando la declaracion "WHERE"
```
SELECT * FROM table_name WHERE <condition>;
```
La consulta puede tener condiciones satisfactorias
```
SELECT * FROM logins WHERE id > 1;

```

Por ejemplo se puede seleccionar todos los datos del valor id que sea mas grande que 1 pero skipeara el 1
```
SELECT * FROM logins where username = 'admin';
```
Esta consulta especifica el usuario admin podemos usar UPDATE para especificar las condiciones

## CLAUSE LIKE

Otra importante para entablar selecciones de datos de algo exacto por ejemplo con admin o parecido a ello
```
SELECT * FROM logins WHERE username LIKE 'admin%';
```
EL simbolo % hace todos los maches que sean parecidos después de admin y por ejemplo lo hacemos con "___" nos saldran los que tienen 3 digitos .

```
SELECT * FROM logins WHERE username like '___';
```

# OPERADORES SQL

## OPERADOR AND

Aveces, no solo necesitamos una simple conficion podemos necesitar operadores logics con multiples condiciones como "AND" "OR" o "NOT"

```
condition1 AND condicion2
```
El resultado de "AND" es true o false

```
SELECT 1 = 1 AND 'test' = 'test'
SELECT 1 = 1 AND 'test' = 'abc';
```

En los terminos de MySQL un no zero se considera true porque devuelve 1 y 0 es considerado falso test=abd falso

## OPERADOR OR

El operador OR coge dos expresiones y devuelve true si una de las dos es verdad

```
SELECT 1 = 1 OR 'test' = 'abc';
```
true
```
SELECT 1 = 2 OR 'test' = 'abc';
```
false

## OPERADOR NOT

El operador NOT simplemente devuelve true cuando es falso y asi vice versa

```
SELECT NOT 1 = 1;
SELECT NOT 1 = 2;
```

## OPERADORES POR SIMBOLOS

Todos los operadores pueden ser representados

| AND | OR | NOT |
| ---- | ----- | ------ |
| && | "||" | ! |

## OPERADORES EN CONSULTAS

Vamos a ver como hacer que los operadores puedan usar las consultas. Lo siguiente es ordenar el usuario que no es john

```
SELECT * FROM logins WHERE username != 'john';
```
ahora quiero una consulta en la que la id empiece por mas que 1 y el usuario no sea john

```
SELECT * FROM logins WHERE username != 'john' AND id > 1;
```

## PROCEDENCIA DE MULTIPLES OPERADORES

SQl suportea varios operadores en adicional, divisiones y demas operaciones debemos saber cuales son:

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/1412a599-8379-418a-9aa5-49e8db343713)


Operaciones :
```
SELECT * FROM logins WHERE username != 'tom' AND id > 3 - 2;
```
La consulta hace cuatro operadores !=, AND, >, - de la procedencia del operador y la evalcuaion es 3-2=1
```
SELECT * FROM logins WHERE username != 'tom' AND id > 1;
```
Despues de las dos operadores de ocmapracion podemos hacer los mimos procesos con cualqueira
```
select * from logins where username != 'tom' AND id > 3 - 2
```
## DENTRO DE SQL INJECTION

Ahora que tenemos una idea de como funciona MySQL pasaremso a SQL injection.

### USAR SQL EN LAS APLICACIONES WEB

Primero de todo ya sabemos que todas las aplicaciones web usan bases ded atos MySQL, en este caso para guardar y recibir datos. 

Por ejemplo podemos utilizar PHP para conectar con nuestra base de datos y emperaz usando MySQL para la sintaxis.

Código PHP
```
$conn = new mysqli("localhost", "root", "password", "users");
$query = "select * from logins";
$result = $conn->query($query);
```
Entonces cuando la se manda la consulta sabemos que la variable $result contiene la consulta.

Debajo del codigo php podemos printear quen os devuelvan los resultados de SQL en nuevas lineas.

```
while($row = $result->fetch_assoc() ){
	echo $row["name"]."<br>";
}
```
La aplicacion web normalmente usa inputs del usuario para recibir daots. Por ejemplo, cuando usuario utiliza la funciona de buscar esta preguntando a la base de datos.

```
$searchInput =  $_POST['findUser'];
$query = "select * from logins where username like '%$searchInput'";
$result = $conn->query($query);
```

Si nosotros usamos user-input con SQL, y nuestro código no es seugor, en muchos casos será mun fallo y una vulnerabilidad a Injecciones SQL.

En el anterior código vemos un código que no esta sanitizado, en los casos tipicos el "searchinput" completa la consulta devolviendo lo que esperamos, cualquier cosa que escribamos va a esa consulta

```
select * from logins where username like '%$searchInput'
```
Entonces si nosotros inputemos "admin" tal qque '%admin' en este caso nosotros estamos escribiendo SQL, esto se considera buscar un termino. Por ejemplo si nosotros inputeamos "SHOW DATABASES" se ejecutara '%SHOW DATABASES' la aplicacion buscara usuarios similares, pero sin sanitizacion podemos añadir un "'" para escribir más código tal que 1'; DROP TABLE users;

```
'%1'; DROP TABLE users;'
```
Añadimos una sola ' despues del 1 para escapar de la consulta inicial.

```
select * from logins where username like '%1'; DROP TABLE users;'
```
## ERRORES DE SINTAXIS

Con el anterior comando es posible que nos de error
```
Error: near line 1: near "'": syntax error
select * from logins where username like '%1'; DROP TABLE users;'
```
En este caso es por "'" que se encuentra en el final de la consulta.

Para tener una injección satisfactoria, deberemos crear una nueva consulta que no tenga errores de sintaxis, en muchos casos no tenemos acceso al codigo pero que no hemos hecho uan buena consulta.

### TIPOS DE SQL INJECTIONS

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/464162f3-9ff7-4d7e-aec8-0c8c896d4eb8)






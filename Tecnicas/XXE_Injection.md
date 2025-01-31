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

XSLT (Exstensible Stylesheet Language Transformation) Es un lenguaje usado para transformar y 

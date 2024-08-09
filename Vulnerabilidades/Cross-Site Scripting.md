<p align="right"><img height=100px width=100px src="https://github.com/user-attachments/assets/28eba669-a8dd-418a-bc8d-cc7c8e147edc"></p>

<p align="center"><img src="https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/6f0d0415-0442-48c8-ab8e-f78062b511a3"></p>


<h1 align="center">Cross-site scripting (XSS)</h1>

# ÍNDICE

- [PROTECCIÓN](https://github.com/D4l1-web/PenetrationTester-Ruta/blob/main/Cross-Site%20Scripting.md#protecci%C3%B3n)
- [IMPLEMENTAR CONTENIDO SEGURO](https://github.com/D4l1-web/PenetrationTester-Ruta/blob/main/Cross-Site%20Scripting.md#implementar-contenido-seguro-de-privacidad)


Cross-site scripting (XSS) es uno de los metodos mas comunes que los hackers utilizan para atacar paginas webs. Permite al usuario malicioso ejecutar codigo arbitrario JavaScript cuando otros usuarios visitan la página.

XSS es una de las vunerabilidades que más se reportan en los sitios web. 

¿Qué puede hacer un hacker explotando una vulnerabilidad XSS?

XSS ejecuta arbitrariamente codigo JavaScript, el daño que puedan hacer depende de la sensibilidad de los datos que pueda proveer dicha página.

- Se podrían difundir gusanos en las páginas como "Facebook, Twitter"
- Robo de sesión, pudiendo coger las cookies remotamente.
- Robo de identidad, Información confidencial tarjetas, numeros, etc.
- Denegación de servicio.
- Robo de datos sensibles, como contraseñas.
- Fraude financiero en bancos.

# Protección 

Para poder ser protegido por estos ataques, tendremos que tener seguros que nuestro contenido dinamico que puedas enviar datos, puedas inyectar codigo JavaScript.

Todas las páginas estan hechas con HTML, usualmente describen el tema y los ficheros, lo utilizariamos con contenido dinamico y cuando la pagina se volivera a cargar parariamos.

Con "Stored XSS attacks" hacemos que dentro del contenido dinamico podamos guardan contenido en el "backend". El atacante abusa de archivos que se pueden editar e insertar codigo JavaScript, evaluado por el navegador cuando otro usuario visita la página.

A menos que tu sitio web sea un (CMS), es raro que sus usuarios puedan crear "HTML" sin formato, debee escapar de todo el contenido dinámico que se pueda almacenar datos y el navegador sepa que solo pueda aceptar etiquetas "HTML"

![image](https://github.com/pons-rgb/vuln/assets/174595469/07f4790a-365a-4360-b818-3557e00734a8)

## VALORES DE LISTA PERMITIDOS

Si un particular dato dinamico de cada item puede tener solo algunos valores validos, la mejor practica es restringir estos valores, solo permitir los valores que necesitemos.

## IMPLEMENTAR CONTENIDO SEGURO DE PRIVACIDAD

Los navegadores puedes suportear contenido de seguridad y sus politicas, cuando el autor tiene el control de la página web, donde "JavaSCript y otros valores" pueden ser ejecutados. Los ataques XSS pueden hasta llegar a correr código malicioso en la página del usuario. Injectando 
```
<script></script>
```
Obviamente con el tag de html.

Una buena privacidad de seguridad, un header responsivo, y decirle al navegador que nunca ejecute arbitrariamente código "JavaScript", y bloquear los dominios que contengan host "JavaScript" para la página.

Por ejemplo le podrías decir al navegador.
![image](https://github.com/pons-rgb/vuln/assets/174595469/9dab6c46-d5b9-4543-9cb6-85e11dadeea9)

Listar los scripts que no queremos que se ejecuten.
![image](https://github.com/pons-rgb/vuln/assets/174595469/a9f6348a-9083-4639-9ad8-c34fc80818a7)

Esto protejera a los usuarios efectivamente, sin embargo esto no hace que sea 100 por cien seguro.

# XSS PAYLOAD

En XSS, los payloados son código JS que se ejecutan en el ordenador víctima. Hay dos partes del payload, la intención y la modificación.

La intención que tenga JS, la modificacion cambia el codigo que necesitamos para ejecutar en diferentes escnarios

### PROOF OF CONCEPT

Estos sin payloads simples que demostran XSS en las aplicaciones web
```
<script>alert('XSS');</script>
```

### ROBO DE SESIÓN

Los detalles de la sesión de usuario, son con logins de tokens, aveces conseguiremos las cookies de la maquina víctima. Debajo del script de JS, base64 docificados esta la cookie para una tranmisión y no pueda un hacker tener el contro. Si el atacante consigue las cookies del usuario puede entrar sin necesidad 

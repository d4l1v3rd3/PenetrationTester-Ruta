En aplicaciones modernas, las vulnerabilidades OAuth son serias y fecuentes, hablaremos sobre esta versión y la siguiente la 2.0, y los frameworks

# Objetivos de aprendizaje

- Conceptos esenciales de OAuth 2.0
- Flow
- Identificas dichos servcios
- Explotar técnicas
- Evolución de OAuth 2.1

# Conceptos Clave

Discutiremos los conectpos clave para enteder esta vulnearbilidad, estos conecptos son para entender los frameworks donde eta construido. 

## Recursos de Jefe

El recurso jefe es una persona o un sistema que controla los datos y la autorizacion a la aplicacion y acceso a dicho datos. Este conecpto es fundamentar porque se centra alrededor del consentimiento y el control.

## Cliente

El cliente es la aplicación movil o la aplicacion web. Esto son intermediarios, consultas y recursos dependiendo las acciones que permite el jefe. 

## Autorización de Servidor

La autorización del servidor es responsable de qque ningun token emisor tenga acceso despues de la autentificación del recurso y obtenga previa autorización. La autorización del servidor juega un rol crucial en el proceso de OAuth asegurando que el cliente tiene los permisos solo los usuarios legitimos. 

## Recursos del Servidor

El servidor hostea los recursos y los protegue aceptando y respondiendo las consultas a dichos recuros usando tokens de acceso. El servidor solo autentificay autoriza a clientes que puedan acceder y manipular los recursos.

## Garantia de Autorización

EL cliente usa credenciales para representar la autorización al recurso, y obtiene su token. Los primeros tipos son 

- Autorhization
- Code
- Implicit
- Resource Owener Password CRedentials
- Client Credentials

## Token de Acceso

Una credencial de un cliente acceso a recursos protegidos en nombre del propietario del recurso. Esto limita la vida y un alcance mas limitado. Los tokens de acecso son esenciales para un mantenimiento seguro y protegido en la comunicación del cliente y el recurso del servidor esto se hace en todo.

## Regenerar Token

Una credencial de un cliente obtiene un nuevo acces ode token sin requererir la re-autentificación del propietario de dicho recurso. 

## Redirección URI

La URI es la que autoriza al servidor a redirigir el recurso propietario despues de denegar la autorización. Esto checkea al cliente si el cliente tiene la autorizació0n y si la consulta es correcta.

## Alcance

EL alcance es el mecanismo que limita el acceso de la aplicación a usuarios. Esto permite al cliente especificar el nivel de acceso que va a necesitar y la autorización de lservidor informar al usuario que acceso tiene. 

## Parametro estado

Un parametro opcional mantenido entre el cliente y la autorización del servidor. Esto ayuda a prever ataques CSRF (Cross-site request forgery) asegurando que las respuestas concuerdan con las del cliente. Este parametro es crucial para la seguridad de OAuth

## Final de un Token y Autorización

El final es cuando el cliente intercambia la autorización para el acceso al token.

# Tipos de Concesión OAuth


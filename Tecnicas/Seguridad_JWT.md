# Seguridad de JWT
Aprenderemos sobre los JSON Web Tokens (JWTs) y la seguridad asociada a ellos. Con esto sus APIs, autentificación basada en tokens siendo la mas popular, y las mayores implementaciones

# Objetivos de aprendizaje

- Aprender sobre autentificación basada en tokens
- Aprender sobre JSON web tokens
- Aprender sobre porque son populares
- Aprender sobre la seguradad y consideraciones cuando usamos JWTs
- Aprender como atacatar implementaciones vulnerables de JWT

# Autentificación basada en Token

## El ascenso de las APIs

APIs, son increiblemente populares hoy en día. Una de las razones por este boom esta disponible para crear una sola API esque puede tener diferentes interfaces, como una aplicación web y aplicacion de móvil en el mismo tiempo. Esto es una lógica de servidor centralizado. Para una perspectiva de seguridad, esto es beneficioso porque implementamos la misma seguridad en una sola API.

Sin embargo, los nuevos metodos de sesión se crean con las Apis. Las cookies normalmente se asocian a las aplicaciones web usadas por el navegador, las basadas en cookies normalmente no funcionan bien. 

## Token Basados en gestión de sesión

Este tipo de tokens son relativamente un concepto nuevo. En cambio usando el navegador automaticamente la cookie funcionaba, después de la autentificación, la aplicacion da un codigo con un body. Usa el lado cliente el JS, el toquen se guarda en el navegador.

## Projectos API

Vamos a aprender a explotar API. documentar metodos.

### API Final 

El prjecto de API tiene un endpoint (en este caso) http://ip/api/v1.0/exampleX

### Credenciales

user:passwordX

### Ejemplos

```
curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "passwordX" }' http://10.10.48.144/api/v1.0/exampleX
```
```
curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "passwordX" }' http://10.10.48.144/api/v1.0/exampleX
```

El JWT token se puede remplazar por ejemplo por usuario:contraseña como en el anterior caso

# JSON Web Tokens

JWTs son contenedores de tokens que se usan para trasmitir la información segura. Es un stardar abierto, dando información al desarrollador y creador de libreria.

## JWT Estructura

- Header
- Payload
- Signature

## Algoritmos firmados

- None
- Symmetric Signing
- Asymmetric Signing

# Información Sensible

## Ejemplo práctico 1

```
curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password1" }' http://10.10.48.144/api/v1.0/example1
```

[Decodificar](https://jwt.io)

Esto nos da un token JWT lo decodificamos

```
payload = {
    "username" : username,
    "password" : password,
    "admin" : 0,
    "flag" : "[redacted]"
}

access_token = jwt.encode(payload, self.secret, algorithm="HS256")
```

### Como arreglarlo

```
payload = jwt.decode(token, self.secret, algorithms="HS256")

username = payload['username']
flag = self.db_lookup(username, "flag")
```

Que se guarden en variables del server



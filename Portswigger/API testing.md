# RECONOCIMIENTO DE API

Para empezar con el testeo de API, necesitamos saber toda la información posible de dicha aplicación, para encontrar un vector de ataque.

Para empezar, dberiamos empezar a identificad los "endpoints". Estas localizaciones son donde la aplicación recibe consultar especificas que llegan a los recursos del servidor.
Por ejemplo, la consulta "GET"
```
GET /api/books HTTP/1.1
Host: example.com
```

Podemos ver el "endpoint" de la consulta es "/api/books". Este resultando es la interaccción con la aplicacicón y le devuelve la lista de los libros de la libreria.
Otro "endpoint" podría ser por ejemplo, "/api/books/mystery", dandonos la dicha lista.

Una vez identificado esto, podemos determinar como vamos a interactuar. Estos activa la construcción de una consulta HTTP válida para testear la API. Por ejemplo, deberiamos buscar este tipo de información.

- Los dos datos input los procesa l API, incluyendo parámetros obligatorios como opcionales.
- Los tipos de consultas deben ser aceptados por la API, incluyendo los métodos y los formatos.
- Limites de tarifas y metodos de autentificacion.

## DOCUMENTACIÓN DE LA API

Una aplicación normalmente esta documentada por los desarrolladores y saben como integrarla.

La documentación debe poder ser leida. 

### DESCUBRIR DOCUMENTACIÓN DE API

La documentación de la API no siempre esta disponible, deberemos tener acceso a los mismos archivos.

Para hacer esto, podemos usar Burp Sacnner dentro de API. Y podemos buscar manualmente con el navegador y buscar endpoint como:

- /api
- /swagger/index.html
- /openapi.json

Si identificamos un endpoint sería bueno especificar dentro de el recursivamente

## LAB 1 - EXPLOTAR EL ENDPOINT DE APLICACION USANDO LA DOCUMENTACIÓN

Para resolver este laboratorio, deberemos buscar documentación de la API y borrar a "Carlos". Logearnos con una cuenta con las credenciales "wiener:peter"


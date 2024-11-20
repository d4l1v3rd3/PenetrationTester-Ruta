# Enumeración y Fuerza Bruta en Aplicaciones Web

La enumeración para autentificarse es fundamental para un tester de seguridad, especficamente en los mecanismos que progentes aspectos sensibles de la web, en este proceso aprenderemos metodicamente varios aspectos de autentificarse dando rangos de usuarios y contraseñas segun las politicas y sesion.

## Objetivos

1- Entender el significado de enumerar y como paso a paso hacer ataque por fuerza bruta efectivo.
2- Aprendes metodos de enumeración avanzados, enfocados en extraer información gracias a los errores.
3- Comprender la relación entre enumerción y fuerza bruta en los mecanismos de autentificación comprometidos
4- Ganar experiencia practica usando herramientas y tecnicas de enumeració y ataques de fuerza bruta

### Pre-Requisitos

1- Conocer HTTP y HTTPS, incluidos consulta/respuesta estructura y codigos de estado comunes
2- Experiencia en herramientas como Burp Suite
3- Conocer y navegar sobre Linux


# ENUMERACION

Pensemos como detectives digitales. Queremos busacr pistas y para ello deberemos entender como funciona la seguridad de un sistema. En esencial su forma de autentificar. 

## Identificar Usuarios Validos

Sabiando los usuarios podemos centrarnos en un ataque de contraseñas. Podemos figurar diferentes usuraios, y observar como una aplicacion responde dependiendo el usuario o contraseña. Por ejemplo, mensajes de error como "esta cuenta no existe o contraseña incorrecta" podemos tener pistas de usuarios validos, haciendo el trabajo más fácil.

## Politicas de contraseñas

Las guias que tenemos que hacer cuando creamos contraseñas nos dan un gran valor dentro del cotexto de las contraseñas que puede usar la aplicacion. Si enterndemos las politicas, el atacante tiene un mayor potencial para contextualizar las contraseñas. Y seguir una estrateguia, como codigos PHP, simbolos unicos, numeros, etc.

```
<?php
$password = $_POST['pass']; // Example1
$pattern = '/^(?=.*[A-Z])(?=.*\d)(?=.*[\W_]).+$/';

if (preg_match($pattern, $password)) {
    echo "Password is valid.";
} else {
    echo "Password is invalid. It must contain at least one uppercase letter, one number, and one symbol.";
}
?>
```

## Lugares Normales para enumerar

### Páginas de registro

Las aplicaciones web normalmente usan usuarios para un proceso de registro en que indica si el email o el usuario esta disponible. Si se prueba a registrar con un usuario o email "existente" no nos djeara crearlo y de hay podemos sacar cuentas potenciales.

### Intentos de Reseteo de contraseña

Los reseteo de ocntraseña estan diseñados para ganar acceso al usuario a su cuenta recibiendo instrucciones de reset. Sin embargo la aplicación puede darnos información relevante. 

### Errores Verbose

Durante un intento de logeo nos puede devolver mucha información. Cuando estos mensajes son como "usuario no encontrado" y "contraseña incorrecta" nos da mucho par aentender los fallos del login.

### Información Filtrada

# Enumerar Vía Errores Verbose

- Errores de ruta: Como un mapa buscando un tesoro, encontrar ficheros o rutas relevantes pero también malas configuraciones y información que no es normalmente visible
- Detalles de la base de datos: Como una serpiente dentro de la base de datos, hay errores como nombres de tabla y detalles de columnas.
- Información de usuario: A veces, estos errores pueden dar usuarios o información personal.

## Provocar Errores verbose

1- Intentos de logeo invalidos: Esto es como tocar a cada puerta y ver si esta abierto. Promptear intencionalmente usuarios y contraseñas incorrectos.
2- SQL Injection: Esta teqnica da comandos maliciosos SQL como meter una sola ' y ver que erorr lanza
3- File Inclusion/Path Traversal: Manipulando rutas, los atacantes pueden encontrar ficheros restringidos o errores en rutas internas. "../../"
4- Manipulación de formulario: Cogiendo formularios e intentando enseñar errores de logica de backend y información e usuarios sensible.
5- Fuzzing de aplicación: Inputear o mandar inputs de varias partes de aplicaciones y ver como reacciona identificando puntos vulnerables (Bupr Suite)

![image](https://github.com/user-attachments/assets/014228eb-e8d5-4013-9dab-3f94687109fd)


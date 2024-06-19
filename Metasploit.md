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



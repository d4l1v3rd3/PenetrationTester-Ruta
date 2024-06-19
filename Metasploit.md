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


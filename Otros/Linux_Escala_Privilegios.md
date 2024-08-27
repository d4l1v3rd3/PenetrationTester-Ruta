<h1 align="center"> Linux Escala Privilegios </h1>

# INTRODUCCIÓN

Todo depende de la configuración específica del target. Versión del kernel, aplicaciones instaladas, programas que soporten lenguajes de programación, contraseñas de usuarios y cosas que puedan generar una shell root.

# ¿Qué es la escala de privilegios?

Ganar desde permisos bajos hasta aumentar a mayor disponibilidad. La explotación de la vulnerabilidad, te deja acceder a recursos no autorizados por los usuarios.

## ¿Porqué es importante?

Porque puedes ganar acceso administrativo pudiendo:

- Resetear contraseñas
- Bypasear controles de acceso o datos comprometidos
- Editar configuraciones de software
- Activar persistencia
- Cambiar los privilegios existentes de nuevos usuarios
- Ejecutar comandos de administrador

# ENUMERACIÓN

Enumeración es el primero paso que tomaremos para ganar acceso a un sistema. Cuando ya tengamos dicho acceso al sistema, podemos explotar vulnerabilidades criticas para ganar acceso root, mandando comandos usando bajos privilegios.

## Hostname

El comando "hostname" te devuelve el hostanme de la máquina. como "Ubuntu-31412" 

## Uname -a

Nos da información o detalles sobre el kernel usado en el sistema

## /proc/version

Da información sobre los procesos del sistema. Tu puedes buscar "proc" diferentes.

## /etc/issue

Los sistemas se identifican mirando /etc/issue. El archivo usualmente contiene información sobre el sistema operativo pudiendo cambiarlo o customizarlo. Mientras, podemos buscar información el sistema. 

## ps

El comando "ps" te enseña los procesos que se estan ejecutando en el sistema Linux. 

- PID : ID proceso
- TTY : Tipo de terminal
- Time : Cantidad de CPU que se usa en el prceso
- CMD : Los comandos o ejecutables

![image](https://github.com/user-attachments/assets/e789a811-afe8-48e2-ae46-a0971b022270)

```
ps -a
ps axjf
ps aux
```

## nev

Nos enseña variables imporatnes

![image](https://github.com/user-attachments/assets/2eb4980d-c673-4fb9-b712-3b68d28d9c15)

## sudo -l

Nos dice los comandos que estan disponibles para hacer desde dicha cuenta

## ls

## id

Grupos, usuarios, etc

## /etc/passwd

## history

## ifconfig

## netstat 

```
nestat -a
nestat at
netsat -l
```

Chequeamos si existen intercaes y router network o comnicaciones lgicas.

## find 

```
find . -name flag1.txt: find the file named “flag1.txt” in the current directory
find /home -name flag1.txt: find the file names “flag1.txt” in the /home directory
find / -type d -name config: find the directory named config under “/”
find / -type f -perm 0777: find files with the 777 permissions (files readable, writable, and executable by all users)
find / -perm a=x: find executable files
find /home -user frank: find all files for user “frank” under “/home”
find / -mtime 10: find files that were modified in the last 10 days
find / -atime 10: find files that were accessed in the last 10 day
find / -cmin -60: find files changed within the last hour (60 minutes)
find / -amin -60: find files accesses within the last hour (60 minutes)
find / -size 50M: find files with a 50 MB size
find / -writable -type d 2>/dev/null : Find world-writeable folders
find / -perm -222 -type d 2>/dev/null: Find world-writeable folders
find / -perm -o w -type d 2>/dev/null: Find world-writeable folders
find / -name perl*
find / -name python*
find / -name gcc*
```

# Herramientas automatizadas para enumerar

Muchas herramientasn os ayudan a enumerar procesos mucho más rapido. 

- [LinPeas](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
- [LinEnum](https://github.com/rebootuser/LinEnum)
- [LES (Linux Exploit Suggester)](https://github.com/mzet-/linux-exploit-suggester)
- [Linux Smart Enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
- [Linux Priv Checker](https://github.com/linted/linuxprivchecker)



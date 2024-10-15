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






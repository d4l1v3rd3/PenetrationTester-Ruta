# Enumerar Active Directory

Una vez que tenemos las credenciales y la forma de autentificarnos dentro de la misma red, se nos abre un nuevo mundo de posiblidades que se nos abren! Podemos enumerar varios detalles de AD y su estructura y como funciona el acceso para autentificarse con pocos privilegios.

Durante un analisis de red team, es normal que se utilice una escala de privilegios o movimiento lateral ganando acceso adicional con privilegios suficientes para ejecutar nuestras herramientas.

# Objetivos de aprendizaje

- Snap-ins AD de Microsoft Management Console
- Los comandos Net del Command Prompt
- Los AD-RSAT cmdlest de PowerShell
- Bloodhound

# Meterse dentro de un dominio por comandos

```
systemd-resolve --interface enumad --set-dns $THMDCIP --set-domain za.tryhackme.com
```

# Injección de credenciales

Antes de entrar dentro de los objetos de AD y la enumeración, vamos a hablar sobre metodos de injección de credenciales. De una brecha de red de AD, deberemos ver si nuestras credenciales han sido comprometidas. 

## Explicación RUNAS

Una vez encontradas dichas credenciales necesitaremos entrar.

Para esto utilizaremos runas

```
runas.exe /netonly /user:domain\username cmd.exe
```

Una vez ejecutado dicho comando, deberemos promptear la contraseña y estaremos dentro del dominio.

# Siempre es DNS

Despues de meternos, deberemos verificar si las credencailes funcoinan. La forma de listar "SYSVOL" cualquier cuenta de AD, en dicho directorio

Este directorio existe en todos los controladores de dominio. Es un archivo de GPOs donde hay información sobre srcips de domini y componentes esenciales de AD.

Antes de listar SYSLOS, deberemos configurar nuestro DNS. Aveces si tenemos suerte con DHCP se configurara automaticamente, pero no siempre. Deberemos configurar nuestro servidor DNS usando una ip del conotrolador de dominio

Powershell

```
$dnsip = "<DC IP>"
$index = Get-NetAdapter -Name 'Ethernet' | Select-Object -ExpandProperty 'ifIndex'
Set-DnsClientServerAddress -InterfaceIndex $index -ServerAddresses $dnsip
```

Vamos a meter un nslookup a dicho dominio en este caso

```
nslookup za.tryhackme.com
```

Cuando resuelva el DC la ip. podemos testear en por ejemplo dicha ruta

```
dir \\za.tryhackme.com\SYSVOL\
 Volume in drive \\za.tryhackme.com\SYSVOL is Windows
 Volume Serial Number is 1634-22A9

 Directory of \\za.tryhackme.com\SYSVOL

02/24/2022  09:57 PM    <DIR>          .
02/24/2022  09:57 PM    <DIR>          ..
02/24/2022  09:57 PM    <JUNCTION>     za.tryhackme.com [C:\Windows\SYSVOL\domain]
               0 File(s)              0 bytes
               3 Dir(s)  51,835,408,384 bytes free
```

# Enumerar desde la consola de Microsoft Management

# Usuarios y ordenadores

Irnos a la estructura de AD donde teemos todas las UA Unidades organizaticas

![image](https://github.com/user-attachments/assets/f2ed5e11-b234-4128-b2d8-afd640dff3bb)

![image](https://github.com/user-attachments/assets/5bbd587d-f75d-478e-a850-bf4e3c4011de)

Clickar cualquier usuario y poder ver propiedades y atrubtutos

# Enumerar con comandos

## Usuarios

```
net user /domain

net user zoe.marshall /domain
```

## Grupos

```
net gorup /domain
```

## Politica de contraseñas

```
net accounts /domain
```

# Enumerar con PowerShell

## Usuarios

```
 Get-ADUser -Identity gordon.stevens -Server za.tryhackme.com -Properties *
```

Otros que podemos usar parametros -Filter o formatos de tabla

```
PS C:\> Get-ADUser -Filter 'Name -like "*stevens"' -Server za.tryhackme.com | Format-Table Name,SamAccountName -A
```

## Grupos

```
PS C:\> Get-ADGroup -Identity Administrators -Server za.tryhackme.com
```

## Objetos de AD 

```
PS C:\> $ChangeDate = New-Object DateTime(2022, 02, 28, 12, 00, 00)
PS C:\> Get-ADObject -Filter 'whenChanged -gt $ChangeDate' -includeDeletedObjects -Server za.tryhackme.com
```
```
PS C:\> Get-ADObject -Filter 'badPwdCount -gt 0' -Server za.tryhackme.com
```

## Coger Dominios

```
PS C:\> Get-ADDomain -Server za.tryhackme.com
```

## Alterar Objetos de Dominio

Alterar contraseñas

```
PS C:\> Set-ADAccountPassword -Identity gordon.stevens -Server za.tryhackme.com -OldPassword (ConvertTo-SecureString -AsPlaintext "old" -force) -NewPassword (ConvertTo-SecureString -AsPlainText "new" -Force)
```






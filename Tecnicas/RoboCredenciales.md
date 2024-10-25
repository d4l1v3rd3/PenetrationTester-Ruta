# Robo de credenciales

Este termino es ganar acceso al usuario y sistema con credenciales.

# Acceso de credenciales

El acceso a credencailes es comprometes systemas y ganar acceso al usuarios por ejemplo. Es importante el movimiento lateral y acceder a otros recursos o aplicaciones de los sistemas.

Clear-text files
Database files
Memory
Password managers
Enterprise Vaults
Active Directory
Network Sniffing

## Archivos de texto claros

Los atacantes buscas maquinas comprometicas o archivos locales o archivos remotos. Incluyendo información sensible para crear usuarios, contener contraseñas, etc.

- Historial de comandos
- Configuracion de archivos
- Otros ficheors de aplicaciones windows
- Archivos de bakcup
- Archivos compartidos
- Registros
- Código fuente

- Una buena forma de buscarlo en windows es buscar por ejemplo en el registro de windows

```
reg query HKLM /f password /t REG_SZ /s
```

## Archivos en bases de datos

Las aplicacione susan bases de datos para leer o escribir configuraciones o credenciales.

## Gestores de contraseñas

Apliacciones que guardan pero esta no la veo mucho la verdad

## Memoria

## Directorio activo

## Sniffing

### Ejemplo

```
Get-ADUser -Filter * -Properties * | Select-Object DistinguishedName, SamAccountName, D
```

# Credenciales locales de windows

## Keystrokes

Keyloggets son software o hardware que se conecta y coge los logs del teclado. Diseñados para estos propositos y podemos usar Metasploit o demás

## SAM

SAM es una base de datos que contiene cuentas locales información como usuarios y contraseña. Guarda los datos encriptados en formado para luego sea muy complicado volver a conseguirlos.

Hay varias formas de atacar la base de datos

primero vamos a ver si tenemos posibilidad de leer

```
type c:\Windows\System32\config\sam
```

## Metasploit Hash

El primer metodo es con Metasploit, hashdump, es copias el contenido de la base de dato sde sam y injectarle el LSASS.exe

```
getuid
hashdump
```

## Copias volumen shadow

1- Ejecutamos un prompr con permisos de administrador desde el cmd
Ejecutamos el comandos wmic para copias el disco C:
Verificar las creacion
Copias las base de datos a lvolumen creado

```
wmic shadowcopy call create Volume='C:\'
```

Una vez el comando sea bueno ejecutaremos "vssadmin" para listar y confirmar la copia

```
vssamin list shadows
```

Una vez veamos la ruta, cogemos la copia

## Registros

Otro posible metodo es con los registros de windows.

```
reg save HKLM\sam C:\users\Administrator\Desktop\sam-reg
reg save HKLM\system C:\users\Administrator\Desktop\system-reg
```

Podemos intentar desencriptar con "secretsdump.py" 

```
python3.9 /opt/impacket/examples/secretsdump.py -sam /tmp/sam-reg -system /tmp/system-reg LOCAL
```

# Local Security Authority Subsystem Service (LSASS)

Verifica las cuentas, contraseñas, hashes, tickets, etc..

Aquí tenemos bastante info

# Windows credential manager

Es una catacteristica de windows

![image](https://github.com/user-attachments/assets/a5caa28a-610a-4b0a-aae7-a44957cd7e82)

```
vaultcmd /list
```

# Controlador de dominio


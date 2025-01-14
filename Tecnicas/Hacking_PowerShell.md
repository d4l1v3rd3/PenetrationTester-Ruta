# Hacking PowerShell

# Que es Powershell

Powershell es un lenguaje de Scripting de windows y un entorno de shell construido en .NET

Esto da la opción a Powershell ejecutar directamente funciones .NET desde la shell. La mayoría de los comandos se llaman "cmdlets", escritos en .NET no como otros lenguajes de script y entornos de shell. 

- Get
- Start
- Stop
- Read
- Write
- New
- Out

Comando para conseguir un nuevo objeto get net "Get-Command"

```
Get-New
```

# Enumeración Básica Powershell

## Usar Get-Help

Este comando nos da información sobre los cmdlets. 

```
Get-Help Command-Name -Examples
```

```
Get-Help Get-Command -Examples
```

## Usar Get-Command

Este comando nos dice todos los cmdlets instalados en el ordenador. 

```
Get-Command Verb-* o Get Command *-Noun
```

```
Get-Command New-*
```

## Manipulación de Objetos

El | se usa para pasar de output a otro cmlet. 

```
Verb-Noun | Get-Member
```

Un ejemplo para ver la nos miembros 

```
Get-Command | Get-Member -MeberType Method
```

## Crear objetos de los anteriores cmdlest

Una forma de manipular objetos es porner fuera las propiedades del output del cmdlet y crear un objeto. Esto se hace usando el Select-Object

Ejemplo: Listar directorios seleccionados por el modo y el nombre

```
Get-ChildItem | Select-Object -Property Mode, Name
```

- first
- last
- unique
- skip

## Filtrar objetos

Cuando nos devuelves objetos, queremos seleccionar valores especificos. Podemos usarlo con Where-Object para filtrar valores

```
Verb-Noun | Where-Object -Property PropertyName -operator Value

Verb-Noun | Where-Object {$_.PropertyName -operator Value}
```

Ejemplo: Ver servicios parados

```
Get-Service | Where-Object -Property Status -eq Stopped
```

## Clasificar objetos

Cuando el output nos da tanta información que no podemos hacerlo eficientemente deberemos utilizar el cmdlet Sort-Object

```
Verb-Noun | Sort-Object
```

Ejemplo: Usarlo para listar directorios

```
Get-ChildItem | Sort-Object
```

Práctica:

Buscar un archivo especifico

```
Get-ChildItem "*interesting-file.txt*" -Path C:\ -Recurse
```

```
C:\Program Files
```

Especificar el contenido de un archivo

```
Get-ChildItem "*interesting-file.txt*" -Path C:\ -Recurse | Get-Content
```

```
notsointerestingcontent
```

Coger un Hash md5 de un archivo

```
Get-ChildItem "*interesting-file.txt*" -Path C:\ -Recurse | Get-FileHash -Algorithm MD5
```

```
49A586A2A9456226F8A1B4CEC6FAB329
```

Comando para ver el directorio actual

```
Get-Location
```

Comando para ver las consultar web

```
Invoke-WebRequest
```

# Enumeración

El primer paso cuando ganamos acceso inicial es enumerar

- Usuarios
- Información basica de red
- Permisos de archivo
- Permisos de registro
- Tareas y procesos
- Archivos inseguros

Usuario de la máquina

```
Get-LocalUser
```

```
5
```

SID usuario

```
Get-LocalUser | Select-Object *
```

```
Guest
```

Usuarios con contraseña flase

```
4
```

Grupos existentes

```
Get-LocalGroup
```

```
24
```

Saber mi IP

```
Get-NetIPAddress
```

Puertos en escucha

```
Get-NetTCPConnection | Where-Object {$_.State -eq “Listen”}
```

Puerto en especifico

```
Get-NetTCPConnection | Where-Object {($_.State -eq “Listen”) -and ($_.LocalPort -eq “445”)}
```

Ver parches aplicados

```
Get-HotFix
```

Ver fecha parche en especifico

```
Get-HotFix | Where-Object {$_.HotFixID -eq “KB4023834”} | Select-Object * | Select-Object InstalledOn
```

Buscar archivos backup por todo el sistema

```
Get-ChildItem “*.bak*” -Path C:\ -Recurse -ErrorAction SilentlyContinue | Get-Content
```

Buscar todos los archivos que puedan contener una API_KEY

```
Get-ChildItem -Path C:\Users -Recurse -ErrorAction SilentlyContinue | Select-String “API_KEY”
```

Listar todos los procesos

```
Get-process
```

Ver ACLs de un archivo

```
(Get-Acl -Path “C:\”).Owner
```

## Scripting Básico en PowerSHell

Ahora que entendemos un poco de comandos y hemos hecho prácticas, vamos a intentar escribir y ejectuar un script mas complejo y que podamos hacer acciones

Vamos a usar el Powershell ISE (el editor) Vamos a utilizar un escenario muy particulas.

Quermeos que nos de la lista de puertos, para ver si los puertos que queremos estan abiertos

Estos scripts usan la extensión .ps1

```
$system_ports = Get-NetTCPConnection -State Listen

$text_port = Get-Content -Path C:\Users\Administrator\Desktop\ports.txt

foreach($port in $text_port){

    if($port -in $system_ports.LocalPort){
        echo $port
     }

}
```

En la primera linea, nosotros queremos la lista de los puertos del sistema que estan abierto. Usamos el comando anterior "Get-NetTCPConnection". Guardamos el output en una variable.

```
$variable_name = value
```

En la siguiente linea, leemos la lista de puertos del fichero "port.txt" usamos el Get-Content. Otra vez guardamos dicha variable

El siguiente paso es iterar todos los puertos en un archivo y ver los puertos abiertos. Para hacer esto usaremos

```
foreach($new_var in $exiting_var){}
```

Este código particula se usa para hacer un loop (bucle) de seleccion de objetos. Una vez queremos un puerto en particula, nosotros podemos comprobar si estos puertos estan localmente. Haciendo otro bucle,  usamos el parametro if con el operador -in para chequear si los puertos existen y tienen la propiedad "LocalPort" en el objeto. 

Lista completa de todos los operadores [Aqui](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_comparison_operators?view=powershell-7.4&viewFallbackFrom=powershell-6)

Ahora que sabemos lo básico en script, es el momento de crear nosotros uno mismo.

Ejemplo: Imagina que tienes un arhcivo con emails en el escritorio que contiene copias de email de John, Marta y Maria se han estado enviado copias entre sí.

[Recurso](https://learnxinyminutes.com/powershell/)









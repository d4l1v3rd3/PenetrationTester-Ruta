![image](https://github.com/user-attachments/assets/b4a40264-fed5-48da-8181-5ce1e368f44b)# Active Directory Basics

- Que es Active Directory
- Que es un dominio de active directory
- Que componentes estan dentro del dominio de Active Direcry
- Forense y verificación de dominio

# Dominios de Windows

Imaginemos que tenemos que administrar una pequeña empresa de una red con solo 5 ordenadores y 5 empleados. Una pequeña red. Podriamos sin ningún problema configurar cada ordenador por seaprado. Manualmente, crear usuarios, configuraciones...

Pero imagina, que tienes 157 ordensdores y 300 usuarios locales en diferentes oficinas. 

Esto es una gran limitacion, para eso existen los dominios de Windows. Simplemente un dominio de windows es un grupo de usuarios y ordeandores bajo la adminsitración de la empresa. La idea del dominio y centraliuzar todo se llama Active Directory (AD) el servidor se ejectura y sirve como Domain Controller (DC)

![image](https://github.com/user-attachments/assets/9df6e7df-3af8-4817-bf93-a03dcfb2b93c)

### Ventajas:

- Gestión de identidad centralizada: Todos los ursuarios de lared estan configuracion del AD
- Politicas de seguridad: Podemos configurar politicas de seguridad directamente de AD y aplicar a los otros usuarios o ordenadores a través de la red.

# Active directory

El corazon de un dominio de windows es el Ative Directory Domain Service (AD DS) Este servicio cataloga la información de todos los objetos existentes en la red. Usuarios, grupos, máquinas, impresoras, etc..

### Usuarios

Los usuarios son el objeto más común de AD, es uno de los objetos que mas centrados tenemos que estr, tienen que tener una autenticación y privilegios dependiendo a que recurso como archivos o impresoras.

- Gente: Los usuarios normalmente representan personas de la organización que tienen acceso a la red.
- Servicios: Puedes defiinir usuarios como servicios "ISS" o "MSSQL"

### MÁQUINAS

Máquinas es otro tipo de AD, es cada ordenador que esta en el AD, un maquina se puede crear como objeto, asiganda a una cuenta de un usuario regular. La cuenta aveces tiene limitaciones con el dominio.

La cuentas de la máquina puede tenenr administradorfes locales asignados al ordenado., si tienes una contraseña puedes loguearte en cualquier máquina que este dentro del dominio.

### Grupos de seguridad

Si hemos utilizado un poco windows, sabemos que hay usuarios que se definen dependiendo a que accesos a recursos. Es una forma de tener mejor manejo podemos añadir usuarios a dicho grupo.

![image](https://github.com/user-attachments/assets/199f3ebe-2154-4a7e-91b9-0fb95c600c28)

## AD usuarios y ordenadores

Para configurar usuarios, máquinas o grupos en AD, nostros necesitamos loguearnos en el controlador de dominio y ejectura "Active Directroy useras and Computers"

![image](https://github.com/user-attachments/assets/650600a5-0148-4ac0-a054-8974af298983)

Nos abrira la dicha ventana y podremos interactuar.

![image](https://github.com/user-attachments/assets/cabc9291-71a8-481b-b335-52e235b0c521)

Si vemos, hay contenedores creador por windows automaticamente.

- Builtin: Contieene los grupos por defecto de cualquier windows
- Computers: Cualquier mauqin que se una a la red se ponde por defecto. Podemos movernos si queremos.
- Domain Controllers: Contenedores de organización de unidad contiene los controladores de dominio de la red
- Users: Usuarios por defecto y grupos que se meten al dominio por defecto
- Managed Service Accounts: Cuentas que se usan como servicio en los dominios de Windows.

## Grupos de Seguridad vs OUs

- Ous se usan para aplicar políticas de usuarios y dispositivos, incluyen configuraciones especificas que pertencen a los usuarios dependeidno del rol que tenga en la mepresa.
- Grupos de seguridad se usan para garantizar permisos sobre recursos. Por ejemplo, si queremos que los grupos tengan acceso al mismo qeu los usuarios hacemos una carpeta compartida.

# Gestión de usuarios en AD

![image](https://github.com/user-attachments/assets/6c7e5636-0caa-4b35-8b29-75dfd63a2690)

## Borrar extra OUs y usuarios

Lo primero que tenemos que darnos cuenta es borrar cosas repetidas o no funcionales

![image](https://github.com/user-attachments/assets/b21ef370-45f9-406a-8547-4391c15e6c49)

Por defecto, las Ous estran protegidas para borrados accidentales. Para borrarlo simplente nor iremos a lo avanzado 

![image](https://github.com/user-attachments/assets/267d09fb-fe98-4c35-9554-15b61b36ca0b)

![image](https://github.com/user-attachments/assets/faf59520-7566-41f8-947c-6cc3486a750e)

Esto hara que podamos ver las propiedades de los contenedores

## Delegar

Otra cosa importante es dar especificos control sobre usuarios o OUs. El proceso de delegacion garantiza privilegios especificos.

Lo mas comun de todo esque los privilegios lo den el grupo "IT support". 

Por ejemplo queremos delegar el control de sales

![image](https://github.com/user-attachments/assets/29353d48-bfff-40ae-91fb-d33b24a8f851)

Nos abrira una nueva ventana y podremos chekear los nombres a lo que quermeos delegar

![image](https://github.com/user-attachments/assets/f87b6516-4ced-4e85-8369-e28ff6fef0c7)

Clicamos ok

![image](https://github.com/user-attachments/assets/243ef73c-ba58-4934-ac70-18c946a83b8f)

En caso de no tener privilegios podemos utilizar comandos de powershell
```
Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
```

```
Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose
```

# Gestion de ordenadores en AD

Por defecto todas las máquinas que estan en dominio se añaden al contenedor "coputers" 

![image](https://github.com/user-attachments/assets/5bee5fa3-66d0-4456-8bb8-1fda21f32745)

Podemos ver servidores, pc de gente, etc..

## Workstation

Son los dispositivos mas comunes que contienen dominios de active directory. Cada usuario de dominio se loguea en su espacio de trabajo "workstation" 

## Servers

Son dispositivos comunes que se utilizan en el dominio, generalmente dan servicios a los usuarios

## Controladores de dominio

Son los terceros mas utilizados y funcionan para manejar el dominio de AD, para las partes mas sensibles de los dominios

# Politicas grupales

Una vez que tenemos todos los ordenadores y usuarios organizados podemos empezar con las diferentes politicas.

Las politicas en windows son (GPO) Group Policy Objects. Es una colección de configuraciones que se aplican a las Ous 

Para configuirar - "gruoup policy management"

![image](https://github.com/user-attachments/assets/623810ac-782d-435b-90da-45605d81182a)

![image](https://github.com/user-attachments/assets/855439d3-bf19-4e47-82eb-45aa0533a697)


Para cambiar la longitud de la contraseña

```
Computer Configurations -> Policies -> Windows Setting -> Security Settings -> Account Policies -> Password Policy
```

![image](https://github.com/user-attachments/assets/fb0c3a89-ab33-48df-a08e-3440569b07d6)

# Metodos autenticación

CUando usamos los dominios de windows, todas las credenciales se guardan en los controladores de dominio. Sin embargo el usuario tiene que autenticarse para usar los servicios del domino. 
Tenemos dos protocolos:

- Kerberos: Usando para las versiones mas recientes de Widnows. Protocolo por defecto
- NetNTLM: Autenticación antigua protocolo compatible.

## Kerberos Authentication

Kerberos es la autenticación por defecto o el protcolo usado para las versiones mas recientes de Windows. Los usuarios se loguean para usar dicho servicio asignado por tickets. Piensa que los tickets es el paso anterior de la autenticación. 

Para la autenticación de kerberos funciona a sí:

1- El usuario manda el usuario y la contraseña para la pass se usa una key Distribution Center (KDC), el servicio usualmente se mete y carga en la red Kerberos.

El KDC crea y manda de vuelta un ticket, que da al usuario un acceso a los servicios especificos. La necesidad del ticket se hace para que no tenga que estar poniendo las  credenciales cada vez que se conecte al servicio, para eso esta la "Session Key" que se le da a un usaurio, par no generar mas consultas.

Por supuesto todo esta encriptado usando "krbtgt" 

![image](https://github.com/user-attachments/assets/9589815c-f9c4-45d4-90c4-624ccae7b2a2)

2. Cuando el usuario quiere conectarse a un servicio o recurso compartido, el TGT le pregunta al TGS (ticket) porque el ticket solo sirve para el servicio especifico. El usuario simplemente manda su usuario y su sesion encriptada y indica el acceso que quiee.

El KDC manda otra Service serssion key, y le autnetifica. 

![image](https://github.com/user-attachments/assets/7d86ca5e-7331-4cc5-95f4-41d58cae7abb)

3. El thhs manda el servicio y la autenticaión para entablar la conexión.

![image](https://github.com/user-attachments/assets/45935ae8-0246-43b8-82f8-359db40d2e9e)

## NetNTLM Authentication

![image](https://github.com/user-attachments/assets/98967519-2474-4d3f-97cf-da11d10020a6)

Usa un reto-respuesta como mecánismo

1. El cliente manda una consulta para autentificarase al servidor.
2. EL servidor genera un numero aleatorio y envia un reto al cliente
3. El cliente combina la contraseña haseada con el reto y se genera una respuesta para el reto y el servidor verifica
4. El servidor le llega la respuesta y responde al domain controller para verificar
5. El conotrolador de dominio se usa como reto para recalcular la respuesta y comparar con la original que mando el cliente. El cliente se autentifica o no
6. El servidor el manda una respuesta definitiva

# Arboles bosques y confianzas

## Árboles

![image](https://github.com/user-attachments/assets/b4c2e8e3-1de1-43ee-83e8-a392a3ea8f61)

## Bosques

![image](https://github.com/user-attachments/assets/62b8c821-7959-487d-843d-52bd6e0f8fac)

## Relaciones de confianza

![image](https://github.com/user-attachments/assets/30b4ce93-d02d-4a99-83cd-51bb06ebc0ff)



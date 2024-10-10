# Active Directory Basics

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

Otra cosa importante son 




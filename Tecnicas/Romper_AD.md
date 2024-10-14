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


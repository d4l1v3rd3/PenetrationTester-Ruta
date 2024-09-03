<h1 align="center"> Command AND Control </h1>

# INTRODUCCIÓN

Command and Control (C2) Sus frameworks son esenciales para los equipos de Red Team y sistemas avanzados. Ellos haces mas facil el manejo de los dispositivos controlados.

- Como funcionan los frameworks de C2
- Los varios componentes que se usan
- Como poner a punto un framework de C2
- Usar Armitage o Metasploit para C2
- Como administrar C2
- Consideraciones mientras administramos

# COMMAND AND CONTROL ESTRUCTURA

Mientras intentamos digerir los componentes de C2, que parece intimidatorio. Sin embargo, esto no es asi vamos a entenderlo desde el nivel más basico, piensa en un Netcat como servidor siendo capaz de dar multiples shells llamadno a los agentes.

Es un servidor, muchos frameworks de C2 requieren generadores de payloads. Esto usualmente se hace para construir el mismo framework. Por ejemplo, Metasploit es un Framework de C2 con su propio payload generator, MSFVenom.


![image](https://github.com/user-attachments/assets/b820f611-138d-41c7-bc39-ca1f59d69ec7)

Entonces, exactamente que hace?

Implementa sesiones de netcat por ejemplo, herramientas de post explotación

## ESTRUCTURA

C2 Servidor

Para entender el framewokr, nosotros deberemos empezar por entender los componentes, y el servidor es muy importante, porque guarda a los agentes periodicamente esperando para ejecutar comandos.

![image](https://github.com/user-attachments/assets/c026b6fa-df60-4e7d-9dad-d978253da219)

### AGENTES / PAYLOADS

Un agente es un programa que genera el mismo C2 framework, que devuelve un listener al sevidor. Estos agentes activan funcionalidades comparadas con una reverse shell normal. Los frameworks que mas se implemente pueden hacer comandos o operaciones para hacer mucho más facil la vida al operador.

Muchos ejemplos de comandos como descargas o subidas al sistema, son fácilmente configurables.

### LISTENERS

En el nivel más básico, el listener es una aplicacion que corre en el sevridor esperando en un puerto o protocolo especifico como DNS, HTTP

### BEACONS

El beacon es el proceso del agente devolviendo el listener que corre el servidor

## OFUSCAR AGENTES

Una gran cosa de los analistas de seguridad, son los anti-virus o firewalls nuevos, cuando intentan identificar un C2 o el trafico entre ello. El firewall puede observar el trafico. (podemos utilizar sleep timers)

Los jitters tambien hacen que se duerman

```
import random
sleep = 60
jitter = random.randit(-30,30)
sleep = sleep + jitter
```

## TIPOS DE PAYLOADS

Como una reverse shell, hay dos tipos de payloads, los staged y lo stageless

## STAGELESS

Son simples, contienen al agente y al servidor y inmediatamente se conectan

![image](https://github.com/user-attachments/assets/bcb42ef0-23ad-4981-8eb1-a72fc2928c39)

## STAGED

Requieren una llamada se descarga el archivo el servidor y el agente debe llamarlo.

![image](https://github.com/user-attachments/assets/2e91e1a8-b9b5-4155-8ad3-2ff26ef0500d)

## FORMATOS DE PAYLOADS

- Powershell Scripts
- HTA files
- JScript files
- VIsual basic
- Microsoft office

## MODULOS

Modulos son componentes del mismo framwork, los agentes se los añaden para que el servidor sea mas flexible. Dependiendo del framework, los scripts pueden estar escritor en otros lenguajes.

CObal Strike estan en "aggresor Scripting language" 

PowerShell empire suporta muchos lenguales, metasploit esta en Ruby

## MODULOS POST EXPLOTACION

Simplemente son modulos que se utilizan luego del acceso

## PIVOTING

Los mayores componentes son los de pivoting, hacen un fácil acceso a zonas restringidas de la red.

![image](https://github.com/user-attachments/assets/2244b4d3-17bd-4bb6-b6b5-5d5b504f7c94)


## LUCHA CONTRA EL MUNDO

Las fronteras de dominio por ejemplo, usan Cloudfare para las conexiones HTTP. 

![image](https://github.com/user-attachments/assets/d7df9f0a-e3c3-44eb-a913-2256689426f9)


# FRAMEWORKS COMUNES DE C2

Como todo en la vida

- Gratis
- De pago

Porque usariamos frameworks de pago? Normalente son porque son más dificiles de identificar para los vendedores de Anti-Virus. No es imposible, pero date cuenta que un open source es fácil de entender pero uno de pago no lo tiene todo el mundo.

Usualmente, los frameworks premiums generan exploits modulos más avanzados, pivoting o consultas que los opn-source pues no van a estar ahi. Por ejemplo Cobalt Strike ofrece frameworks que no tienen la capacidad de abrir un tunel VPN. 

## METASPLOIT

Por ejemplo este de Rapid7.

## ARMITAGE

Es una extension de metasploit, añade una interfaz gráfica escrita en Java, increiblemente similar a Cobalt Strike. Porque su desarrollador es Raphael Mudge.

## PORWERSHELL EMPIRE

## COVENANT

## SLIVER

# DE PAGO

## COBALT STRIKE

## BRUTE RATEL

# INICIAR UN SERVIDOR C2 FRAMEWORK

## DESCARGAR

```
git clone https://gitlab.com/kalilinux/packages/armitage.git && cd armitage
bash package.sh
cd ./release/unix/ && ls -la
systemctl start postgresql && systemctl status postgresql
msfdb --use-defaults delete
msfdb --use-defaults init
cd /opt/armitage/release/unix && ./teamserver YourIP P@ssw0rd123
cd /opt/armitage/release/unix && ./armitage
```
Siempre la interfaz normalmente esta en local por el puerto 55553 (127.0.0.1)







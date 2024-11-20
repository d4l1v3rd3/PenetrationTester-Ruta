<h1 align="center"> Vulnerabilidades </h1>

<p align="center"> <img src="https://github.com/user-attachments/assets/52c73772-8740-4cf4-b059-4131e2947092"></p>

# ÍNDICE

# INTRODUCCIÓN

Aprenderemos que es exactamente una vulnerabilidad, tipos de vulnerabilidades y como podemos explotarlas y como penetration tester testearlas.

- Que vulnerabilidades hay
- Porque es rentable aprender sobre esto?
- Como se califican las vulnerabilidades
- Bases de adtos para buscar vulnerabilidades
- Como se buscan y como se usan


# INTRODUCCIÓN A LAS VULNERABILIDADES

Una vulnerabilidad en ciberseguridad se define como una falla en el diseño, o implementación se un sistema o aplicación. El atacante se aprovecha de ese fallo para ganar acceso desautorizado gracias al exploit.

El termino "vulnerabilidad" tiene muchas definiciones en ciberseguridad. Sin embargo, hay pocas variaciones.

Por ejemplo, el NIST dice que una vulnerabilidad "es un fallo en al información del sistema, de los procedimientos, controles internos o implementación"

Las vulnerabilidades se pueden organizar por muchos factores, incluyendo un mal diseño de la aplicacion o acciones intencionadas de los usuarios.

Veremos varías vulnerabilidades

Vulnerabilidad | Decripción
---- | ---- 
Sistema operativo | Se crean en el sistema operativo (OS) y normalmente resulta para escala de privilegios. |
Mala configuración | Este tipo de vulnerabilidades ocurren por una mala configuración en la aplicación o servicio |
Malas o credenciales por defecto | Cuando hay un elemento de autenticación con credenciales por defecto. | 
Lógica de aplicación | Mal diseño de las aplicaciones |
Factor Humano | Emails con phising, trucos a humanos etc | 

# PUNTUAR VULNERABILIDADES (CVSS Y VPR)

La gestión de vulnerabilidades es el proceso de evaluarlas, categorizarlas y por ultimo remediarlas cara la organización, es totalmente imposible parchear cada vulnerabilidad en al red o de cada ordenador de lsistema porque aveces es un gasto de recursos.

Aproximadamente el 2% de las vulnerabilidades se pueden explotar, pero las vulnerabilidades mas peligrosas se reducen mucho a la hora de explotar el sistema.

Esto se utiliza para determinar el riesgo potencial o impacto que puede tener la vulnerabilidad en la red o en un sistema. por ejemplo un "Common Vulnerability Scoring System" (CVSS) se basa en puntos, disponibilidad y lo fácil que es reproducirlo.

## Common Vulnerability Scoring System

1 - Como de facil es explotar esta vulnerabilidad?
2 - Existen exploits para esto?
3 - Como interfiere esta vulnerabilidad en el triado CIA

![image](https://github.com/user-attachments/assets/b07a482a-9fea-4b83-aeba-686266bf18ec)

## Vulnerability Priorty Rating (VPR)

El framework VPR es un framework moderno, desarrollado por Tenable, este framework se considera impulsado por riesgo, dando puntuación y centrandose para el riesgo que tiene la vulnerabilidad cara la organización.

![image](https://github.com/user-attachments/assets/667e92f1-9c11-45e9-a779-6e67536743d6)

## BASES DE DATOS DE VULNERABILIDADES

Alrededor de la carrera de ciberseguridad, utilizaremos multiples aplicaciones y servicios. Por ejemplo, un CMS tiene los mismos propositos, pero con diferentes diesños y diferentes vulnerabilidades.

Gracias aesto, los recuross de internet o vulnerabilidades, nos vendran muy bien.

[NVD (National VUlnerability Database)](https://nvd.nist.gov/vuln)

[Exploit-DB](https://www.exploit-db.com)

![image](https://github.com/user-attachments/assets/7a23d836-1899-4c34-b648-8e69a444680e)

## NVD - National Vulnerability Database

El "NVD" es una web que publicas listas categorizando vulnerabilidades. En ciberseguridad, se clasifican con "Commong Vulnerabilities and Exposures" (CVE"

Estas CVEs tienes formatos como "____-YEAR-IDNUMBER" por ejemplo "CVE-2017-0144"

![image](https://github.com/user-attachments/assets/abfc1dd2-87c2-4d57-ba97-51ca1549d7ad)

## EXPLOIT-DB

Exploit-Db es un recurso, para hacekers, que ayuda a entender todo un poco más. 

![image](https://github.com/user-attachments/assets/05a9e51b-cde3-4fb1-a1de-30e7a8bd218c)

# EXPLOTAR VULNERABILIDADES

## AUTOMATICOS VS MANUAL

Hay unas cuantas herramientas y servicios disponibles cara la el escaneo de vulnerabilidades. En rango comercial o open-source.

Veremos Nessus

![image](https://github.com/user-attachments/assets/8139f766-6308-43ae-b4d8-8b1b32d296af)

Por ejemplo, un escaner de vulnerabilidades "Nessus" es gratis pero tenemos una edición comercial. La version cuests y se usa para organizaciones que son penetration testers.

![image](https://github.com/user-attachments/assets/47f3cd76-0315-4396-ac82-7196678c86b9)

Utiliza frameworks como Metasploit para escaneres o exploits utilziando sus modulos

Los escaneos manuales aveces son una gran arma pra los PT para testear aplicaciones o programas individuales. Un escaneo manual puede buscar las mismas vulnerabilidades que uno automatico.

![image](https://github.com/user-attachments/assets/26c49def-9f7c-40d7-be08-d8ae1d0149db)


## BUSCAR EXPLOITS MANUALMENTE

### RAPID 7

Otros servicios que da Exploit DB y NVD es un buscador en la base de datos.

![image](https://github.com/user-attachments/assets/f77d9ab0-8dbe-4c7b-be3d-cd551ef3eb25)

Adicionalmente la base de datos contiene instrucciones de como usarla.

### GITHUB

Es un sitio web muy popular que utilizan desarrolladores para diseñar software. Se usa para hostear o guardar codigo de aplicaciones. Para nostros esto nos viene increible y muchos vienen con PoC's (Proof of concept)

![image](https://github.com/user-attachments/assets/c3e5032d-e6b8-41d0-bf6d-5d4730d7f6f2)

### Serachsploit

Es una herramienta disponible en sistemas Linux.

![image](https://github.com/user-attachments/assets/720dbfc1-4c89-4d8d-9a1c-ddf06096c849)


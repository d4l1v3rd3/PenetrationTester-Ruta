# Introducción

AV es un software esencial basado en soluciones de seguridad disponibles para detectar y prevenir ataques de amlware. Consiste en varios modulos, deteciones, técnicas, etc.

# Objetivos de aprendizaje

- Que es un antivirus
- Antivirus detecciones
- Enumerar software instalado
- Testear un entorno simnulado

# Antivirus

Antivirus software es una capa extra de segurdad con la capacidad de detectar y ejecutar código malicioso

Es una aplicacion basada en ejecuciones en tiempo real, dond se monitoriza y checkea las nuevas descargas de archivos. El software de AV inspecciona el dispositivo y dice que archivos son maliciosos usando difernetes técnicas.

# Características

El motor de lAV es el responsable de vuscar y eliminar el código malicioso de los ficheros inclutyendo:

- Escáner
- Técnicas de detección
- COmpresores y archivos
- Descompiladores
- Emuladores

# AV Static Detection

Es una técnica que simplemente es una tipo de detección basada en firmas, simplemente detecta mira la firma en MD4, SHA o lo que sea y te dice en la base de datos si se considera negativa o no.

![image](https://github.com/user-attachments/assets/74514d13-5e6e-4e4c-bf7f-a80d534dc6f8)

## Crear nuestra propia base de datos de firma

Generamos un MD5 Hash

![image](https://github.com/user-attachments/assets/dbaefa39-c83d-418c-94af-446e99e73e68)

Generamos una base de datos

```
"c:\Program Files\ClamAV\sigtool.exe" --md5 backdoor2.exe > thm.hdb
```

Escaneamos de nuevo un archivo

```
C:\Users\thm\Desktop\Samples>"c:\Program Files\ClamAV\clamscan.exe" -d thm.hdb backdoor2.exe
Loading:     0s, ETA:   0s [========================>]        1/1 sigs
Compiling:   0s, ETA:   0s [========================>]       10/10 tasks

C:\Users\thm\Desktop\Samples\backdoor2.exe: backdoor2.exe.UNOFFICIAL FOUND
```

## Reglas de Yara 

Yara es una herramienta de malware que clasifica y detecta malware. basada en rule-based detecta malware, podemos crear reglas

```
C:\Users\thm\Desktop\Samples>strings AV-Check.exe | findstr pdb
C:\Users\thm\source\repos\AV-Check\AV-Check\obj\Debug\AV-Check.pdb
```

```
rule thm_demo_rule {
	meta:
		author = "THM: Intro-to-AV-Room"
		description = "Look at how the Yara rule works with ClamAV"
	strings:
		$a = "C:\\Users\\thm\\source\\repos\\AV-Check\\AV-Check\\obj\\Debug\\AV-Check.pdb"
	condition:
		$a
}
```

```
C:\Users\thm>"c:\Program Files\ClamAV\clamscan.exe" -d Desktop\Files\thm-demo-1.yara Desktop\Samples
Loading:     0s, ETA:   0s [========================>]        1/1 sigs
Compiling:   0s, ETA:   0s [========================>]       40/40 tasks

C:\Users\thm\Desktop\Samples\AV-Check.exe: YARA.thm_demo_rule.UNOFFICIAL FOUND
C:\Users\thm\Desktop\Samples\backdoor1.exe: OK
C:\Users\thm\Desktop\Samples\backdoor2.exe: OK
C:\Users\thm\Desktop\Samples\eicar.com: OK
C:\Users\thm\Desktop\Samples\notes.txt: YARA.thm_demo_rule.UNOFFICIAL FOUND
```

![image](https://github.com/user-attachments/assets/340167eb-87f9-4aa5-b364-1e63381e0ca7)

# Otras técnicas de detección

## Detección Dinámica

La detección dinamica es más avanzada y que la detección estatica. Se centra en checkear que los archivos no hagan cosas que no deberían hacer

## Heuristic

Se basa en los productos modernos. Puediendo ser en static, descomprimiendo o dinamica.

Hashes NLRM, tickets de kerberos, etc.. esto sería el dinamico

![image](https://github.com/user-attachments/assets/75fa6c1e-31ad-43d1-a608-6d762292d61e)


# AV testing y fingerprinting

## Virus total

![image](https://github.com/user-attachments/assets/ed58c8a0-cf8c-4f44-b07b-dae17fd225b6)

c checker

sharpedrchecker




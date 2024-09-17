# WEAPONIZATION

Weaponization es el segundo paso de el modelo Cyber Kill Chain. En este paso, el atacante genera y desarrolla su propio codigo malicioso para ejecutar el payload, como un word, pdf etc. 

Muchas organizaciones utilizan el sistema operativo Windows, osea que es un buen target, Normalmente las organizaciones bloquean la ejecución de .exe por temas de seguridad, los red teames construyen payloads o campañas de phising, ingeniera social, explotación de software, USB o metodos web.

El siguiente grafico es un ejemplo de como funcionaria.

![image](https://github.com/user-attachments/assets/3f45f14e-2d34-4467-bfe1-0b93b0ce810a)

[GITHUB REDTEAM TOOLS](https://github.com/infosecn1nja/Red-Teaming-Toolkit#Payload%20Development)

```
xfreerdp /v:10.10.109.131 /u:thm /p:TryHackM3 +clipboard
```

# Windows Scripting Host - WSH

Windows scripting host es una herramienta administrativa de windows ejecuta archivos automaticamente y administra herramientas con el sistema operativo.

"cscript.exe" (linea de comandos) y "wscript.exe" (Interfaz), son responsables de ejecutar varios scripts basicos de microsoft, incluyengo "vbs" y "vbe".

Vamos a escribir un codigo simple de "VBScript" para crear un cuadro de mensaje en windows

```
Dim messsage
message = "bienvenidos"
MsgBox message
```

En la primera linea, declaramos el "message" variable usando "Dim" donde el valor "BIenvenido" en la variable "message" 

![image](https://github.com/user-attachments/assets/6e73cd59-d2a4-4fb5-97bc-cb0946ce3296)

Ahora vamos a ejecutar estos archivos. 

```
Set shell = WScript.CreateObject("Wscript.Shell")
shell.Run("C:\Windows\System32\calc.exe " & WScript.ScriptFullName),0,True
```

```
c:\Windows\System32>wscript c:\Users\thm\Desktop\payload.vbs
c:\Windows\System32>cscript.exe c:\Users\thm\Desktop\payload.vbs
```

![image](https://github.com/user-attachments/assets/32118e44-fa0c-48e4-be22-40ec5db618a8)

```
c:\Windows\System32>wscript /e:VBScript c:\Users\thm\Desktop\payload.txt
```


# Objetivos de Aprendizaje

1. Aprender como evadir detecciones modernas usando herramientas de ofuscación
2. Entender los principios de ofuscación y origenes de protección intelectual
3. Implementar metodos de ofuscacion para ocultar funciones maliciosas

# Origen de Ofuscación

Se usa en muchos casos normalmente para proteger IPs y otras propiedades de información que contiene una aplicación.

Por ejemplo, para el Minecraft y ofuscar funciones de Java. Minecraft tiene los mapas ofuscandos para dar información limitada entre el traductor y las clases

![image](https://github.com/user-attachments/assets/46af5135-6105-4b57-b969-e3ccb67eff10)

Si desglobamos cada capa hacia abajo veremos que cada capa tiene un objetivo

![image](https://github.com/user-attachments/assets/30e906e3-5e18-4c2f-814b-362c16e86c3d)

Para usar la taxonomia, podemos determinar el objetivo y coger el metodo para nuestro requerimiento necesario. Por ejemmplo, si queremos ofuscar una capa de nuestro codigo pero no podemos modificar el codigo existnete. En este caso, necesitaremos injectar codigo

```
Code Element Layer > Obfuscating Layout > Junk Codes.
```

Como se puede usar esto maliciosamente? Rompen las firmas o analisis de programas y meter malware ofuscado

# Funciones de Ofuscacion para una Evasion Estatica

Dos de los límites considerados de la seguridad en el ambito de antivirus y EDR soluciones. 

Para evadir firmas, los actores malintencionados tienen un rango extenso con logica o reglas de sintaxis para implementar la ofuscación.

![image](https://github.com/user-attachments/assets/ff685a81-35e8-46db-bb5b-ef7744cbdfec)

# Concatenacion de Objetos

Es una programación muy común o concepto que combina dos objetos separados en uno 

```
>>> A = "Hello "
>>> B = "THM"
>>> C = A + B
>>> print(C)
Hello THM
>>>
```

Dependiendo del lenguaje utilizaremos uno o otro operador

![image](https://github.com/user-attachments/assets/b9575aab-1aa8-4b8b-996e-189709648a3e)

POrque es bueno para los atacantes? La concatenacion abre la puerta para vectores modificando simplemnete o manipulando aspectos de la aplicación. Los más comunes que se usan en malware van directos a las firmas estaticas. Los atacantes rompen todos los objetos y eliminan las firmas.

Esta de abajo es una regla de yara que concatenación para evadirla

```
rule ExampleRule
{
    strings:
        $text_string = "AmsiScanBuffer"
        $hex_string = { B8 57 00 07 80 C3 }

    condition:
        $my_text_string or $my_hex_string
}
```

Cuando compila el binario y escanea con Yara, se crea una aleta positiva. Usando concatenación, esta string cambia y cuando se escanea no da alerta

![image](https://github.com/user-attachments/assets/a78b86f7-40db-40be-a944-587d458f7fbc)

![image](https://github.com/user-attachments/assets/f25b1963-0d80-4a81-944f-53bef187d991)

# Función de ofuscacion para analisis

Depués de funciones básicas, estamos ya preparados para pasar los software de detecciones sucepitbles para analisis de humanos. 

![image](https://github.com/user-attachments/assets/5eb79f5b-3ac0-42a2-8d0a-973ba48d0b29)

# Flujo de código y Logica

El flujo de control es un componente critico de un programa ejecutado que define como el programa logicamente procede. Logica es uno de las cosas mas importantes que determina factores de un control de aplicacion y compone varios usos como if/else o loops

![image](https://github.com/user-attachments/assets/b1c961b7-03d7-4724-82c9-8487efe0ccc2)

```
x = 10 
if(x > 7):
	print("This executes")
else:
	print("This is ignored")
```

![image](https://github.com/user-attachments/assets/a5f04886-61e4-4034-8304-12e5427a6f03)

# Control Arbitrario

Para crear un control de flujo arbitrario tenemos que entender matematicas y logica y algoritmos complejos

Podemos predecir la complejidadd de la logia o los algoritmos matematicos. Alfinal para que el input sea un T/F.

Aplicando este concepto de ofuscacion, opcacas las predicciones usadas de control de output y input. 

```
x = 0
while(x > 1):
	if(x%2==1):
		x=x*3+1
	else:
		x=x/2
	if(x==1):
		print("hello!")
```



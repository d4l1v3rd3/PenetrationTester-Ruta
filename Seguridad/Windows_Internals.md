# Introducción

# Objetivos de aprendizaje

- Entender y interactuar con procesos de WIndows y entender las tecnologias
- Aprender sobre formatos de archivos y como se usan
- Interactuar con Windows Internals y entender como opera el Kernel de Windows

# Procesos

Un proceso mantiene y representa la ejecución de un programa; una aplicacion puede contener uno o mas procesos. Un proceso son un conjunto de componentes que se descomponen y interactuan entre sí. 

Un proceso tiene una dirección virtual, un código, puertos abiertos, objetos de sistema, contextos de seguridad, identificacdores, variables, clases, trabajos, ejecuciones, etc.

Aplicaciones por defecto:

- MsMpEng (microsoft Defender)
- wininit (Keyboard and mouse)
- Isass (Credential storage)

Componentes de los procesos

![image](https://github.com/user-attachments/assets/d1f78dea-29c3-4fdb-92d7-0972d6797915)

La de arriba se reside en un espacio virtual. La tabla de abajo reside en la memoria

![image](https://github.com/user-attachments/assets/d378feb0-5cb6-481e-bb74-5b2b954326a7)

Componentes que enseña la barra de tareas

![image](https://github.com/user-attachments/assets/1abb9203-5f6e-4590-a2b5-adb32a94c918)

# Trapos

Un thread es un ejecutable que tiene un proceso y una tarea basado en el factor de un dispositivo.

Los factores de un dispositivo varian en base a su CPU y especificaciones de memroia, prioridad y factores logicos

Mientras los trapos controlan la ejecución, es muy comun targetear un componente. El abuso de trapos se puede usar tu mismo en el código de una ejecución.

Los trapos guardan los mismos detalles y recursos de los procesos, como codigo, variables globales. Los trapos tienen valores unicos y datos.

![image](https://github.com/user-attachments/assets/b219d870-ced0-44d7-a4ca-8dd5efd0049d)

# Memoria Virtual

La memoria virtual es componente critico de como funciona windows por dentro e interactua. Este componente interactua con la memoria y si la memoria fisica sin que haya colisiones entre apliaciones. 

La memoria virtual da un proceso con un espacio virtual privado. Un controlador de memoria se usa para transaldar la dirección virtual. Esto se hace para dicho espacio no sea directamente escrito en la memoria fisica, pudiendo causar un daño.

Los controladores de memoria utilizan paginas o conjuntos de memoria:

![image](https://github.com/user-attachments/assets/098557f2-e74e-45b0-b1d7-fe1550f8ae93)

En teoria la cantidad maxima de espacio virtual son 4GB en 32bits

para los 64bits son los 256TB

# Librerias dinámicas

EL Microsoft Docs describe las DL "Libreria que contiene codigo y datos que se usan en los programas en el mismo tiempo"

DLLs se usan como funcionalidades mas importantes detrás de la aplicaciones ejecutando Windows. 

Cuando una DLL es ejecutada en una fucnion de un programa, el DLL asigna las dependencias. Como el programa depende de las DLL, los atacantes ponen como target las DLLs controlando las aplicaciones y los aspectos de ejecución y funcionalidad.

- DLL Hijacking
- DLL Side-Loading
- DLL Injection

DLL: 

```
#include "stdafx.h"
#define EXPORTING_DLL
#include "sampleDLL.h"
BOOL APIENTRY DllMain( HANDLE hModule, DWORD ul_reason_for_call, LPVOID lpReserved
)
{
    return TRUE;
}

void HelloWorld()
{
    MessageBox( NULL, TEXT("Hello World"), TEXT("In a DLL"), MB_OK);
}
```

# Formato portable ejecutable

Los ejecutables y aplicaciones son largas porticiones de como windows funciona por dentro en alto nivel. los PE (Portable Executable) define la información sobre los ejecutables y datos guardados. El formato PE define la estructura de como los datos son guardados.

![image](https://github.com/user-attachments/assets/acd90ac7-7bc8-43b0-b53c-b407c904ccb8)

Los DOS headers definen el tipo de archivo

Definen por ejemplo el formato .exe

![image](https://github.com/user-attachments/assets/6145a926-df2c-4df3-b88b-5153d1e2dcac)

El DOS stub es un programa que se ejecuta predeterminadamanete ante sdel arhcivo par aver la compitibilidad no afecta a la funcionalidad

![image](https://github.com/user-attachments/assets/6856065b-f58a-4f10-a8d9-48517450e79c)

![image](https://github.com/user-attachments/assets/44038314-a2b8-4acc-bc48-bee2117519ad)

# Interactuar con Windows Internals

Interactuar con windows internals puede parecer desalentador, pero es bastante simple. Los lugares mas accesibles y opciones con interactuar windows nos da una interfaz API.

Los mayores componentes de internals con hardware y memoria

![image](https://github.com/user-attachments/assets/3a8f7482-21a0-4573-9149-570117279132)

![image](https://github.com/user-attachments/assets/f079d6a7-d2dc-42e6-991b-e445d800fbd0)




# Introducción

Las API de Windows dan una funcionalidad nativa a la hora de interactuar con los componentse clave de Windows. Las API son ampliamente usadas por muchos, incluyendo red teamers, actores maliciosos, blue teamers, desarrolladores de software.

La API integra sin problema al sistema Windows, ofrenciendo su gama de casos para cada uso. Solo hay que ver que el Win32 API se uso como una herramienta ofensiva y desarrollo de malware, EDR (Endpoint Detecetion & Response).

# Objetivos de aprendizaje

- Entender que son las API, casos de uso, como interactua con los susbsistemas del OS
- Aprender como implementar las API de Windows en diferentes lenguajes
- Entender como la API de Windows se usa con una perspectiva maliciosa y desglosar casos prácticos

# Subsistemas y Interacción de Hardware

Los programas aveces necesitan acceder o modificar subsistemas de Windows o hardware pero estan restriguindas para mantener la estabilidad de la máquina. Para arreglar este problema, Windows saco Win32 API, una libreria con una interfaz entre las aplicaciones de usuario y el kernel

Windows distingue el acceso al hardware en dos modos: User y modo kernel. Esto determinal el hardware, kernel y el acceso a la memoria y aplicaciones o drivers permitidos.

![image](https://github.com/user-attachments/assets/6a8ad962-32d6-49e5-ba3d-f4efd14300b0)

# Componentes de la API de Windows

La API de Win32, mas conocida como Windows API, tiene bastantes comoponentes dependientes que se usan para definir la estructura y organización de la API.

Vamos a desglosar la API

![image](https://github.com/user-attachments/assets/a676a328-d867-4f6f-980b-34ef2babdafe)

# Liberias OS

Cada API llama a la libreria de Win32 en la memoria y requiere un puntero a la dirección de memoria. El proceso se obtiene con punteros estas funciones se oscurecen porque la ASLR (Address Space Layout Randomization) se implementa, cada lenguaje o paquete tiene un unico procedimiento para el ASLR.

## Cabecera de Archivo Windows

Microsoft lanzo las cabeceras de archivo Windows, es una solución directa de los problemas asociados con la implementación ASLR. Este concepto de alto nivel, carga y determina como las llamadas se hacen y se crean en la tabla para aobtener la funcion de direccion o puntero.

Una vez "windows.h" se incluye en el top; la funcion de Win32 se llama

## P/Invoke

Microsoft describe P/Invoke como una plataforma para invoacar la tecnologia de acceso a estructuras.

Es una herramienta que cogue el proceso entero de invocar y sin necesidad de mantener el código o otras palabras, llama a la API e importa el DDL necesario para la funcion necesario

```
using System;
using System.Runtime.InteropServices;

public class Program
{
[DllImport("user32.dll", CharSet = CharSet.Unicode, SetLastError = true)]
...
}

```

En el cóidgo, nosotros importamos las DLL del user 32 usando el atribudo DLLImport

```
using System;
using System.Runtime.InteropServices;

public class Program
{
...
private static extern int MessageBox(IntPtr hWnd, string lpText, string lpCaption, uint uType);
}
```

# Estructura de llamadas de API

La llamada de la API es el segundo componente de la libreria Win32. Estas llamadas ofrecen flexibilidad y se usan en muchos casos.

Esquema:

![image](https://github.com/user-attachments/assets/550224d7-2e14-4501-b7d3-9b108ec189a4)

Cada llamada tiene una estructura pre definida. 

Por ejemplo la "WriteProcessMemory"

```
BOOL WriteProcessMemory(
  [in]  HANDLE  hProcess,
  [in]  LPVOID  lpBaseAddress,
  [in]  LPCVOID lpBuffer,
  [in]  SIZE_T  nSize,
  [out] SIZE_T  *lpNumberOfBytesWritten
);
```

# Implementaciones C API

Microsoft provee en bajo nivel lenguajes de programación como C y C++ con librerias preconfiguradas que se usan para llamar a APIs

La cabecera windows.h, anteirormente tratada, es usada para definir las llamadas de las estructuras y obtener las funciones de los punteros. Para incluir la cabecera windows, pretendemos que la linea sea c o C++

#include <windows.h>

Estructura

```
HWND CreateWindowExA(
  [in]           DWORD     dwExStyle, // Optional windows styles
  [in, optional] LPCSTR    lpClassName, // Windows class
  [in, optional] LPCSTR    lpWindowName, // Windows text
  [in]           DWORD     dwStyle, // Windows style
  [in]           int       X, // X position
  [in]           int       Y, // Y position
  [in]           int       nWidth, // Width size
  [in]           int       nHeight, // Height size
  [in, optional] HWND      hWndParent, // Parent windows
  [in, optional] HMENU     hMenu, // Menu
  [in, optional] HINSTANCE hInstance, // Instance handle
  [in, optional] LPVOID    lpParam // Additional application data
);
```

Vamos a coger estos parametros predefinidos y asignar los valores

```
HWND hwnd = CreateWindowsEx(
	0, 
	CLASS_NAME, 
	L"Hello THM!", 
	WS_OVERLAPPEDWINDOW, 
	CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, 
	NULL, 
	NULL, 
	hInstance, 
	NULL
	);
```

Hemos definido nuesstra primera llamada de API en C! vamos a implementar dentro de una aplicacion y usar esta funcionalidad.

```
BOOL Create(
        PCWSTR lpWindowName,
        DWORD dwStyle,
        DWORD dwExStyle = 0,
        int x = CW_USEDEFAULT,
        int y = CW_USEDEFAULT,
        int nWidth = CW_USEDEFAULT,
        int nHeight = CW_USEDEFAULT,
        HWND hWndParent = 0,
        HMENU hMenu = 0
        )
    {
        WNDCLASS wc = {0};

        wc.lpfnWndProc   = DERIVED_TYPE::WindowProc;
        wc.hInstance     = GetModuleHandle(NULL);
        wc.lpszClassName = ClassName();

        RegisterClass(&wc);

        m_hwnd = CreateWindowEx(
            dwExStyle, ClassName(), lpWindowName, dwStyle, x, y,
            nWidth, nHeight, hWndParent, hMenu, GetModuleHandle(NULL), this
            );

        return (m_hwnd ? TRUE : FALSE);
    }
```

# Implementaciones .NET y PowerShell

Para entender como P/invoke es implementado, vamos a ir abajo

```
class Win32 {
	[DllImport("kernel32")]
	public static extern IntPtr GetComputerNameA(StringBuilder lpBuffer, ref uint lpnSize);
}
```

La funcion de clase define la llamada de la API y definde una

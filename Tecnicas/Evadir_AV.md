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

1. .text
2. .data
3. .bss
4. .rdata
5. .edata
6. .idata
7. .reloc
8. .rsrc


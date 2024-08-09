<p align="right"><img height=100px width=100px src="https://github.com/user-attachments/assets/28eba669-a8dd-418a-bc8d-cc7c8e147edc"></p>

<h1 align="center">COMMAND EXECUTION</h1>

<p align="center"><img src="https://github.com/D4l1-web/PenetrationTester-Ruta/assets/79869523/05b77941-9070-4a7f-8cac-c422eedc2991"></p>


# ÍNDICE

- [PELIGROS](https://github.com/D4l1-web/PenetrationTester-Ruta/blob/main/Command_Execution.md#peligros)
- [PROTECCIÓN](https://github.com/D4l1-web/PenetrationTester-Ruta/blob/main/Command_Execution.md#protecci%C3%B3n)
- [CODE REVIEWS](https://github.com/D4l1-web/PenetrationTester-Ruta/blob/main/Command_Execution.md#code-reviews)

Si un atacante puede ejecutar código arbitrario en los sevidores, tus sistemas obviamente estas o van a ser comprometidos. Necesitamos tener una gran preocupación de como el servidor web interactua con el Sistema Operativo.

## PELIGROS

La injección de comandos es donde más seguridad necesitamos enfocar, es el ultimo paso para tener el control del sistema. Después de ganar acceso, el atacante puede intentar escalar privilegios en el servidor, instalar scripts maliciosos, o hacer una "botnet" al servidor.

## PROTECCIÓN

Si tu aplicacion coge servicios del mismo sistema operativo, tu necesitas estar seguro de los comandos y la seguridad en la aplicación, si no alguien que tiene intenciones maliciosas puede hacer instrucciones. Esta sección haremos formas para protegernos.

Intentemos que todos los comandos llamen a lo que tiene que llamar

Los programas de lenguaje modernos tienen interfaces para leer archivos, mandar emails y hacer servicios de operacion o funciones. Usar APIs siempre que sea posible, solo usar comandos de shell cuando sea absolutamente necesario. Esto reduce el numero de ataques y vectores para la aplicación y simplificar siempre el codigo base.

### INPUTS CORRECTAMENTE

Las vulnerabilidades por injección ocurren porque no hay una sanitación correcta. Si tu usas comandos de termina, estate seguro que los inputs que vayas a meter no sean potenciales caracteres maliciosos.

![image](https://github.com/pons-rgb/vuln/assets/174595469/1e7b28d0-4d52-418e-86eb-386338069d12)

Siempre es mejor, restringir los input y las expresiones regulares para mayor seguridad.

### RESTRINGUIR LOS COMANDOS PERMITIDOS

Intentaremos costruir todos o casi todos los comandos de terminal usando strings literales, cuando el input sea necesario para el usuario, añadiremos valores permitidos o enumeraremos todas las condiciones.

## CODE REVIEWS

Siempre checkear todas las vulnerabilidad que el codigo pueda tener en el proceso. Las vulnerabilidades aveces estan mucho tiempo y no nos damos cuenta.

## INICIAR CON PERMISOS RESTRINGUIDOS

Es una buena practica que el servidor processe solo con los permisos que la funcion requiera, limitando el impaacto de los comandos y la injección.

Debemos estar seguros que los procesos web solo tienen acceso a los directorios que necesitan, y igual con lo que se pueda ejecutar, Considera iniciar el proceso como una jaula como si fuera "UNIX". Esto hace que limite mucho al atacante malicio o escalar privilegios.


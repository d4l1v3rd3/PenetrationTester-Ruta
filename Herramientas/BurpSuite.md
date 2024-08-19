<p align="left"><img height=100px width=100px src="https://github.com/user-attachments/assets/28eba669-a8dd-418a-bc8d-cc7c8e147edc"></p>

<h1 align="center">BURP SUITE</h1>

<p align="center"><img  width=300px heigh=300px src="https://github.com/user-attachments/assets/c97fa100-4a80-4290-a3a8-83f04cb6bb7a"></p>

# ÍNDICE

- [INTRODUCCIÓN](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/edit/main/Herramientas/BurpSuite.md#qué-es-burp-suite)
- [CARACTERÍSTICAS BURP SUITE](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/BurpSuite.md#características-burp-suite)
- [REPEATER](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/BurpSuite.md#burp-suite-repeater)
- [INSPECTOR](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/BurpSuite.md#inspector)
- [INTRUDER](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/BurpSuite.md#intruder)
- [TIPOS DE ATAQUE](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/BurpSuite.md#introducción-a-tipos-de-ataque)
- [DECODER](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/BurpSuite.md#decoder-descripción)
- [COMPARER](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/BurpSuite.md#comparer--descripción)
- [SEQUENCER](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/BurpSuite.md#sequencer--descripción)
- [ORGANIZER](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/BurpSuite.md#organizer--overview)
- [EXTENSIONES](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/BurpSuite.md#burp-suite-extensiones)
- [BAPP STORE](https://github.com/d4l1v3rd3/PenetrationTester-Ruta/blob/main/Herramientas/BurpSuite.md#bapp-store)

# ¿QUÉ ES BURP SUITE?

Burp SUire es un framework basado en Java diseñado para servir a comprender soluciones de condución aplicaciones web sobre los penetration tester.

Es un estandár en la industria para todo el mundo en seguridad web, mobil, incluyendo en aplicaciones.

Simplemente, Burp Suite captura y tiene la posibilidad de manipular el trafico de HTTP/HTTPS entre el navegador y el servidor. Esta capacidad fundamental. Pudiendo coger consultas, y flexibilidad de hacer lo que queramos con ello.

La capacidad de intercetpar, ver y modifical la consulta antes de que le llegue al servidor es maravillosa.

![image](https://github.com/user-attachments/assets/0b166ebd-8150-4d47-92e6-53ed8a53a193)

Tenemos diferentes ediciones de Burp Suire, siendo la que utilizaremos "Burp Suite Community Edition"

# CARACTERÍSTICAS BURP SUITE

Burp Suite Comminty ofrece muchas caracteristicas (límitadas con la Professional).

- Proxy : Entabla una interceptación y modificación de las consultas mientras interactuamos con la web.
- Repeater : Captura, modifica y la consulta multiples veces. Esta funcion sirve particularmente para crear payloads, ver errores SQL o testear otros vectores de vulnerabilidad.
- Intruder : Es comunmente usado como fuerza bruta ataques o fuzzind endpoints.
- Decoder : Ofrece un servicio de transformación de datos. Si se captura una consulta se puede codificar o descodificar para enviarlo.
- Comparer : Ofrece la comparacion entre dos datos a nivel byte.
- Sequencer : Crea random rokens, ofrece sesiones de cookie random. Generación de algoritmos, etc.

![image](https://github.com/user-attachments/assets/7ec2e559-b1ee-4fcd-bfd7-e9686b4c3771)

# BURP SUITE REPEATER

Antes de usar Burp Suite Repeater, deberemos familiarizarnos con este modulo. Su proposito y funcionalidad.

En esencia, el repeater te habilita para modificar y volver a mandar la consulta interceptada al target que queramos. Gracias al proxy de Burp podemos manipular dicha consulta.

La capacidad de editar y volver a enviar multiples veces hace que repeater sea muy bueno. Tenemos una GUI para poder craftear estos payloads y ver la respuesta.

![image](https://github.com/user-attachments/assets/272b30ba-55d2-44f0-a388-3c558f7a30a7)

1. Lista de consulta: Localizada en la parte izquierda del tabulador, nos enseña la lista de consultas del repeater. Multiples consultas pueden ser administradas simultaneamentem y crear nuevas consultas.
2. Control de consulta: Posicionada directamente bajo la lista de la consulta, esto hace que podamos controlar lo que mandamos, cancelar, y navegar en el historial.
3. Consulta y Vista de respuesta: Ocupa la mayor parte de la interfaz, esta sección enseña la consulta y la respuesta. Podemos editar la consulta y ver lo posterior, mientras correspta la respuesta con lo que vemos.
4. Opciones de Diposición: Localizada en la parte derecha, esta opcion activa la customizacion de la consulta y la respuesta. La configuración por defecto es (horizontal), pero podemos cambiar vertical o separados.
5. Inspector: Posicionado en la derecha, el inspector analiza y modifica la consulta para que sea mas intuitiva para el editor.
6. Target: Situada debajo del inspector, el el archivo taget donde se especifica la IP o el dominio de la consulta que queremso enviar. Cuando la consulta se envia por el repeater otros componentes del Burp funcionan.

## USO BÁSICO

Ahora que conocemos todo de la interfaz vamos a ver como es de efectivo.

Mientras crafteamos una opción deberemos capturar una consulta para enviarla a repeater.

![image](https://github.com/user-attachments/assets/ea3bdf28-8ba8-4694-bd4f-c22a1fcc876c)

Cuando pulsamos en send, vemos mas información o lo que veriamos en el navegador.

![image](https://github.com/user-attachments/assets/9bc7561b-c779-4109-a974-ff64bc3131b9)

Si queremos modificar un aspecto de la consulta, simplemente modificamos y pulsamos en Send de nuevo. 

Vamos a actualizar la forma de ver. Alterando la "Connection" para "open" 

![image](https://github.com/user-attachments/assets/61a7f1e8-e112-46ce-b223-b7ed615503b9)

## ANALISIS DE BARRA DE HERRAMIENTSA

El repeater tiene varias pormas de consulta y respuesta dependiendo de las opciones, siendo ragging, hexadecimal o render, etc.

Para explorar estas opciones, vamos a referirnos a la respuesta y los veremos

![image](https://github.com/user-attachments/assets/47592229-8ef1-4fe0-b962-5b555b7cafb7)

1. Pretty: Es la opción por defecto, coge las respuestas y aplica el formato leible.
2. Raw: Esta opción coge la consulta sin modificar y la envia al servidor sin ninguna adicion.
3. Hex: Podemos examinar la consulta o respusta a nivel byte, útil cuando vemos archivos binarios.
4. Render: La opcion de render haceq ue visualicemos la página como el navegador. Mientras utilizamos el repeater, podemos ver el codigo fuente y sus valores. En muchos escenarios Pretty es suficientesin embargo tiene otras opciones mejores.

## INSPECTOR

Inspector es una caracteristica del repeater y obtiene una visualizacion organizada de las consultar y respuestas, podemos experimentar como cambiar todo dependiendo de como cambiemos la consulta.

Se uutilizacon el proxy.

![image](https://github.com/user-attachments/assets/5ab08e7f-268b-4202-b4b7-fdfdeab76e19)

Otros componentes, como las secciones que normalmente modificamos, editamos, podemos borrar items, podemos alterar elementos, su localización, metodo, protocolo, etc.

![image](https://github.com/user-attachments/assets/346a9b8b-b3f8-4976-8c94-95afbdcc5878)

Otras secciones:

1. Request Quet Parameters: Se refiere a los datos que envia al servidor via la URL. Por ejemplo, en una consulta GET como "https://admin,yeye.com/?redirect=false", el parametro "redirect" esta en valor "falso"
2. Request Body Parameters: Similar a las consultas, pero especificamos la consulta POST. Otro dato enviado aparte de este metodo se enseña.
3. Request Cookie: Esta sección contiene lista modificables de las cookies para enviarlas.
4. Request Headers: Esto activa que podamos ver, acceder y modificar cualquier cabecera que envie uan consulta.
5. Response Headers: Esta seccion esñea las cabeceras que te devuelve el servidor. No se pueden modificar ni tienes control de ellas.

### PRÁCTICA

Vamos a coger la consulta de la ip "http://10.10.112.203/"

Mandaremos la consulta al "Repeater" veremos el codigo de la respuesta, veremos diferentes formas de verlo como "hex"

Usando el Inspector (o manualmente), añadiremos la cabecera "FlagAuthorised" y enviaremos el valor "True" 

![image](https://github.com/user-attachments/assets/81966309-db97-41fe-a30f-0653dc4ec089)

# INTRUDER

Veremos como hacer consultas automatizadas, manipularlas y activarlas gracias a un fuzzing de fuerza bruta. 

Intruder es una herramientas de fuzzing que automatiza las consultas y modifica y repite tareas hasta validar los inputs. Caputa las consultas, introduce multiples consultas con valores alterados basados en configuraciones definidas. 

Para el servidor, hace como logeo de fuerza bruta sustituyendo username y contraseña en valores de una wordlist como un ataque fuzzing, pudiendo hacerlo a los subdirectorios, endpoints o virtual hosts.

Sin embargo, es importante saber que en "Burp Community Edition" esta limticada, en comparacion con "Burp Professional" (velocidad).

![image](https://github.com/user-attachments/assets/34523894-34c6-486a-bc4f-a3dc67726597)

Lo primero que vemos es una simple interfaz en la que podemos seleccionar al target. Este archivo es popular de la consulta que cogeremos del proxy

```
Ctrl + I
```
Para entrar

- Positions: Esta tabla selecciona el tipo de ataque y configuracion que qeuremos para insetar nuestros payloads y el template de la consulta.
- Payloads: Aqui es donde seelccionamos los valores que queremos insetar en posiciones especificas. Tenemos varias opciones de payloads, como items de una wordlist. Insertar dentro de un tema depende del tipo de ataque que elijamos. Nos deja modificar en base al payload.
- Resource Pool: Esta tabla es bastante buena. haciendo que podamos ver la localizacion el recurso atumotazando tareas.
- Settings: Configuracion.

## POSITIONS

Cuandos usamos Burp y utilizamos intruder, el primer paso es examinar las posiociones que queremos en la consulta y donde queremos insertar nuestros payloads. Estas posiciones informan al intruder las localizaciones de estos payloasd.

![image](https://github.com/user-attachments/assets/e6306e17-dffb-4293-9b8c-06cde1120f58)

Burp Suite automaticamente identifica las posiciones mas propables para insetar selecionandolo con (§)

```
Add § Clear § Auto §
```

- Add §: Define una nueva posicion manualmente.
- Clear §: Remove una posicion definida.
- Auto §: Automaticamente identifica las posiciones basadas en la consulta.

![image](https://github.com/user-attachments/assets/4f633d5f-6dc5-43e7-bf7b-24e4a58acf68)


## PAYLOADS

La tabla de Payload dentro del Burp, podemos crearm assignar y configurar los payloads que utilizaremos para atacar. Se divide en cuatros secciones.

![image](https://github.com/user-attachments/assets/e1f61b04-579f-410b-8c03-82ce10f97f97)

1. Payloads Sets:
 - Esta seccion haceq ue puedas elegir la posicione que queremos configurar el payload y seleccionar el tipo que queremos usar.
 - Cuando atacamos con un payload solo (Snipper or Battering Ram), "payload set" solo tenemos una opciones, en un numero de posiciones definidas.
 - Si usamos otros tipos de ataque requiere multiples payloads (Pitchfork o Cluster Bomb), hay un item en esta posicion
 - Note: Cuando miramos numeros en "payload Set" tenemos multiples posiciones, seguimos de arriba a bajo o de derecha a izquierda. Por ejemplo
```
username=§pentester§&password=§Expl01ted§
```

2. Payload settings:
 - Esta seccón especifica opciones para seleccionar tipos de payloads para el payload principal.
 - Por ejemplo, usamos la "lista simple" como tipo de payload, manualmente añadimos o quitamos payloads usando "add" "paste" "load" "remove" "clear"

![image](https://github.com/user-attachments/assets/53c50afc-6d40-4291-b763-6a10bd28ae8d)

3. Payload Processing:
 - En esta seccion, nosotros definimos las reglas que va aseguir el apyload.
 - Por ejemplo, mayuscula cada letra, skipea reglas o aplica transformaciones.

4. Payload Encoding:
 - Esta seccion haceq ue podamos customizar el codificado de los Payloads.
 - En defecto codifica la URL para una transmision segura

## INTRODUCCIÓN A TIPOS DE ATAQUE

La tabla "positions" de Burp selecciona el tipo de ataque. Tenemos 4 tipos de ataque.

- Sniper: Este tipo de ataque es el mas común. Recorre ciclos continuamente, insertando este payload en una posición definida de la consulta. Sniper hace un testero preciso y focuseado al target.
- Battering Ram: Manda todos los payloads simultaneamete.
- Pitchfork: Activa simultaneamente multiples posiciones con diferentes payloads. Esto hace que podamos modificar diferentes payloads, asociados a posciones especificas
- Cluster Bomb: Combina "Sniper" y "Pitchfork" simultaneamente. Es bueno cuando queremos multiples posiciones con diferentes payloads.

## SNIPER

Este tipo de ataque es el predeterminado, particularmente fectiro para "single-position" como una fuerza bruta de contraseña.

![image](https://github.com/user-attachments/assets/302822f6-5f6b-41d6-885a-8d11838da6ef)

En este ejemplo, tenemos dos posiciones definidas "username" "password" 

Asumiento que tenemos una wordlist 

![image](https://github.com/user-attachments/assets/4b7e338a-7b65-4e2c-9689-a52b6bdfa55a)

Vemos que Intruder empieza por la primera posicion "username" y luego "password" 

## BATTERING RAM

Utiliza el mismo payload en todas las posiciones simultaneamente.

![image](https://github.com/user-attachments/assets/32238d82-3f92-457d-9c8f-8fb21b49063c)

![image](https://github.com/user-attachments/assets/33a39d67-5224-4fa6-b577-1a0fbe74e329)

Como vemos en la tabla, coge la wordlist "burp" "suite" "intruder" y lo prueba en cada posicion, simultaneamente en todos. Es bueno en caso de secuencias.

## PITCHFORK

Este tipo de ataque es similar al Sniper. Mientras que Sniper se usa en todas las posiciones. Pitchfork utiliza en un maximo de 20 simultaneamente

1 - La primera wordlist "joel" "harriet" "alex"
2 - la segunda: "J03l" "Emma1815" "Sk1ll"

![image](https://github.com/user-attachments/assets/b2f89834-4ad1-41f0-ac25-ee68002646f9)

Tenemos que usar dos listas

Como vemos en la tabla, primero coge el primer item, en la primera posicion y repite el proceso cogiendo en del a segunda y asi etc.

## CLUSTER BOMB

Este tipo de ataque coge multiples payloads, uno por posicion a maximo de 20, lo teste simultanemante.

- Usernames : "joel" "harriet" "alex"
- Passowrds: "J03l" "Emma1815" "Sk1ll"

En este ejemplo, asumimos que no sabemos la contraseña que tiene el usuario,. Tenemos 3 usuarios y 3 contraseñas. 

![image](https://github.com/user-attachments/assets/785d079f-a939-4356-be15-14122484cc2f)

Como vemos en la tabla, cluster bomb coge cada combinación, cada posibilidad.

Puede generar tráfico significante en cada testeo de combinación. El numero de consultas se calculan por el numero de lineas. Es importante ser cuidadoso con este tipo de ataque.

### EJEMPLO PRÁCTICO

Imaginamos que tenemos acceso a un portal de support en "http://10.10.143.61/support/login"

![image](https://github.com/user-attachments/assets/f1542b1f-c1ee-4ac0-ad48-fc4a2fb56673)

Tenemos multiples opciones de explotar, incluyendo por fuerza bruta.

1. Descargamos o preparamos una wordlists:
 - email.txt
 - usernames.txt
 - passowrds.txt
 - combined.txt
2. Navegamos a la dirección "http://10.10.143.61/support/login"
3. Mandamos la captura con "Ctrl + I"
4. En la tabla "Positions", tenemos la opcion de seleccionar "username" "pasword"
5. Seleccionamos "Pitchfork"

![image](https://github.com/user-attachments/assets/2725b44c-3ed1-427d-8f8c-3c98c82644ff)

6. Nos vamos a "payloads" y seleccionamos los set validos

![image](https://github.com/user-attachments/assets/43c50ff9-daed-4636-a39d-b9c8eb8ea81a)

7. En este primer payload (username) en paload options nos vamos a "load" y seleccioanmos la word list repetimos el mismo proceso para passowrd

![image](https://github.com/user-attachments/assets/4cc82df7-5a3f-4ec6-ad9c-cc1192d4c838)

# OTROS MODULOS DE BURP SUITE

Veremos un poco del Decoder,Comparer, Sequencer y las herramientaso organizadas.

# DECODER: DESCRIPCIÓN

Este módulo da al usuario la capacidad de manipular los datos del usuario. Como un nombre, pudiendo codificar los datos que se interceptan durante el atacante dando la funcion de codificar nuestros propios datos, cra hashes de datos.

![image](https://github.com/user-attachments/assets/07b33c10-1a1f-4797-84ee-b57c087a9b03)

1. La caja da un espacio de trabajo para poner datos codificadonlos o descodificandolos. Consiste con otros modulos pudiendo mandarlo a otros modulos
2. En la parte baja de la lista tenemos opciones como hexadecimal texto etc.
3. Debajo de la lista tenemos menus que reperesnan, encode,decode, hash
4. El Smart Decode, localiza y automaticamente codifica el input

![image](https://github.com/user-attachments/assets/72278748-d0b6-4c46-8e88-ceeb21a1a0fc)

![image](https://github.com/user-attachments/assets/e109f803-5f47-47f9-ae06-d5c94266e47d)

## DECODER : ENCODING/DECODING

Ahora vamos a examinar los detalles

![image](https://github.com/user-attachments/assets/97f05c67-ea68-42f3-b035-05353a239b2e)

- Plain: El texto antes de ninguna transformación.
- URL: Codifical la URL utilizando un transferencia segura de datos de la web. Sustituye caracteres por ASCCI en formato hexadecimal, procede a (%) este metodo sirve para testear aplicaciones web.

![image](https://github.com/user-attachments/assets/155e229c-80d8-4540-ad16-c6ad520d24d6)

- HTML: Remplaza los caracteres especiales con un (&) seguido por un numero hexadecimal.

![image](https://github.com/user-attachments/assets/8bb1fbc2-4f89-445f-9727-3aa8d4710e95)

- BASE&$: Comunmente utilizado, convierte los datos en un formato ASCII compatible.
- ASCII HEX: "ASCII" - "4153434949"
- HEX, OCTAL Y BINARIO : Codifica en estas formas
- GZIP : Descomprime los datos, reduciendo las paginas el archivo etc.
  
![image](https://github.com/user-attachments/assets/13e54b70-6347-4664-b6a9-0773aa68a5df)

![image](https://github.com/user-attachments/assets/f5ff6a80-3ee5-44ef-8283-768bcc89d3ce)

### SMART DECODE

![image](https://github.com/user-attachments/assets/2a2b9278-9a99-4f8f-901f-a4bac8116c6f)

## DECODER : HASHING

Hash es un añadido a todo esto, solo tiene la habilidad de poder generar hashsums de nuestros datos

El hasheo es un prceso que transformación de datos en una unica firma. Esta funcion clasifica el algoritmo en hash, siendo irreversible. 

Usando el algoritmo MD5 "MD5sum" del hash "4ae1a02de5bd02a5515f583f4fca5e8c" usaremos ese mismo algoritmo para hacer "13b436b09172400c9eb2f69fbd20adad"

### HASHING IN DECODER

![image](https://github.com/user-attachments/assets/2793b8e0-f8af-42e7-814d-2b1b43d62d57)

![image](https://github.com/user-attachments/assets/98f9625f-6b01-4e91-8d79-7bf6ee46674a)

![image](https://github.com/user-attachments/assets/d86e8317-4897-4957-a821-70cd1958840d)

![image](https://github.com/user-attachments/assets/0750df1d-0a6d-403b-ad26-d99d1bab8a8a)

# COMPARER : DESCRIPCIÓN

![image](https://github.com/user-attachments/assets/3aac2554-2fe6-4bd2-8be4-50fc8c5d143f)

1. En la izquierda, podemos ver los objetos del comprarer. Podemos añadir datos en el comprarer, en tablas y columnas. Podemos seleccionar dos bases de datos para comparar.
2. En la parte derecha, tenemos la opcion de pegar los datos o cargarlos o eliminarlos o borrarlos
3. Por ultimo, abajo a la derecha tenemos la opcion de comprar nuestras propias bases de datos por letras o bytes. No importa que tipo de botones seleccionamos podemos cambiarlo luego.

![image](https://github.com/user-attachments/assets/6bcf2ae2-f910-4ffa-8444-e2a3292f6bb7)


# SEQUENCER : DESCRIPCIÓN

Valora la entropia, lo randos los "tokens" se usan para identificar o generar una secuenda. Estos tokens normalmente son cookies de sesion on "Cross-Site Rquest Forgery (CSRF)". Estos tokens son generados secuencialmente, en teoria se podría predecir dicho token.

![image](https://github.com/user-attachments/assets/8317be9e-901d-4630-8654-028f42aec047)

- Live Capture: El metodo más común captura en el momento y genera un token que ha analizado.
- Manual Load: Añadimos un token pregenrado

## SEQUENCER : LIVE CAPTURE

Capturamos la consulta "http://10.10.217.172/admin/login/" lo enviamos al secuencer.

![image](https://github.com/user-attachments/assets/3a7e8360-307a-4b00-b32d-f47be415a92f)

![image](https://github.com/user-attachments/assets/5f32536e-b8ec-4e3f-8f16-9d195f3e7b08)


## SEQUENCER : ANALYSIS

Ahora que tenemos el reporte podemos ver y leer lo que pone

![image](https://github.com/user-attachments/assets/2d705097-c97c-4276-86f8-29daf8ad65cf)

- Overall result: No dice la seguridad que tiene el mecanismo generado. En este caso indica la seguridad que pone
- Effective entropy: La effectividad que tiene la entropia de 117 bits es alta, indicando que el token es suficientemente rando, y seguro para ataques de fuerza bruta.
- Realibity: 1%
- Sample: Provee los detales de ejemplso de tokens mientras da el proceso de testeo, incluye el numero e tokens  y sus caracteristicas.

# ORGANIZER : OVERVIEW

EL modulo de "Organizer" diseñado para ayudar y guardar las consultas HTTP que luego queramos ver.

- Podemos guardar consultar que queramos investigar luego o identificar
- Podemos mandar consultas a organizer y luego al repetear o etc "Ctrl + O"

![image](https://github.com/user-attachments/assets/cedab0c6-33de-4f14-966a-668edd8b0154)

Las consultas se guardan en la tabla, con columnas y numeros con index.

![image](https://github.com/user-attachments/assets/cb5febbe-bdba-467d-97fd-eb1337552841)

![image](https://github.com/user-attachments/assets/be949a09-4540-4c80-9e5f-c01019d31858)


# BURP SUITE: EXTENSIONES

## INTERFAZ DE EXTENSIONES

la interfaz que nos da Burp nos deja añadir con esta herramienta.

![image](https://github.com/user-attachments/assets/c34b5723-4aaa-4fba-8bff-bcf0c716c327)

1. Extension List: La caja enseña la lista de extensiones que estan instaladas actualmente
2. Managing Extensions: En la parte izquierda veremos la interfaz
 - ADD: podemos usarlo para añdir nuevas extensiones de los archivos que tengamos de nuestro disco o en "BApp store"
 - Remove: Desistala las extensiones
 - Up/Down: Controla el orden de como se instalan las extensiones. El orden determina la secuencia en la que se ejecutaran las extensiones al procesar el tráfico. Van de arriba a abajo.
3. Detalles, Output y Errores:
 - Dettales: Nos da la información sobre las extensiones
 - Output: Lo que produce trás su ejecución.
 - Errors: Durante la ejecución.

## BApp STORE

(Burp App Store) facil de descubrir en la misma lugar de las herramientas de extensión. Escrito en varios lenguajes como Java y Python. Las extensiones de java se introducen automaticamente las de python necesitan un interprete de Java.

![image](https://github.com/user-attachments/assets/9d95789f-52d8-46b7-9185-575d9006650e)





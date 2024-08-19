# BURP SUITE

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

![Uploading image.png…]()

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




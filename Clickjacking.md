# CLICKJACKING

Clickjacking hace un truco a los usuarios de la web haciendo creer una acción que ellos no hacen, tipicalmente renderizado en una página invisible los elementos de arriba de la acción el usuario piensa que no pasa nada.

Clickjacking no afecta al sitio directamente, si no afecta potencialmente a los usuarios.

## PELIGROS

¿Por quñe un determinado hacker haria un ataque así?

Un ejemplo sería hacer un truo a un objeto de Facebook.

- Robo de credenciales
- Trucos a usuarios para activar web-cam o microfono.
- Poner gusanos en redes sociales
- Promover scams online.
- Promover malware.

## PROTECCIÓN

Clickjacking hace un ataque de agrupación en la pagina para hacer creer a los usuarios algo, entonces renderiza elemento invisibles dentro del codigo. Para estar seguros que no nos vamos a comer un ataque de clickjacking, tu necesitas estar seguro de que no puedes agrupar un iframe en un sitio malicioso. Esto hara instrucciones al navegador para que los headers "HTTP"

## SEGURIDAD DE POLITICA

El contenido de seguridad de politica es un apartado de "HTML5" estandarizado, provee un rango de protección por las opciones del frame de la cabecera. Esto esta diseñado para que los autores de las páginas puedan enumerar individualmente los dominios y sus recursos (como scripts, estilos y fuentes) puedan ser cargadas, y otros dominios tengan la funcion de poder hacerlo.

El control de tu pagina tiene que ser incorporado, usando la directiva "frame-ancestors"

![image](https://github.com/pons-rgb/vuln/assets/174595469/8a9e5cb6-3ab8-4a7a-9e77-6a017162935f)

### X-FRAME-OPTIONS

El header "X-Frame-Options" es el estandar más viejo que podemos indicar apra que un navegador renderice una página "frame" "iframe" o "object" (tag). Esto esta diseñado específicamente para ayudar a una protección de clickjacking, pero es mas obsoleto que las politicas de privacidad.

Hay 3 valores permitidos en el header: 
![image](https://github.com/pons-rgb/vuln/assets/174595469/827b760a-ead0-4080-82a6-b186d67a3e8f)

### FRAME KILLING

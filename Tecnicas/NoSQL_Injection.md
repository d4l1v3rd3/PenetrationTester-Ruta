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



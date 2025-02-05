# Introducción

Con los avances en ciberseguridad, muchos desarrroladores tienen que adoptar relaciones de objetos (ORM) "object-relational mapping" para mitigar injecciones SQL. MIentras ORM intenta simplificar las interacciones de bases de datos y mejorar la seguridad, el truco de la injección no acaba. Las injecciones ORM ocurren cuando un atacante explota un framework relacionado, o ejecuta consultas arbitraris.

# Objetivos de aprendizaje

- Entender que es ORM
- Identificar injecciones
- Malas implementaciones
- Implementaciones vulnerables

# Entender ORM

ORM es una técnica de programacion que facilita la conversión de datos entre sistemas incompatibles usando lenguajes orientados a objetos. Los desarrolladores interactuan con la base de datos usanto un lenguaje nativo, manipulando datos y reduciendo la necesidad de SQL. 

![image](https://github.com/user-attachments/assets/790f7455-9743-4a68-8517-f28c131568d8)

## Proposito

ORM es como un puente entre el modelo de programación y la base relacional. El primer proposito de ORM es abstraer las capas de una base de datos.

- Reduciendo el codigo repetitivo: reduciendo la necesidad de codigo repetitivo generando automaticamente consultas SQL
- Incrementando la productividad: Los desarrolladores se centran en un esquema lógico sin preocuparse sobre las interacciones de base de datos
- Asegurandose consistencia: Frameworks consistentes que vayan de la mano con las operaciones de la BD, recuendiendo errores.
- Mejorando el mantenimiento: Cambios más fáciles, reflejar objetos sin la necesidad de modificación de código masivo.

## Frameworks Usados normalmente

- Doctrine (PHP) : Popular en frameworks de Symfony y usado independientement. Da herramientas interactivas de la BD, incluyendo constructor de consultas, esquemas, y consultas orientadas a objetos. Abilita un mapa complejo de estructuras y el mejor para desarrolladores PHP
- Hibernate (JAVA) : Simplifica las clases de Java para las tablas y mejor devolución de datos y manipulacion de las capacidades con su propio lenguaje (HQL).
- SQLAlchemy (python): Es como una herramienta de SQL y un sistema ORM para los desarrolladores que quieren los dos beneficios a la vez.
- Entity Framework (C#)

# Como funciona ORM

## Mapeo entre objetos en código y tablas

ORM es una técnica para simplificar la interaccion con la aplicación. En php, este proceso desenvuelve clases y representa las tablas y rleaciones. Cada clase tiene sus propiedades correspondientes en la tabla, y cada clase representa una columna

Usando por ejemplo Larave's Eloquent, definimos las clases a sí:

```
namespace App\Models;
use Illuminate\Database\Eloquent\Model;
class User extends Model
{
    protected $table = 'users';
    protected $fillable = [
        'name', 'email', 'password',
    ];
    // Other Eloquent model configurations can go here...
}
```
En este ejemplo podemos ver qeu el "user" class mapea los usuarios de la tabla de la BD, con las propiedades correspondientes a las columnas. Elooquent translada entre los objetos la representación y el entendimiento de una consulta SQL.

## Operaciones comunes

- Create: Crea nuevos criterios ela BD como un nuevo objeto, configurando propiedades y lo guarda en la BD

```
use App\Models\User;

// Create a new user
$user = new User();
$user->name = 'Admin';
$user->email = 'admin@example.com';
$user->password = bcrypt('password'); 
$user->save();
```

Este código crea un nuevo usuario y lo guarda. El metodo save prepara una entidad de insercción que ejecuta una consulta SQL INSERT para añadir un nuevo record. La funcion "bcrypt()" se usa para segurar la contraseña con un hash

- Read: Lee simplemente jaja

```
use App\Models\User;

// Find a user by ID
$user = User::find(1);

// Find all users
$allUsers = User::all();

// Find users by specific criteria
$admins = User::where('email', 'admin@example.com')->get();
```

Este código demuestra las diferencias de una bd. La funcion find(1) devuelve al usuario con ID 1 haciendo una consulta SQL. La función all() devuelve todos los usuarios como si hiciera "SELECT * FROM users". La clausula "where('email', 'admin@example.com')-get() devuelve el email especificado ejecutando las consultas "SELECT * FROM users WHERE email = 'admin@example.com"

## Vamos a comparar 

- SQL Injection: Los atacantes pueden manipular la consulta. Normalmente se hace el OR '1'='1 haciendo que siempre devuelva verdad

```
SELECT * FROM users WHERE username = 'admin' OR '1'='1';
```

- ORM Injection: Lo mismo pero vamos a manipular con ORM

```
$userRepository->findBy(['username' => "admin' OR '1'='1"]);
```

# Crear nuestro propio entorno

Vamos a usar Laravel para este proyecto, vamos a explicar como configurar el ORM Eloquent. Ya incluido normalmente con Laravel.

Necesitaremos instalar Laravel usando Compose. Abriremos una terminal y pondremos el comando

```
composer create-project --prefer-dist laravel/laravel nodo-project
```

donde nodo-project es el nombre de tu proyecto

## Configurar la BD

Ahora deberemos configura la bd en el archivo .env donde se guardan las variables

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database_name
DB_USERNAME=your_database_user
DB_PASSWORD=your_database_password
```

## Crear migraciones

Para crear una migración, debemos usar el comando Artisan con Larvael

```
php artisan make:migration create_users_table --create=users
```

Para migrar los usuarios de la tabla

Este comando genera un archivo que migra el direcotrio que continee la estructura de usuarios

```
<?php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;
class CreateUsersTable extends Migration
{
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('email')->unique();
            $table->string('password');
            $table->timestamps();
        });
    }
    public function down()
    {
        Schema::dropIfExists('users');
    }
}
```

Vemos que el metodo up() define la estructura de usuarios. Con las columnas ID, nombre, email, password y timestamps. Luego down() para eliminar los usuarios si la migración vuelve.

Después de definir la migración ejecutaremos el comando

```
php artisan migrate
```

Aplicaremos la migracion y crearemos la tabla usuarios. El comando que ejecuta up() migra y crea la tabla con las columnas especificas

# Identificar Injecciones ORM

![image](https://github.com/user-attachments/assets/3a1c233d-ad49-4e02-b6a8-db0642224279)

## Exploremos un target

Técnicas para identificar el framework

- Verificar las cookies

![image](https://github.com/user-attachments/assets/bc3fd51e-6429-4555-9007-03c6e02bd978)

- Ver el código fuente

![image](https://github.com/user-attachments/assets/7718fe5c-fa2e-4b53-b07c-e9d6174d7a2e)

- Analizar los headets los HTTP
- La estructura de URL
- Errores de páginas como logins

Ahora que conocemos por ejemplo inspeccionando las cookies, vamos a probar inputs

![image](https://github.com/user-attachments/assets/ab0575f6-e7b4-4b63-ba68-0aa4537cb6c2)







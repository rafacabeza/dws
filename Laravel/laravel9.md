# 8.**

- Laravel se ha convertido en el framework php más utilizado para desarrollo web.
- En septiembre/2019 se publicón la versión 6.0
- Desde entonces usa versionado semántico.
  - Febrero y agosto: versiones mayores: 6 (LTS), 7, ... (febrero 2022: v9)
  - Semanalmente pueden aparecer versiones menores (segundo dígito) y de parcheo (tercero). 
  - Las versiones mayores pueden incluír cambios de ruptura, el resto no.


- Usa arquitectura MVC (Modelo Vista Controlador) y mucho más:
- Sistema de enrutamiento potente
- Usa Blade como gestor de plantillas
- Su propio ORM: Eloquent (¿Adios SQL en nuestros proyectos?, para el 99% de las situaciones, sí)
- Y muchos otros componentes que lo hacen muy interesante: Query Builder, Database Migrations, Seeders, Passport, Dusk, Socialite, ... y muchos otros componentes brindados por terceras partes.


- Enlaces:
  - [Wikipedia](https://en.wikipedia.org/wiki/Laravel) mejor en inglés.
  - [Sitio oficial](https://laravel.com/)
  - [Documentacion](https://laravel.com/docs)
  - [Laracast](https://laracasts.com/skills/laravel): videotutoriales oficiales
  - [Styde.net](https://styde.net/): numerosos videotutoriales, cursos y publicaciones
  - [Código fuente](https://github.com/laravel/laravel). Se puede clonar para iniciar proyecto.



## Instalación

- https://laravel.com/docs/9.x/installation
- Requisitos: https://laravel.com/docs/9.x/deployment 
  - php8.0 y algunos complementos
  - Composer (v2) como gestor de dependencias
- Podemos desarrollar sin servidor o con él (Apache o Nginx)
- Podemos dockerizar el entorno de desarrollo. Usando docker podríamos obviar estas dependencias en el anfitrión pero las vamos a instalar. Facilitará nuestra tarea.


## Instalación via composer (directorio laravelpruebas):

```
#proyecto de ejemplo: laravelpruebas
#última versión
composer create-project --prefer-dist laravel/laravel laravelpruebas
#versión 8
composer create-project --prefer-dist laravel/laravel:^8.0 laravelpruebas
```

- Esto implica:
  - Descargar fremwork original
  - Ejecutra composer install
  - Crear fichero de entorno (.env)
  - Generar clave de cifrado


## Instalación via git

- Partimos del código de GitHub o de otro proyecto Laravel. Vamos a usar este ejemplo para iniciar el proyecto en nuestro entornods. Aquí todas las fases son manuales:
  - Antes de nada, vamos a la rama correcta de "entornods"
    ```
    cd ~/entornods
    git fetch
    git checkout --track origin/laravel
    ```

  - Descarga

  ```
  git clone https://github.com/laravel/laravel.git nombreProyecto
  cd nombreProyecto
  ```

  - Instalación de dependencias. Ojo, si no tenemos la versión correcta de php y composer entramos dentro del contenedor.

  ```
  docker exec -it -u devuser laravel bash
  composer install
  ```


  - Crear fichero de entorno a partir del de ejemplo:

  ```
  cp .env.example .env
  ```

  - Creación de la clave de cifrado (ver .env).

  ```
  php artisan key:generate
  ```


**NOTA**

- **artisan** se usa de forma generalizada. Por ejemplo, todas las clases **deben** crearse con ese comando.
- Podemos desarrollar sin servidor ejecutando:

```
php artisan serve  # para iniciar el servicio
chrl+C             # para cerrar
```


### Laravel en nuestro entornods:

- Descargamos nuestro entornods y nos ponemos en la rama laravel
  ```
  git checkout --track origin/laravel
  ```
- Añadimos al fichero hosts:
  ```
  127.0.0.1   laravel.local
  ```
- Colocamos el directorio del proyecto dentro de *data*
- Levantamos nuestro servicio:
    ```
    cd entornods 
    docker-compose up -d
    ```


OJO!!! 
- El _entornods_ viene configurado para una aplicación ubicada en data/laravel
- El resto de instrucciones también
- Si usamos otro nombre deberemos ajustar alguno de estos comados y el contenido del fichero _docker-compose.yml_


- El uso de la consola va a ser importante en el desarrollo con Laravel
- Puede ser interesante definir aliases de comandos
- Editamos el fichero ~/.bashrc 

    ```
    #añadir lo siguiente al final: de  ~/.bashrc
    alias laraa='docker exec --user devuser laravel  php artisan '
    alias larac='docker exec --user devuser laravel  composer '
    alias larai='docker exec -it --user devuser laravel bash'
    ```
- ejecutar lo siguiente si no queremos reiniciar:

    ```
    source ~/.bashrc
    ```


- El fichero `.env`
    * El fichero .env define variables utilizadas únicamente en el entorno de desarrollo. 
    * Cada sitio de desarrollo o producción mantiene su propio `.env`
    * Este fichero debe excluirse de _git_. Compruébalo en el fichero .gitignore.
    * Debemos ajustar la parte de BBDD y el APP_URL=http://laravel.local
- El directorio de configuración es _config_. Buena parte de la configuración es condicionada al contenido del fichero `.env`


Repositorio de clase

- Vamos a usar el  repositorio: [laravel22](https://bitbucket.org/rafacabeza/laravel22)
- Recomendación: hacer un fork del mismo:
- Una vez hecho esto:
- 
  - Ve a bitbucket y realiza un fork
  - Clona dentro de `entornods/data` tu repositorio
  - Instala dependencias
  - Obten una copia del fichero `.env`
  - Genera la clave de cifrado


    ```
    git clone ....
    cd laravel
    cp .env.example .env
    composer install
    php artisan key:generate
    ```

- Es interesante poder descargar el código de clase. Para hacerlo añade el repositorio del profesor:

```
cd data/laravel
git remote add rafa git@bitbucket.org:rafacabeza/laravel20.git
```

- Cuando quieras descargar la última clase:

```
git fetch rafa
git checkout --track rafa/claseXX #la rama que corresponda
```




## Arquitectura


- Configuramos nuestro servidor para que todas las peticiones lleguen a _index.php_ (var https://laravel.com/docs/6.x/installation)
- _index.php_ sólo crea un objeto $app a partir del script _bootstrap/app.php_.
- Tras esto la petición o _request_ se pasa al kernel (nucleo) http o de consola:


- Kernel Http:
    ```php
    class Kernel extends HttpKernel
    ```
    - Carga clases _bootstrapers_ que preparan la aplicación (heredado)
    - Define una serie de _middlewares_ (filtros) que se ejecutan antes de tratar el _request_
    - Por fin, el kernel trata el _request_ y devuelve un _response_ (heredado)
- Todas las clases dentro de _Iluminate_ están dentro de vendor, son parte del framework y no debemos modificarlas en absoluto. Todo lo "heredado" está ahí.


- La respuesta
    - Tras preparar la aplicación, la petición se pasa al _router_
    - De acuerdo a las rutas definidas se genera una respuesta o se delega en un controlador.
    - A las rutas y controladores se les puede asignar middlewares.


- Los Service Providers
    - Son el elemento clave en el arrange (_bootstraping_) de la aplicación.
    - Definen los componentes disponibles en la aplicación.
    - El fichero `config/app.php` cuenta con un array donde definimos cuales otros queremos usar
        - Muchos de ellos están en el frameword (empiezan por _Illuminate_)
        - Otros están en  _app/Providers_
        - Si añadimos librerías o creamos los nuestros propios (avanzado) deberemos añadir elementos a este array.



## Rutas


- https://laravel.com/docs/8.x/routing

* En nuestro framework mvc "casero", un controlador era responsable de una tarea concreta, o conjunto de tareas relacionadas entre sí \(CRUD sobre una tabla, por ejemplo\).

* Para nosotros el _path_ o ruta de nuestra url representaba /controlador/metodo/arg1/arg2/argN. 

* Laravel tiene un sistema de enrutado mucho más flexible y potente. 


* Las rutas se definien en el directorio `routes`:
    - `web.php` define todas las rutas de la aplicación web
    - `api.php` define las rutas asociadas a un servicio web
    - `console.php` define rutas usadas desde la consola con `php artisan`
    - `channels.php` se usa en aplicaciones en tiempo real y websockets. No toca...


- Para definir una ruta usamos una clase llamada Route.
- Usamos una función que define el tipo de petición (get, post, ...)
- Definimos una ruta
- Definimos una función de callback o como veremos un controlador y un método

```php
Route::get('user', function () {
 return 'hola mundo. Estamos en user';
});
```


* Veamos algunos ejemplos ilustrativos:

```php
Route::get('user', function () {
 return 'hola mundo. Estamos en user';
});

//prueba esta ruta creando un formulario.
Route::post('user', function () {
 return 'hola mundo. Estamos en user, por el método POST';
});

//podemos usar parámetros
Route::get('user/{id}', function ($id) {
 return 'Preguntando por user' . $id;
});

//podemos usar parámetros opcionales
Route::get('book/{id?}', function ($id = "desconocido") {
 return 'Preguntando por el libro ' . $id;
});

Route::get('user/delete/{id}', function ($id) {
 return 'Baja de usuario, argumento id:' . $id;
});

//Podemos filtrar los argumentos con expresiones regulares
Route::get('user/{name}', function ($name) { 
    // name debe ser texto
}) ->where('name', '[A-Za-z]+'); 

Route::get('user/{id}', function ($id) { 
    //  id debe ser un nº. natural
}) ->where('id', '[0-9]+')
```


### Rutas y controladores

- Las rutas anónimas son útiles para pequeñas tareas y pruebas.
- Pero un trabajo serio necesita el uso de controladores (MVC)

```php
use App\Http\Controllers\UserController;

//ruta prueba gestionada por el método "index" de PruebaController
Route::get('/photos', [PhotosController::class, 'index']);
```


Ojo! Esto ha cambiado en la versión 8. Anteriormente la sintáxis era: 

```php
Route::get('foo', 'Photos\AdminController@method');
```


### Rutas resource: REST

- Un CRUD necesita definir 7 rutas para 7 acciones.
    - Lectura: ruta de listado y de detalle
    - Creación: mostrar formulario de alta y procesarlo
    - Update: mostrar formulario de edición y procesarlo
    - Borrado

```php
Route::get('photos/create', 'Photos\AdminController@create');
Route::post('photos', 'Photos\AdminController@store');
Route::get('photos', 'Photos\AdminController@index');
Route::get('photos/{id}', 'Photos\AdminController@show');
Route::get('photos/{id}/edit', 'Photos\AdminController@edit');
Route::put('photos/{id}', 'Photos\AdminController@update');
Route::delete('photos/{id}', 'Photos\AdminController@destroy');
```


- Si definimos rutas de tipo resource se mapearán estas 7 rutas.
- En la medida de lo posible estas rutas siguen el paradigma REST.
- Son las acciones necesarias para un CRUD completo y usando los distintos verbos HTTP: get, post, put y delete.
- Para ver como gestionar estas rutas y este tipo de controladores: https://laravel.com/docs/8.x/controllers#resource-controllers


- Definición de una ruta
```
Route::resource('photos', 'PhotoController');
```
- Creación de un controlador resource:
```
php artisan make:controller PhotoController --resource
```
- Lista de rutas:
<small>
<table>
<thead>
<tr>
<th>Verb</th>
<th>URI</th>
<th>Action</th>
<th>Route Name</th>
</tr>
</thead>
<tbody>
<tr>
<td>GET</td>
<td><code>/photos</code></td>
<td>index</td>
<td>photos.index</td>
</tr>
<tr>
<td>GET</td>
<td><code>/photos/create</code></td>
<td>create</td>
<td>photos.create</td>
</tr>
<tr>
<td>POST</td>
<td><code>/photos</code></td>
<td>store</td>
<td>photos.store</td>
</tr>
<tr>
<td>GET</td>
<td><code>/photos/{photo}</code></td>
<td>show</td>
<td>photos.show</td>
</tr>
<tr>
<td>GET</td>
<td><code>/photos/{photo}/edit</code></td>
<td>edit</td>
<td>photos.edit</td>
</tr>
<tr>
<td>PUT/PATCH</td>
<td><code>/photos/{photo}</code></td>
<td>update</td>
<td>photos.update</td>
</tr>
<tr>
<td>DELETE</td>
<td><code>/photos/{photo}</code></td>
<td>destroy</td>
<td>photos.destroy</td>
</tr>
</tbody>
</table>
</small>



## MVC: Controladores & vistas
- Laravel es mucho más que una arquitectura MVC
- No obstante esta es una parte fundamental del framework
- Veamos de momento como tratar controladores y vistas


### Controladores

- Su ubican en _app/Http/Controllers_ 
- Para crear un controlador usaremos:
```
php artisan make:controller ControllerName
```
    - Deberemos definir los métodos que necesitemos y asociarlos a una ruta del fichero de rutas (de momento, web.php).


- Si queremos usar rutas de tipo resource:
```
php artisan make:controller ControllerName --resource
```
    - Ya nos vendrá con los 7 métodos asociados a una ruta resource

* OJO! Los formularios HTML no pueden usar PUT, PATCH ni DELETE. Podemos usar campos ocultos para falsear los verbos. El metodo helper `method_field` lo puede hacer por nosotros:

```php
{{ method_field('PUT') }}
{{ method_field('DELETE') }}
```


- Si en un método necesitamos hacer uso del objeto $request debemos incluírlo en su cabecera con _type hinting_:

```php
    public function foo(Request $request)
    {
        //
    }
```


- Podemos acceder a los parámetros GET y POST así:

```php
    public function foo(Request $request)
    {
        $code = $request->code;
        $all = $request->all();
        dd($code);
    }
```


### Vistas: Blade

* Las vistas en php: php + html ==> código poco claro.
* Mejor motores de plantillas que facilitan la inclusión de contenido variable en nuestro html.
* Motores de plantillas son:
  * **Blade**, usado por Laravel
  * Smarty, muy habitual, no ceñido a ningún framework
  * Twig, usado por Simphony.
* Código más limpio. 

```
plantilla + compilación transparente = html + php
```


- En laravel se devuelven con la función helper _view()_
- Desde un controlador o ruta: 
    ```php
    return view('grupo.vista'); //estilo habitual
    return view('grupo/vista'); //también válido
    ```
    - Buscará una vista en el directorio _resources/views/grupo_
    - El fichero se llamará _vista.blade.php_ o _vista.php_


- Para llevar variables a la vista debemos hacer lo siguiente:
    ```php
    return view('grupo.vista', $arrayAsociativo);
    ```
- Ejemplos:
    ```php
    return view('user.index', ['name' => 'James']);
    ```
    ```php
    return view('blog.history', [
            'mes' => $mes,
            'ano' => $ano,
            'posts' => $posts
    ]);
    ```


- Existen otras variantes:
    * Con with:

    ```php
    return view('greeting')->with('name', 'Victoria');
    ```

    * Y por último usando compact.

    ```php
    return view('users', compact('mes', 'ano', 'posts'));
    //equivale a:
    return view('blog.history', [
            'mes' => $mes,
            'ano' => $ano,
            'posts' => $posts
    ]);
    ```



## Modelos y Bases de datos.

* Un modelo se crea así:

  ```php
  php artisan make:model Study
  ```

* Pero para hablar de modelos debemos entender como gestionar las BBDD.


* Para hacerlo Laravel nos brinda:

  - Migraciones
  - Seeders
  - Model Factrories


### Migraciones

* Laravel provee un sistema llamado "migraciones" que permite elaborar la estructura de la base de datos de un modo paulatino.
* Las migraciones se asemejan a un control de versiones para la base de datos. Cada migración puede desplegarse o deshacerse según se precise.
* Facilitan enormemente el trabajo en equipo y mediante SCV \(git u otro\).


* Facilitan la migración de SGBD (Mysql, PostGres, ...).
* Facilitan la implantación en nuevos equipos de desarrollo o en producción.
* Debemos crear una migración por cada tabla.
* Debemos crearla con artisan y editarla después.
* Nota, en la versión 6 podemos crearla a la vez que el modelo:

```
php artisan make:migration create_studies_table  //crear migración
php artisan make:model -m Study //crear modelo y migración en un comando
```


- Para modificar una tabla tenemos dos opciones:
    - Modificar la migración de creación. NO SIEMPRE POSIBLE
    - Crear una migración de modificación. Ejemplo: 

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class ModifyUsersTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->bigInteger('role_id')->unsigned()->default(1)->after('id');
            $table->foreign('role_id')->references('id')->on('roles');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->dropForeign('users_role_id_foreign');
            $table->dropColumn('role_id');
        });        
    }
}

```


## Creación de migraciones

* Se usa `artisan make:migration`:
  ```
  // ej. comando base.
  php artisan make:migration create_users_table
  // ej. comando para creación de tabla. Añade código de creación.
  php artisan make:migration create_users_table --create=users
  ```

* El resultado es una clase hija guardada **database/migrations** 
* El fichero se nombra usando el *timestamp* de creación para que sea ejecutado en el orden correcto.


### Métodos de una migración
* up\(\) sirve para modificar la base de datos. Típicamente crear una tabla.
* down\(\) sirve para devolver la base de datos a su estado previo. Típicamente borrar una tabla.

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateStudiesTable extends Migration
{
    /**
     * Run the migrations.
     * Ejecutar migración
     * @return void
     */
    public function up()
    {
        //code
    }

    /**
     * Reverse the migrations.
     * Deshacer
     * @return void
     */
    public function down()
    {
        //code
    }
}
```


### Código de las migraciones

* Dentro de las funciones _up_ y _down_ usamos las funciones de la clase esquema para:

* Crear tablas: 

```php
Schema::create('studies', function (Blueprint $table) {
    $table->id();
    ...
    $table->timestamps();
});
```


* Modificar tablas.: 

```php
Schema::table('studies', function (Blueprint $table) {
    $table->string('name', 50)->change();
    $table->bigInteger('role_id')->unsigned()->default(1)->after('id');
    $table->string('description', 100)->after('name');
}
```


* Borrar tablas: 
    
```php
public function down()
{
    Schema::dropIfExists('studies');
}
```

* En las funciones anónimas \(closure\) de creación y modificación podemos hacer prácticamente cualquier cosa que soporte SQL. Debemos usar los métodos de la clase Blueprint aplicados a la variable $table:

  ```php
  $table->string('email');  //añadir columna de texto VARCHAR(100). Hay métodos para muchos tipos de datos. Ver documentación.
  $table->string('name', 20);
  $table->tinyInteger('numbers');
  $table->float('amount');
  $table->double('column', 15, 8);
  $table->integer('votes');

  //podemos añadir modificadores e índices:
  $table->id();  //Clave principal, autoincremental, big integer.
  $table->char('codigo', 25)->primary(); //Otras claves principales
  $table->primary(['area', 'bloque']); // o así...
  $table->string('email')->nullable(); //añadir columna y permitir nulos.
  $table->timestamps(); //añade columnas create_at, update_at
  $table->string('email')->unique();  // columna con indice único
  $table->index(['account_id', 'created_at']); //índice multicolumna

  //claves ajenas: columna del tipo adecuado más f.k.
  $table->integer('user_id')->unsigned();
  $table->foreign('user_id')->references('id')->on('users');
  //podríamos añadir ->onDelete(cascade) ...
  ```


  * Los posibles modificadores son:
    ```
    ->first() | Place the column “first” in the table (MySQL Only)
    ->after('column') | Place the column “after” another column (MySQL Only)
    ->nullable() | Allow NULL values to be inserted into the column
    ->default($value) | Specify a “default” value for the column
    ->unsigned() | Set integer columns to UNSIGNED
    ->comment('my comment') | Add a comment to a column
    ```

* Para otras operaciones consultar la documentación oficial.


### Seeders \(o sembradores\)

  * Los _seeders_ son clases usadas para:
  * Rellenar las tablas con datos de prueba
  * Cargar las tablas con datos iniciales.
  * Creación con artisan:

  `php artisan make:seeder StudySeeder`

  * Los seeders tienen un método _run\(\)_ donde se registra el contenido de los registros a insertar. Puede usarse sintáxis de _Query Builder_ o de _Eloquent_


* Ejemplo:

```php
use Illuminate\Database\Seeder;
use App\Review;
class StudiesSeeder extends Seeder
{
    public function run()
    {
        //con eloquent
        Study::create([
        'code' => 'IFC303',
        'name' => 'Desarrollo de Aplicaciones Web',
        'abreviation' => 'DAW'
        ]);

        //lo mismo con query builder
        DB::table('studies')->insert([
        'code' => 'IFC303',
        'name' => 'Desarrollo de Aplicaciones Web',
        'abreviation' => 'DAW
        ]);
    }
}
```


* El orden en que se ejecutan los seeders se establece manualmente. Debe rellenarse el método _run\(\)_ de la clase _DatabaseSeeder_ ubicada en _database/seeds_

```php
public function run()
{
    $this->call(StudySeeder::class);
    $this->call(ModuleSeeder::class);
}
```


* Su ejecución es desde artisan:
  * Modo general
    `php artisan db:seed`
  * Junto a las migraciones:
    `php artisan migrate:refresh --seed`
  * De forma individual
    `php artisan db:seed --class=ModuleSeeder`


### Model Factories

* Un factory es una clase usada principalmente para desarrollo
* Permite llenar nuestra base de datos con registros de prueba
* Crear un _factory_:

```
php artisan make:factory PostFactory
```

* Codificar un factory:

```php
use Faker\Generator as Faker;

$factory->define(App\User::class, function (Faker $faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->unique()->safeEmail,
        'password' => '$2y$10$TKh8H1.PfQx37YgCzwiKb.KjNyWgaHb9cbcoQgdIVFlYg7B77UdFm', // secret
        'remember_token' => str_random(10),
    ];
});
```


* Usar el factory:

  * Crear un objeto con `make()` o con `create()`, el primero crea una variable, el segundo además la guarda en base de datos:

  ```php
      $user = factory(App\User::class)->make();
      $user = factory(App\User::class)->create();
  ```
  * Podemos crear más de un registro en la misma orden:
  ```php
  $users = factory(App\User::class, 3)->create();
  ```


  * Además podemos fijar o sobreescribir el valor de los campos:

```php
$user = factory(App\User::class)->create([
    'name' => 'Abigail',
]);
```



### Modelo


### Bases de datos
- Para acceder a bases de datos podemos (por orden):
 - ORM: Eloquent. Usado por Laravel y que usa una arquitectura Active Record.
 - Fluent Query Builder. Un sistema de creación de consultas independiente del gestor de bbdd utilizado.
 - SQL. Usar consultas de Raw Sql (sql en crudo o a pelo). Poco habitual


- Bases de datos compatibles:
 - MySQL
 - Postgres
 - SQLite
 - SQL Server
- https://laravel.com/docs/6.x/database


- Configuración:
 - Fichero _config/database.php_.
 - Pero basada en el _.env_.
- Podemos usar las bases de datos de forma convencional, creadas previamente. 
- Más interesante usar migraciones como veremos. 


 ### Modelo
- Las clases modelo extienden una superclase llamada Model
- Heredan métodos muy interesantes. 
- Permiten realizar multitud de operaciones sin escribir una sóla línea de código a través de Eloquent.


- Nomenclatura:
    - El nombre del modelo: tiene que ser en singular y mayúscula. P.ej. "User"
    - La la tabla asociada estará en plural y minúscula. P. ej. "users"
    - Clave primaria por defecho "id".
    - Eloquent usa las columnas created_at y updated_at para actualizar automáticamente esos valores de tiempo.


- Artisan nos ayuda a crear los modelos:
 ```
 php artisan make:model Study    // sólo modelo
 php artisan make:model Study -m // modelo y migración
 ```
- La migración es un fichero que nos permite ir creando a la BBDD a nuestra medida
    - Método up: crea o modifica una tabla
    - Método down: retrocede los cambios o borra la tabla


- Si queremos cambiar los parámetros por defecto respecto a nombre de tabla, clave primaria y uso de columnas temporales:

 ```php 
<?php
namespace App;
use Illuminate\Database\Eloquent\Model;
class Study extends Model
{
   protected $table = 'estudio';
   $primaryKey = 'id_estudio';
   public $timestamps = false;
}
``` 


- Veamos un ejemplo de CRUD Study. Vamos a ver el código de las clases Study y de StudyController.

 - Clase modelo. Muy sencillo porque Eloquent nos provee de los métodos que necesitamos.
 
```php
<?php
namespace App;

use Illuminate\Database\Eloquent\Model;

class Study extends Model
{
    //$fillable indica que atributos se deben cargar en asignación masiva.
    protected $fillable = ['code', 'name', 'shortName', 'abreviation'];
}
```


- Clase modelo. Usamos los métodos que nos facilita Eloquent como son:
```
find($id) //Busca un registro
findOrFail($id) //Similar pero lanza excepción si falla. 
all() //Carga todos los registros.
paginate() //Carga una página de registros. Ver paginación.
save() //Guarda el objeto previamente cargado.
push() //Guarda el objeto y si hay objtetos relacionados modificados también lo hace.
delete() //Borra el objeto previamente cargado.
destroy($id) //Localiza y borra un registro (la variable tb. podría ser un array de ids).
```


- El controlador:

```php
<?php
namespace App\Http\Controllers;

use Illuminate\Http\Request; 
use App\Http\Requests\StudyRequest; //usado para validación avanzada
use App\Study; //modelo Study

class StudyController extends Controller
{
    public function index()
    {
        $studies = \App\Study::all(); //toma todos los registros
        return view('study.index', ['studies' => $studies]);
    }
    
    public function create()
    {
        return view('study.create');
    }
    
    public function store(Request $request)
    {
        $this->validate($request, [
            'code' => 'required|unique:studies',
            'abreviation' => 'required|unique:studies',
            'name' => 'required',
            'shortName' => 'required'
            ]);
        $study = new Study($request->all());
        $study->save();
        return redirect('/study');
    }

    public function delete($id)
    {
        // modo 1
        // $study = Study::find($id);
        // $study->delete();
        // modo 2
        Study::destroy($id);
        return redirect('/study'); //redirigir
    }

    public function update($id)
    {
        $study = Study::find($id);
        return view('study.update', ['study' => $study]);
    }

    public function save(StudyRequest $request)
    {
        $study = Study::find($request->id);
        $study->code = $request->code;
        $study->name = $request->name;
        $study->shortName = $request->shortName;
        $study->abreviation = $request->abreviation;
        $study->save();
        return redirect('/study');
    }
}
```



## Response

* Podemos reenviar la respuesta de diferentes modos:


* Reenvio a una ruta concreta: 

```php
//fichero de rutas:
//ruta destino con nombre definido
Route::get('destino/{sitio}', function ($sitio) {
    return "Destino: $sitio";
})->name('destino');
```

* Reenvío a rutas con nombre o con URL:

```php
//código en el una fichero de rutas pero que podría ir en el controlador:
//reenvío ordinario por la URL
Route::get('teruel', function () {
    return redirect('/destino/Teruel');
});
//reenvío por nombre
Route::get('zaragoza', function () {
    return redirect()->route('destino', ['sitio' => 'Zaragoza']);
});
```


* Reenvío a la misma ruta de la que venimos, e incluso añadir mensajes de error:

```php
//redirección simple:
return Redirect::back();

//igual pero con errores
return Redirect::back()->withErrors(['msg', 'The Message']);

//en la vista podríamos hacer:

@if($errors->any())
<h4>{{$errors->first()}}</h4>
@endif
```


* Reenviar a un método de un controlador:
  ```php
  //a un método sin argumentos
  return redirect()->action('HomeController@index');
  //con argumentos
  return redirect()->action(
    'UserController@profile', ['id' => 1]
  );
  ```



## Migraciones y Seeders


### Migraciones

* Laravel provee un sistema llamado "migraciones" que permite elaborar la estructura de la base de datos de un modo paulatino.
* Las migraciones se asemejan a un control de versiones para la base de datos. Cada migración puede desplegarse o deshacerse según se precise.
* Facilitan enormemente el trabajo en equipo y mediante SCV \(git u otro\).
* Facilitan la migración de SGBD.
* Facilitan la implantación en nuevos equipos de desarrollo o en producción.


* Debemos crear una migración por cada tabla.
* Debemos crearla con artisan y editarla después.
* Nota, en la versión 6 podemos crearla a la vez que el modelo:

```
php artisan make:migration create_studies_table  //crear migración
php artisan make:model -m Study //crear modelo y migración en un comando
```


* Ejemplo de migración de la tabla studies:

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateStudiesTable extends Migration
{
    /**
     * up crea la tabla
     * down la elimina
     */
    public function up()
    {
        Schema::create('studies', function (Blueprint $table) {
            $table->id();
            $table->string('code')->unique();
            $table->string('name');
            $table->string('shortName');
            $table->string('abreviation');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('studies');
    }
}
```


- Para modificar una tabla tenemos dos opciones:
    - Modificar la migración de creación. NO SIEMPRE POSIBLE
    - Crear una migración de modificación. Ejemplo: 

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class ModifyUsersTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->bigInteger('role_id')->unsigned()->default(1)->after('id');
            $table->foreign('role_id')->references('id')->on('roles');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->dropForeign('users_role_id_foreign');
            $table->dropColumn('role_id');
        });        
    }
}

```


## Ejecutar migraciones

*
* Las migraciones se gestionan desde la consola mediante comandos de _artisan_.

  * El comando base es `php artisan migrate`. En los siguientes apartados vamos a ver las variantes.

  ```
  migrate:install //Crea una tabla en la base de
    datos que sirve para llevar el control de las migraciones realizadas y su orden.
  migrate:reset //Realiza una marcha atrás de todas las migraciones en el sistema, volviendo el schema de la
    base de datos al estado inicial.
  migrate:refresh //Vuelve atrás el sistema de migraciones y las vuelve a ejecutar todas.
  migrate:rollback //Vuelve hacia atrás el último lote de  migraciones ejecutadas.
  * \ //hp artisan migrate:rollback --step=2 Vuelve atrás las últimas migraciones, en este ccaso los últimos dos lotes.
  migrate:status //Te informa de las migraciones presentes en el sistema y si están ejecutadas o no.
  ```


  ## Creación de migraciones

  * Se usa `artisan make:migration`:
    ```
    // ej. comando base.
    php artisan make:migration create_users_table
    // ej. comando para creación de tabla. Añade código de creación.
    php artisan make:migration create_users_table --create=users
    ```

* El resultado es una clase hija guardada **database/migrations** 
* El fichero se nombra usando el *timestamp* de creación para que sea ejecutado en el orden correcto.


### Métodos de una migración
* up\(\) sirve para modificar la base de datos. Típicamente crear una tabla.
* down\(\) sirve para devolver la base de datos a su estado previo. Típicamente borrar una tabla.

```php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateStudiesTable extends Migration
{
    /**
     * Run the migrations.
     * Ejecutar migración
     * @return void
     */
    public function up()
    {
        Schema::create('studies', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     * Deshacer
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('studies');
    }
}
```


### Código de las migraciones

* Dentro de las funciones _up_ y _down_ usamos las funciones de la clase esquema para:

  * Crear tablas: `Schema::create('nommbreTabla', 'funcion_closure');`
  * Renombrar tablas: `Schema::rename\($original, $nuevo\);`
  * Borrar tablas: `Schema::drop('users');` o `Schema::dropIfExists('users');`
  * Modificación de tablas: `Schema::table('nommbreTabla', 'funcion_closure');`


* En las funciones anónimas \(closure\) de creación y modificación podemos hacer prácticamente cualquier cosa que soporte SQL. Debemos usar los métodos de la clase Blueprint aplicados a la variable $table:

  ```php
  $table->string('email');  //añadir columna de texto VARCHAR(100). Hay métodos para muchos tipos de datos. Ver documentación.
  $table->string('name', 20);
  $table->tinyInteger('numbers');
  $table->float('amount');
  $table->double('column', 15, 8);
  $table->integer('votes');

  //podemos añadir modificadores e índices:
  $table->id();  //Clave principal autoincremental usado por defecto.
  $table->char('codigo', 25)->primary(); //Otras claves principales
  $table->primary(['area', 'bloque']); // o así...
  $table->string('email')->nullable(); //añadir columna y permitir nulos.
  $table->timestamps(); //añade columnas create_at, update_at
  $table->string('email')->unique();  // columna con indice único
  $table->index(['account_id', 'created_at']); //índice multicolumna

  //claves ajenas: columna del tipo adecuado más f.k.
  $table->integer('user_id')->unsigned();
  $table->foreign('user_id')->references('id')->on('users');
  //podríamos añadir ->onDelete(cascade) ...
  ```


  * Los posibles modificadores son:
    ```
    ->first() | Place the column “first” in the table (MySQL Only)
    ->after('column') | Place the column “after” another column (MySQL Only)
    ->nullable() | Allow NULL values to be inserted into the column
    ->default($value) | Specify a “default” value for the column
    ->unsigned() | Set integer columns to UNSIGNED
    ->comment('my comment') | Add a comment to a column
    ```

* Para otras operaciones consultar la documentación oficial.


### Seeders \(o sembradores\)

  * Los _seeders_ son clases usadas para:
  * Rellenar las tablas con datos de prueba
  * Cargar las tablas con datos iniciales.
  * Creación con artisan:

  `php artisan make:seeder StudySeeder`

  * Los seeders tienen un método _run\(\)_ donde se registra el contenido de los registros a insertar. Puede usarse sintáxis de _Query Builder_ o de _Eloquent_


* Ejemplo:

```php
use Illuminate\Database\Seeder;
use App\Review;
class StudiesSeeder extends Seeder
{
    public function run()
    {
        //con eloquent
        Study::create([
        'code' => 'IFC303',
        'name' => 'Desarrollo de Aplicaciones Web',
        'abreviation' => 'DAW'
        ]);

        //lo mismo con query builder
        DB::table('studies')->insert([
        'code' => 'IFC303',
        'name' => 'Desarrollo de Aplicaciones Web',
        'abreviation' => 'DAW
        ]);
    }
}
```


* El orden en que se ejecutan los seeders se establece manualmente. Debe rellenarse el método _run\(\)_ de la clase _DatabaseSeeder_ ubicada en _database/seeds_

```php
public function run()
{
    $this->call(StudySeeder::class);
    $this->call(ModuleSeeder::class);
}
```


* Su ejecución es desde artisan:
  * Modo general
    `php artisan db:seed`
  * Junto a las migraciones:
    `php artisan migrate:refresh --seed`
  * De forma individual
    `php artisan db:seed --class=ModuleSeeder`


### Model Factories

* Crear un _factory_:

```
php artisan make:factory PostFactory
```

* Codificar un factory:

```php
use Faker\Generator as Faker;

$factory->define(App\User::class, function (Faker $faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->unique()->safeEmail,
        'password' => '$2y$10$TKh8H1.PfQx37YgCzwiKb.KjNyWgaHb9cbcoQgdIVFlYg7B77UdFm', // secret
        'remember_token' => str_random(10),
    ];
});
```


* Usar el factory:

  * Crear un objeto con `make()` o con `create()`, el primero crea una variable, el segundo además la guarda en base de datos:

  ```php
      $user = factory(App\User::class)->make();
      $user = factory(App\User::class)->create();
  ```
  * Podemos crear más de un registro en la misma orden:
  ```php
  $users = factory(App\User::class, 3)->create();
  ```


  * Además podemos fijar o sobreescribir el valor de los campos:

```php
$user = factory(App\User::class)->create([
    'name' => 'Abigail',
]);
```



## Relaciones entre tablas/modelos


- El ORM Eloquent no pone fácil para tratar las relaciones entre tablas.
- Basta añadir algunos métodos a los modelos implicados.
- Vamos a ver únicamente las relaciones 1:N
- Para relaciones 1:1 y N:M ver la documentación


- Vamos a verlo con el ejemplo entre usuarios y roles: un `Role` puede tener varios `Users` y un `User` pertenece a un `Role`.
- Crear modelo Role (y migración, seeder y controlador)
  - `-m` añade la migración
  - `-s` añade el seeder
  - `-cr` añade el controlador (c) tipo resource (r)

```php
php artisan make:model Role -mscr
```


- Métodos de la migración
```php
    public function up()
    {
        Schema::create('roles', function (Blueprint $table) {
            $table->id();
            $table->string('name')->unique();
            $table->timestamps();
        });
    }
    public function down()
    {
        Schema::dropIfExists('roles');
    }
```


- Modificación de la tabla users.
  - Es habitual tener que modificar tablas que ya existen. Para eso usamos migraciones un poco diferentes:

```php
    public function up()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->unsignedBigInteger('role_id')->default(1);        
            $table->foreign('role_id')->references('id')->on('roles');
        });
    }
    public function down()
    {
        //las fk se mombran así: tabla_columna_foreign
        //esto se usa para eliminar la clave en el down
        Schema::table('users', function (Blueprint $table) {
            $table->dropForeign('users_role_id_foreign');
            $table->dropColumn('role_id');
        });
    }
```


- Creamos o modificamos el RoleSeeder y el UserSeeder:

```php
    public function run()
    {
        //con DB
        \DB::table('roles')->insert([
            'id' => '1',
            'name' => 'registrado'
        ]);
        
        \DB::table('roles')->insert([
            'id' => '2',
            'name' => 'usuario'
        ]);
        
        \DB::table('roles')->insert([
            'id' => '3',
            'name' => 'administrador'
        ]);
        
    }
```


```php
    public function run()
    {
        //con DB
        \DB::table('users')->insert([
            'id' => '1',
            'name' => 'registrado',
            'email' => 'registrado@dws.es',
            'password' => bcrypt('secret')
        ]);
    
        \DB::table('users')->insert([
            'id' => '2',
            'name' => 'usuario',
            'email' => 'usuario@dws.es',
            'password' => bcrypt('secret'),
            'role_id' => 2
        ]);
    
        \DB::table('users')->insert([
            'id' => '3',
            'name' => 'admin',
            'email' => 'admin@dws.es',
            'password' => bcrypt('secret'),
            'role_id' => 3
        ]);
    }
```


- No debemos olvidar actualizar el DatabaseSeeder:

```php
    public function run()
    {
        $this->call(RoleSeeder::class);
        $this->call(UserSeeder::class);
        $this->call(StudiesSeeder::class);
        // \App\Models\User::factory(10)->create();
    }
```


- Por fín los modelos: Vamos a tratar una relación 1:N
- Un `Role` tiene muchos `Users` asociados.
- La clase Role debe incluír el siguiente método: 

```php
class Role extends Model
{
    // resto del código
    //el método users nos devolverá una colección de usuarios
    //el nombre es significativo
    public function users()
    {
        return $this->hasMany(User::class);
    }
}
```


- Un `User` pertenece a un `Role`.
- La clase Role debe incluír el siguiente método:

```php
    class User extends Authenticatable
    {
        // resto del código
        //el método role nos devolverá un objeto role
        // el nombre también es significativo
        public function role()
        {
            return $this->belongsTo(Role::class);
        }       
    }
```


- Ahora podemos usarla en nuestras vistas.
- Por ejemplo en "user/index.blade.php"

```php
    @foreach($users as $user)
    <tr>
        <td>{{ $user->name }}</td>
        <td>{{ $user->role->name }}</td>
    </tr>
    @endforeach
```


- O en "role/show.blade.php"

```php
{{ $role->name }}

<h2>Lista de usuarios</h2>

@foreach ($role->users as $user)
  <li>{{$user->name}}</li>
@endforeach
```



## Middleware

- Los middleware son filtros que se ejecutan antes de que el control pase a una ruta o a un controlador.

- Podemos asciar un middleware a una ruta:
```php
Route::get('profile', 'UserController@show')->middleware('auth');
```
- O a un controlador, en su constructor:
```php
class UserController extends Controller
{
    public function __construct()
    {
        $this->middleware('auth'); //aplicable a todos los métodos
        $this->middleware('log')->only('index');//solamente a ....
        $this->middleware('subscribed')->except('store');//todos excepto ...
    }
}
```


### kernel.php

- En el kernel.php de la aplicación se definen varias cuestiones relativas a los middleware.
- Un grupo de middleware que se aplican a todas las peticiones. Atributo $middleware.
- Se asignan nombres a los middleware para ser usados en las rutas. Atributo `$routeMiddleware`.
- Dos grupos de middleware:
    - _web_ que se aplica a todas las rutas definidas en el fichero de rutas web.php
    - _api_ que se aplica a las rutas del fichero api.php


### Ejemplos de middleware

- _CheckForMaintenanceMode_ Se aplica a todas las rutas. Se activa su funcionalidad con los comandos:
```
php artisan down
php artisan up
```
 - Después de down genera un error 503
 - Comprueba la existencia del fichero /storage/framework/down
 - Down crea el fichero, up lo borra.


- Vamos a ver los que se aplican a todas las peticiones web.
 - EncryptCookies. Encripta las cookies
 - AddQueuedCookiesToResponse. Añade las cookies creadas mediante el método `Cookie::queue($micookie)`;
 - StartSession. Inicia sesión de forma automática.
 - ShareErrorsFromSession Hace visible el objeto errors tras la validación.
 - VerifyCsrfToken. Verifica los token CSRF.
 - SubstituteBindings ¿?


- Además, dispondemos de los siguientes definidos en route:
 - auth. Pasa el filtro si estás autenticado.
 - guest. Pasa el filtro si no has hecho login
 - can. Filtro usado para autorización.
 - throttle. Limita el número de peticiones por minuto aceptadas desde un host determinado.


### Construir un middleware:
 - Usamos artisasn:
 ``` 
 php artisan make:controller MiddlewareEsAdmin
 ```
 - Código. 
  - La lógica está en la función handle.
  - Si se dan las condiciones adecuadas se pasa el filtro (`return $next($request);`)
  - Si no se dan las condiciones abortar o redirigir.


 ```php
 <?php

namespace App\Http\Middleware;

use Closure;

class IsAdmin
{
    public function handle($request, Closure $next)
    {
        $user = $request->user();
        if ($user && $user->username == 'admin') {
            return $next($request);
        }
        abort(403, 'acceso denegado');
    }
}
```

- Registrar el middleware en el kernel dentro de $routeMiddleware


- Ya podemos usarlo como el resto:

```php
Route::get('families/ajax', 'FamilyController@ajax')->middleware('admin');
``` 



## Sesiones y Autenticación


### Autenticación
- Ya vimos como añadir el sistema de autenticación propio de Laravel
- Disponde de un juego de migraciones, rutas, controladores y vistas que hacen posible la gesión de usuarios

* Documentación oficial: [https://laravel.com/docs/7.x/frontend#introduction](https://laravel.com/docs/7.x/frontend#introduction)
* Autenticarse es identificarse, que la apliación nos reconozca como un usuario concreto.


* Para hacerlo debemos hacer uso de variables de sesión.
* Laravel viene con un sistema base muy completo. Vamos a usar la versión usada en Laravel 7:
  ```
    composer require laravel/ui
    php artisan ui bootstrap --auth
  ```
* Las vistas generadas están construidas con Twiter Bootstrap y se almacenan en el directorio: resources/views/auth.


* Rutas:
  * Para login se usa `/login`
  * Tras el login se usa `/home`. Puede personalizarse en el LoginController.
  * Por defecto el nombre de usuario es su e-mail.
  * Para cerrar la sesión: `logout`
  * Para resetear la contraseña: `password/reset`


* Para el reseteo de contraseñas se envia un correo con un token.
* Para pasar los mails a log y no enviar realmente:
  * En el fichero `.env` cambiar el parámetro: \`MAIL\_DRIVER=log
    \`
  * Completar en config/mail.php:
    ```php
    'from' => [
      'address' => env('MAIL_FROM_ADDRESS', 'admin@example.com'),
      'name' => env('MAIL_FROM_NAME', 'Adminstrador'),
    ],
    ```
  * Los correos se guardan en storage/logs
  * Podemos ver el correo enviado para resetear contraseña


* Si queremos acceder a la información del usuario actual debemos usar la clase fachada Auth.

```php
//Mostrar la información de usuario
Route::get('usuario', function () {
    $user = Auth::user();  //namespace global: \Auth
    dd($user);
    //si sólo nos interesa el id podríamos hacer:
    //$id = Auth::id();
});
```


### Sesiones

* Los parámetros de configuración de sesiones se definen en `config/session.php`. 
* Importante es el almacenamiento y el tiempo de vida de la sesión.
* Con Laravel no es preciso usar directamente la variable $_SESSION.
* ni debemos usar las funciones session\_start\(\)  o session\_destroy\(\)


* Podemos acceder a un dato guardado en sesión de tres modos:

  * El request, esto requiere inyección de dependencias:
    ```php
    public function showProfile(Request $request, $id)
    {
      $value = $request->session()->get('key');
    }
    ```
  * La fachada Session
    ```php
    $value = Session::get('key');
    ```
  * La función helper session\(\):
    ```php
    $value = session('key');
    ```


* Obtener todos los datos de una sesión
  ```php
  $data = $request->session()->all();
  $data = Session::all(); //usando el Facade
  ```

* Podemos guardar datos en sesión de dos modos:

  ```php
  $request->session()->put('email', 'me@example.com');
  session(['email' => 'me@example.com']); //usando el helper
  ```

* Podemos eliminar datos de sesión:

  ```php
  $request->session()->forget('key'); //elemento key
  $request->session()->flush();   // todos
  Session::flush();               //todos con la fachada
  ```


* Guardar datos sólo durante una petición \(request\) sólamente. 
* Esto se usa en validaciones de forma automática, transparente al usuario 
    - Para los datos antiguos (old)
    - Para los mensajes de error
  ```php
  //después de una petición se elimina. 
  $request->session()->flash('mensaje', '¡El usuario fue eliminado!');   
  //guardar los datos flasheados un request más.
  $request->session()->reflash();  
  //guardadr datos flasheados.
  $request->session()->keep(['mensaje', 'email']); 
  ```



## Query Builder. Paginación

- Laravel nos permite realizar gran cantidad de [consultas](https://laravel.com/docs/8.x/queries) sin escribir sql.
- Hasta el momento, sobre los modelos hemos usado algunos métodos: find(), all(), save(), first().


- Las consultas pueden construirse a través de los modelos (User, Role, ...) o de la clase DB:

```php
//todos los usuarios:
$user = User::first(); //mejor así para obtener objetos User
$user = DB::first(); //así obtenmos objetos "básicos"
```


- Vamos a poder filtrar usando `where` en alguna de sus variantes.
- El método `all()` no admite filtrado con where. 
- Debemos usar `get()`
```php
    $users = User::where('id', 100)->get();
    $users = User::where('id', '>', 100)->get();

    $users = User::where('id', '<=', 100)
        ->where('email', 'admin@dws.es')
        ->get();
```


- Existen otras variantes que podemos consultar en la documentación:
  - orWhere()
  - whereBetween()
  - whereIn()....
- No vamos a ver su uso. Queda como investigación del alumno.


- Para paginar basta con cambiar `get()` por `paginate` 
```php
    $users = $query->paginate(); //tamaño 15
    $users = $query->paginate($pageSize); //tamaño $pageSize
```

- En la vista la generación del índice de páginas es automático.
```php
    {{$users->links()}} //estándar: Tailwind CSS
    {{$users->links('pagination::bootstrap-4')}} //Bootstrap
```

- En la rama del proyecto de clase "clase07" ha sido necesario depurar el código
- Puedes ver las explicaciones en los vídeos de dicha rama.



## Validación

- La validación es una cuestión fundamental en cualquier aplicación, no sólo web. Cualquiera.
- Validar consiste en verificar la validez de los datos antes de procesarlos.


- ¿Validar en cliente o en servidor?.
 - En el cliente es más rápido, no necesitamos conectar con el servidor ni hacer uso de la red.
 - Pero nunca debemos confiar en la validación realizada en el cliente. Puede ser burlada intencionadamente o anulada de forma involuntaria.
 - Validar en el cliente es opcional pero en el servidor es obligatorio.


### Cómo validar en Laravel
- Existen varias maneras de hacerlo.
- Nosotros usaremos el método *validate* del objeto $request.

Ejemplo:
```php
public function store(Request $request)
{
    $request->validate([
    'title' => 'required|unique:posts|max:255',
    'body' => 'required',
    ]);
    //validado ->  a guardar
} 
``` 


### ¿¿Qué pasa tras la validación ??
- Si la validación falla se produce una excepción y se reenvía la petición a la URL previa.
- Se crea un objeto $errors que permite mostrar los errores de validación.
- Los datos del formulario se _flashean_ a la sesión y estarán disponibles sólo en la próxima peticicón para ser volver a rellenar el formulario como estaba.
 
- OJO. Si usamos Ajax se envía una respuesta al cliente con un código 422 y un JSON con los mensajes de error.


### Rellenando con los datos _viejos_
- La funcion helper _old()_ nos permite acceder a los datos que no se han validado y están en la sesión.
```php
<label>Nombre:</label>
<input type="text" name="name" value="{{ old('name')}}">
```


### Mostrando los errores con blade
- Todos los errores de la página
```php
@foreach ($errors->all() as $error)
<li>{{ $error }}</li>
@endforeach
```
- Mostrar los errores de un campo:
```php
//sólo el primero
{{ $errors->first('name') }}
//o todos los del campo
@foreach($errors->get('name') as $error)
{{ $error }}
@endforeach 
```


- También podemos usar la directiva *@error* de blade:

```php
<label for="title">Post Title</label>

<input id="title" type="text" class="@error('title') is-invalid @enderror">

@error('title')
    <div class="alert alert-danger">{{ $message }}</div>
@enderror
```


### Traduciendo mensajes

- Podemos traducir todos los literales de Laravel. No sólo los de validación.
- Debemos acceder a la [siguiente ruta](https://github.com/Laravel-Lang/lang)
- Descargar la carpeta del idioma que nos interese.
- Configurar el lenguaje en config.php 


### Reglas
- Se pueden añadir múltiples reglas para cada campo. Entre comillas y separadas por la barra vertical. 
- La regla _bail_ hace que no se sigan evaluando más reglas si esa falla.
- Revisar la documentación oficial: Available Validation Rules.


### Unique y update

- Ojo cuando apliquemos la regla *unique* en actualizaciones (update).
- Si aplicamos la regla sin más va a fallar porque ya existe un registro con ese dato:

```php
    'code' => 'required|unique:studies|max:6',
```
- Debemos indicar que no tenga en cuenta el propio registro para comprobar esta regla:

```php
    'code' => 'required|max:6|unique:studies,code,' . $study->id,
```


### Otros modos de validar

- Hemos planteado la forma habitual pero existen otras:
    -   Creando clases *Request* a la medida de una validación.
    - Creando clases *Validator*
- Ambas soluciones son algo más sofisticadas y no las vamos a usar aunque podrían dar solución a situaciones más complejas.



## Autorización: reglas y políticas
- Laravel nos brinda dos sistemas para autorizar acciones: reglas y puertas.
- Reglas
 - Las reglas se definen asociadas a la clase _Gate_.
 - Son una solución más simple.


- Políticas
 - Son soluciones más elaboradas que engloban la autorización referida a un modelo concreto.
 - Se definen en clases dentro guardadas en _app/Policies_
- Tienen una analogía a rutas y controladores, simpliciad vs complejidad y orden.
- En un proyecteo usaremos una u otra (o ambas) de acuerdo a su complejidad y envergadura.


### Definir reglas

- Las reglas se definen en el AuthServiceProvider.php, dentro de su función boot.
- Podemos definir una regla con un closure (función anónima) dentro de dicho método:
```php
public function boot()
{
    $this->registerPolicies(); 
    //nuestra regla es esta:
    Gate::define('update-post', function ($user, $post) {
        return $user->id == $post->user_id;
    });
}
```


### Usar reglas
- Para comprobar las reglas debemos usar la fachada Gate y alguno de sus métodos (allows, denies). Lo podemos hacer en la propia ruta o más correctamente en el controlador o en un middleware:
```php
if (Gate::denies('update-post', $post)) {
    abort(403);
    //o podríamos redireccionar:
    // return redirect('/home');
    // o mostrar una vista
    // return view('nombreVista');
}
```


- Observa que el $user no es pasado a la clase Gate, toma el autenticado. Si quisieramos pasarlo debemos hacerlo así:  
```php
//preguntando si se le permite
if (Gate::forUser($user)->allows('update-post', $post)) {
    // The user can update the post...
}
//o al revés ...
if (Gate::forUser($user)->denies('update-post', $post)) {
    // The user can't update the post...
}
```


### Políticas
- Si el proyecto es pequeño la solución anterior es válida pero para algo grande puede no ser una buena solución.
- Las políticas separan código para no sobrecargar el código del AuthServiceProvider.
- Las políticas son clases guardadas en app/Policies y que se crean así:
```php
//clase en blanco
php artisan make:policy PostPolicy
//clase con reglas CRUD predefinidas asociada a un modelo
//recomendable:
php artisan make:policy PostPolicy --model=Post
```


- Tras crear las políticas, éstas deben ser regitradas en el AuthServiceProvider:
```php
class AuthServiceProvider extends ServiceProvider
{
    //deben incluírsee en el siguiiente array:
    protected $policies = [
        Post::class => PostPolicy::class,
    ];
```


### Escribiendo las políticas:
- Podemos crear los métodos que necesitemos. Por ejemplo el método update lo podemos usar para actualizar un objeto del modelo afectado. Sus argumentos deben ser el user y un objeto del modelo protegido:
```php
public function update(User $user, Post $post)
{
    return $user->id === $post->user_id;
}
//aunque si el método no usa ningún objeto del modelo lo podemos dejar así:
public function create(User $user)
{
    return true; //una expresión booleana...
}
```


- El método before permite tomar decisiones generales sin evaluar la política

```php
public function before($user, $ability)
{
    if ($user->isSuperAdmin()) {
        return true;
    }
}
// si devuelve true -> autorizado. No comprueba nada más
// si devuelve false -> denegado. No comprueba nada más
// si devuelve null -> lo que diga la regla usada
```

<!--
4 videos de como programar los permisos dinámicamente, registrando permisos y su asignación en bbdd
https://youtu.be/-go8BMcpCf4?list=PLdKRooEQxDnW6OsnV4HQuVo-SX8fCgD1s
--> 



## TODO:
- Test
- Modelos y BBDD
- Errores
- Eloquent & Query Builder    
- Relaciones
- Multi Idioma
- Ajax
- Paginación
- Mail
- Pdf
- Trabajo




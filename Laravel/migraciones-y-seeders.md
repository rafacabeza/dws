# Migraciones

* Laravel provee un sistema llamado "migraciones" que permite elaborar la estructura de la base de datos de un modo paulatino.
* Las migraciones se asemejan a un control de versiones para la base de datos. Cada migración puede desplegarse o deshacerse según se precise.
* Facilitan enormemente el trabajo en equipo y mediante SCV \(git u otro\).
* Facilitan la migración de SGBD.
* Facilitan la implantación en nuevos equipos de desarrollo o en producción.
* Debemos crear una migración por cada tabla.
* Para modificar una tabla podemos modificar la migración implicada o añadir una nueva migración de modificación.

```
php artisan make:migratioin create_study_table
```

* Ejemplo de migración de la tabla study:

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
            $table->increments('id');
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

## Ejecutar migraciones

*
* Las migraciones se gestionan desde la consola mediante comandos de _artisan_.

  * El comando base es `php artisan migrate`. En los siguientes apartados vamos a ver las variantes.
  * `migrate:install` Crea una tabla en la base de
    datos que sirve para llevar el control de las migraciones realizadas y su orden.
  * `migrate:reset` Realiza una marcha atrás de todas las migraciones en el sistema, volviendo el schema de la
    base de datos al estado inicial.
  * `migrate:refresh` Vuelve atrás el sistema de migraciones y las vuelve a ejecutar todas.
  * `migrate:rollback` Vuelve hacia atrás el último lote de  migraciones ejecutadas.
  * \`php artisan migrate:rollback --step=2 Vuelve atrás las últimas migraciones, en este ccaso los últimos dos lotes.
  * `migrate:status` Te informa de las migraciones presentes en el sistema y si están ejecutadas o no.

  ## Creación de migraciones

  * Se usa `artisan make:migration`:
    ```
    // ej. comando base.
    php artisan make:migration create_users_table
    // ej. comando para creación de tabla. Añade código de creación.
    php artisan make:migration create_users_table --create=users
    // ej. comando de modificación de tabla
    php artisan make:migration add_votes_to_users_table --table=users
    ```

* El resultado es una clase hija de _Migration_, guardada en un fichero que comienza con la fecha de creación para que sea ordenado y ejecutado adecuadamente.
  ### Estructura de una migracion
* Las migraciones tienen dos métodos:
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
            $table->increments('id');
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
  * Renombrar tablas: \`Schema::rename\($original, $nuevo\);
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
  $table->increments('id');  //Clave principal autoincremental usado por defecto.
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

```php
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
  $users = factory(App\User::class, 3)->make(); //O create
  ```

  * Además podemos fijar o sobreescribir el valor de los campos:
  ```php
  $user = factory(App\User::class)->make([
    'name' => 'Abigail',
  ]);
  ```



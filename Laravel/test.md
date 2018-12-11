## Tests en Laravel

* Laravel viene preparado para usar phpunit desde el inicio
* Podemos crear casos de test mediante artisan:

  `php artisan make:test StudyTest`

* Los casos de test son clases que heredan de la clase TestCase \(tests/TestCase.php\).

* Nomenclatura:

  * Para probar el controlador _ExampleController_ usaremos _ExampleTest_.
  * Dentro de cada clase de test podemos añadir tantos métodos como necesitemos con la nomenclatura _testNombreMetodo_. OJO, debe empezar por _test..._

* Para ejecutar los test, desde la consola y en el directorio del proyecto ejecutamos: `./vendor/bin/phpunit`

### Ejemplos de tests

#### Empezando

* Aquí vemos la estructura de la clase y la nomenclatura.
* Ejemplo 1. 
  * visit\(\): visitar una URL.
  * see\(\): comprobar que se localiza un texto
  * dontSee\(\): viceversa

```php
<?php

use Illuminate\Foundation\Testing\WithoutMiddleware;
use Illuminate\Foundation\Testing\DatabaseMigrations;
use Illuminate\Foundation\Testing\DatabaseTransactions;

class ExampleTest extends TestCase
{
    /**
     * A basic functional test example.
     *
     * @return void
     */
    public function testBasicExample()
    {
        $response = $this->get('/');

        $response->assertSee('Laravel');
        $response->assertStatus(200);

    }
}
```

#### Enlaces y formularios

* Enlaces:

  * click\(\): pulsar interenlaces.
  * seePageIs\(\): comprobar URI.

* Formularios

  * type\($text, $elementName\) \| “Type” text into
    a given field. 
  * $this-&gt;select\($value, $elementName\) \| “Select” a radio button or drop-down
  * field. $this-&gt;check\($elementName\) \| “Check” a checkbox field.     
  * $this-&gt;uncheck\($elementName\)
    \| “Uncheck” a checkbox field. 
  * $this-&gt;attach\($pathToFile, $elementName\) \| “Attach” a file to the
    form. 
  * $this-&gt;press\($buttonTextOrElementName\) \| “Press” a button with the given text or name

#### Bases de datos

##### Ver cambios en la base de datos

* Problema. ¿He actualizado la base de datos realmente?

  * Está esta información??
    ```php
    ->assertDatabaseHas($table, array $data)
    ```
  * Ya no está esta otra??

    ```php
    ->assertDatabaseMissing($table, array $data) // no esta´en la BBDD
    ->assertSoftDeleted($table, array $data) //Comprobación de borrado "blando", no explicado.
    ```

    ##### Base de datos de pruebas

* Crear bbdd nueva en Mysql y dar permisos. Por ejemplo le llamamos `mysql_test`

* En phpunit.xml agregar:

  ```xml
  <php>
  ...
  <env name="DB_CONNECTION" value="mysql_tests"/> //nuevo
  </php>
  ```

  Esto se hace para alterar el valor DB\_DEFAULT indicado en config/database.php

* En cada clase de Test debemos importar y usar el trait RefreshDatabase:

```php
use Illuminate\Foundation\Testing\RefreshDatabase;

class UsersModuleTest extends TestCase
{
    use RefreshDatabase;

    // ...
```




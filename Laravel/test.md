# Tests en Laravel

* Laravel viene preparado para usar phpunit desde el inicio
* Podemos crear casos de test mediante artisan:

  `php artisan make:test StudyTest`

* Los casos de test son clases que heredan de la clase TestCase \(tests/TestCase.php\).

* Nomenclatura:

  * Para probar el controlador _ExampleController_ usaremos _ExampleTest_.
  * Dentro de cada clase de test podemos añadir tantos métodos como necesitemos con la nomenclatura _testNombreMetodo_. OJO, debe empezar por _test..._

* Para ejecutar los test, desde la consola y en el directorio del proyecto ejecutamos: `./vendor/bin/phpunit`

## Ejemplos de tests

### Empezando

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
### Bases de datos

* ¿Cómo probar el comportamiento de la aplicación para una tabla vacía y la misma tabla con una colección de registros?

* La solución pasa por:

  * Crear una base de datos para los test.

  * Crear las condiciones deseadas en cada test.

* Para logralo debemos hace varias cosas:

#### Base de datos de pruebas

* Crear bbdd nueva en Mysql. Por ejemplo le llamamos `laravel18test`


* En phpunit.xml agregar un parámetro referido a la nueva conexión:

  ```xml
  <php>
  ...
  <env name="DB_DATABASE" value="laravel18test"/> //nuevo
  </php>
  ```


* En cada clase de Test debemos importar y usar el trait RefreshDatabase:

```php
use Illuminate\Foundation\Testing\RefreshDatabase;

class UsersModuleTest extends TestCase
{
    use RefreshDatabase;

    // ...
```

* OJO. Ahora en cada test partimos de una base de datos limpia. Con las migraciones recien hechas y sin registros sembrados.

Vamos a ver una colección de posibles test para verificar el funcionamiento de un CRUD sobre la tabla usuarios.

* Lista vacía de usuarios

```php
  public function test_lista_de_usuarios_vacia()
  {
      $response = $this->get('/users');
      $response->assertStatus(200);
      $response->assertSee('Lista de usuarios');
      $response->assertSee('No hay usuarios');
  }
```

* Lista de usuarios con datos:

  * Primera aproximación. Fallará porque partimos de una tabla vacía:

```php
  public function test_lista_de_usuarios()
  {
      $response = $this->get('/users');
      $response->assertStatus(200);
      $response->assertSee('Lista de usuarios');
      $response->assertSee('Pepe');
      $response->assertSee('pepe@gmail.com');
  }
```

  * Test correcto. Usar _factories_ para crear registros antes del test.

```php

  public function test_lista_de_usuarios()
  {
      factory(User::class)->create([
          'name' => 'Pepe',
          'email' => 'pepe@gmail.com'
      ]);

      $response = $this->get('/users');
      $response->assertStatus(200);
      $response->assertDontSee('No hay usuarios');
      $response->assertSee('Lista de usuarios');
      $response->assertSee('Pepe');
      $response->assertSee('pepe@gmail.com');
  }

```

* Detalle de un registro:

```php
    public function test_metodo_show_user_1()
  {
      factory(User::class)->create([
          'id' => 1,
          'name' => 'Pepe',
          'email' => 'pepe@gmail.com'
      ]);

      $response = $this->get('/users/1');
      $response->assertStatus(200);
      $response->assertSee('detalle del usuario 1');
      $response->assertSee('Pepe');
      $response->assertSee('pepe@gmail.com');
  }

```

* Detalle de un registro con error 404:
```php
  public function test_metodo_show_user_inexistente()
  {
      $response = $this->get('/users/100000');
      $response->assertStatus(404);
  }
```

#### Ver cambios en la base de datos

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





### Enlaces y formularios

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

  
## Ejecución parcial de tests

- Podemos ejecutar un sólo test usando la opción `--filter`:

```
phpunit --filter {TestMethodName} //sintáxis
phpunit --filter test_metodo_show_user_inexistente  //ejemplo

phpunit --filter {TestMethodName} {FilePath} //sintáxis
phpunit --filter test_metodo_show_user_inexistente tests/Feature/UserTest.php //ejemplo
```

- O podemos agrupar los tests con la anotación `@group`:

```php
/**
* @group users
*/
public function test_lista_de_usuarios_vacia()
{
    $response = $this->get('/users');
    $response->assertStatus(200);
    $response->assertSee('Lista de usuarios');
    $response->assertSee('No hay usuarios');
}
```

En este caso la ejecución debe llevar el parámetro group:
    
```
phpunit --group=users
```
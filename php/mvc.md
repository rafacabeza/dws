# MVC y  Conexión a BBDD


## Objetivo:

- Vamos a refinar la arquitectura de nuestra aplicación
    - Aplicaremos una arquitectura MVC
    - Aplicaremos el patrón de controlador frontal.
- Vamos a ver como se hace la conexión y acceso a una base de datos. Usaremos un patrón [*Active Record*](https://es.wikipedia.org/wiki/Active_record)
- Completaremos una pequeña aplicación que refleje todo esto
- https://bitbucket.org/rafacabeza/mvc18



## MVC

![mvc](/assets/mvc.png)


### Ejemplo simple
- Rama mvc00.
- Un controlador: Controller.php
- Un modelo: User.php
- Dos vistas: index y show


El controlador: 

```php
<?php
require_once('User.php');

class Controller  
{
    public function index()
    {
        $users = User::all();
        // echo json_encode($users);
        require('views/index.php');
    }
    public function show()
    {
        $id = $_GET['id'];
        $user = User::find($id);
        // echo json_encode($user);
        require('views/show.php');
    }
}
```


El modelo:
```php
<?php

class User
{
    const USERS = [
        array(1, 'Juan'),
        array(2, 'Ana'),
        array(3, 'Pepa'),
        array(4, 'Toni')
    ];
    
    public static function all()
    {
        return User::USERS;
    }
    public function find($id)
    {
        return User::USERS[$id];
    }
}
```


Vista del listado:

```php
<h1>Lista de usuarios</h1>    
<table>
    <?php foreach($users as $user) {?>
        <tr>
        <td><?php echo $user[0] ?></td>
        <td><?php echo $user[1] ?></td>
        <td><a href="?method=show&id=<?php echo $user[0] ?>">Ver</a></td>
        </tr>
    <?php } ?>
</table>
```


Vista del detalle:
```php
<h1>Detalle de usuario</h1>
<li>
    <strong>Id: </strong>
    <?php echo $user[0]?>
</li>
<li>
    <strong>Nombre: </strong>
    <?php echo $user[1]?>
</li>
<hr>
<a href="/">Lista</a>
```



## Front Controller y Enrutamiento:


## Arquitectura usada:

* Inconvenientes de la arquitectura anterior:
    - Si hay múltiples recursos o tablas tendremos múltiples controladores
    - Cada uno de ellos supone un punto de entrada a la aplicación.
* La existencia de una entrada única:
    - Permite filtrar cualquier petición.
    - Permite realizar tareas sistemáticas:
        - Iniciar sesión
        - Comprobar si el usuario está autorizado


## Front Controller

* Se trata de la clase que recibe todas las peticiones
* Este controlador puede realizar tareas repetitivas, filtros, ...
* De acuerdo a la ruta recibida (URI), puede cargar un controlador u otro y ejecutar el método necesario.
* Esta funcionalidad se conoce como **enrutamiento**.
* Es interesante que el enutamiento sea *friendly*, es decir que evite los parámetros:
    ```
    # parámetros GET
    localhost/index.php?controller=user&method=index
    # user friendly
    localhost/users
    ```


* En nuestro proyecto de clase el front controller carga un controlador de manera fija de acuerdo a la ruta y después ejecuta uno de sus métodos:
* Por ejemplo la ruta `/user/index` carga el controlador UserController y ejecuta su método `index()`

```php
<?php
namespace Core;
/**
*
*/
class App
{

    function __construct()
    {
        if (isset($_GET['url'])) {
            $url = $_GET['url'];
        } else {
            $url = 'home';
        }

        $arguments = explode('/', trim($url, '/'));
        $controllerName = array_shift($arguments);
        $controllerName = ucwords($controllerName) . "Controller";
        if (count($arguments)) {
            $method =  array_shift($arguments);
        } else {
            $method = "index";
        }

        $file = "../app/controllers/$controllerName" . ".php";
        if (file_exists($file)) {
            require_once $file;
        } else {
            header("HTTP/1.0 404 Not Found");
            echo "No encontrado";
            die();
        }

        $controllerName = '\\App\\Controllers\\' . $controllerName;
        $controllerObject = new $controllerName;
        if (method_exists($controllerName, $method)) {
            try {
                $controllerObject->$method($arguments);
            } catch (\Exception $e) {
                header("HTTP/1.0 500 Internal Error");
                echo $e->getMessage();
                echo "<pre>";
                echo $e->getTraceAsString();
            }
        } else {
            header("HTTP/1.0 404 Not Found");
            echo "No encontrado";
            die();
        }
    }
}
```

* ¿Cómo hacemos que esta clase se ejecute en cada petición?
  * Fichero `.htaccess`
  * 




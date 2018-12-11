# Front Controller

* Se trata de la clase que recibe todas las peticiones
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




# Modelo vista controlador

## Idea general

El modelo–vista–controlador \(MVC\) es un patrón de arquitectura de software que separa los datos y la lógica de negocio de una aplicación de la interfaz de usuario y el módulo encargado de gestionar los eventos y las comunicaciones.

Para ello MVC propone la construcción de tres componentes distintos que son el modelo, la vista y el controlador.

El modelo es el responsable de recoger los datos de la aplicación. De su almacenamiento y lectura, por tanto es el elemento que se conecta con ficheros o bases de datos.

La vista se ocupa de generar el interfaz de usuario. En nuestro caso de generar el HTML de la apliación web, o como veremos el XML o JSON.

Para acabar, el controlador es el elemento que recoge las peticiones del navegador y decide qué elementos del modelo deben ser invocados y que vista se debe generar en cada caso.

En nuestro proyecto esto se refleja con la creación de una carpeta con cada uno de estos nombres, en inglés models, views, controllers.

![](/assets/mvc.png)

## MVC

* Haz un fork del repositirio `https://bitbucket.org/rafacabeza/mvc18`
* Después clónalo.
* Hay un fichero sql para la creación de la base de datos de trabajo.
* Vamos a analizar la estructura de ficheros que tenemos y que sigue la arquitectura MVC:
  * index.php
  * UserController.php, fichero de la clase controladora
  * User.php, fichero de la clase modelo
  * user.index.php, fichero  de vista para la lista de usuarios.

## El controlador frontal.

Se trata de un patrón de diseño. Aquí un enlace a [Wikipedia](https://en.wikipedia.org/wiki/Front_controller).

En una apliación de escritorio, la entrada de ejecución es única. En java, por ejemplo, es la función main de la clase principal.

Esa clase principal desempeña la función de controlador frontal. Es la que recoge la información de entrada recibida para determinar qué clase controladora debe gestionar en cada caso las peticiones recibidas.

En el caso de una aplicación web debemos hacer algo parecido. Nuestro punto de entrada será el fichero index.php de inicio. El index debe crear un objeto de la clase App que es el controlador frontal, la clase de entrada.

```php
<?php

require_once 'app/App.php';

echo "Apliacion MVC. Estamos en index.php <br>";
$app = new Bootstrap;
```

Fichero index.php

```php
<?php

/**
 * Description of App
 * Clase raíz que carga el controlador solicitado y ejecuta el método que se
 * haya pedido, con o sin argumentos.
 *
 * @rafacabeza
 */
class App
{
    public function __construct()
    {
        $controller = $_GET['controller'];
        $method = $_GET['method'];

        echo '<!DOCTYPE html>';   
        echo '<meta charset="UTF-8">';
        echo 'Estamos en Bootstrap, el controlador frontal. <br>';
        echo ' Esta será la raíz de la apliación de ahora en adelante. <br>' ;
        echo 'Esto es el __construct, algo asi como el main. <br>';

        echo '<hr>';
        if(empty($controller)){
            echo 'Observa la URL. Despues pulsa el siguiente enlace y vuelve a '
            . 'mirar<br>';
            echo '<a href="?controller=user&method=add"> Alta de usuario </a>';
        }
        else{
            echo "Vamos a ejecutar $controller-->$method";            
        }           
    }
}
```

Clase Bootstrap

### Por donde seguir

En el repositorio  encontrarás https://bitbucket.org/rafacabeza/mvc18 el código de un framework deesarrollado en clase. En el se irá viendo la arquitectura MVC, las conexiones a BBDD y otras cuestiones de PHP que han quedado en el tintero en lo visto hasta la fecha.


# Namespaces

* En un proyecto pequeño no es normal que nos aparezcan nombres de clases repetidas.
* Esto puede no ser así en proyectos grandes, especialmente cuando usamos librerías de terceros.

## Espacios de nombres, directorios y paquetes.

* Un pocom más de información:

  * [https://diego.com.es/namespaces-en-php](https://diego.com.es/namespaces-en-php)
  * [http://php.net/manual/es/language.namespaces.php](http://php.net/manual/es/language.namespaces.php)

* Los espacios de nombres son una equivalencia a los paquetes de Java. Se trata de agrupar clases. Y no sólo clases, funciones y constantes si las usamos.

* No es obligatorio pero se suele asimilar la idea de usar un directorio por cada namespace y que el nombre de ambos coincida.

* Si usamos namespaces, la referencia a una clase es semejante a la referencia a un fichero en el sistema de ficheros:

  * El fichero `foto.jpg` se refiere a un fichero dentro del directorio actual. Es una ruta relativa.
  * El fichero `fotos/foto.jpg` usa también una ruta relativa para referirse a un fichero ubicado en un subdirectorio `fotos` dentro del directorio actual.
  * El fichero `/home/alumno/fotos/foto.jpg` usa una ruta absoluta para referirse al mismo fichero.

* Definir un namespace, y una clase en él. Basta añadir una línea inicial en cada fichero php:

```php
<?php
namespace App/Model;

class User {
....
}
```

* Para usar el namespace podemos hacerlo usando rutas relativas o  absoluta

```php
<?php
namespace App/Controller;
require_once 'app/model/User.php;

class UserController {
    public function index {
        $user = new \App\Controller\User;
    }
}
```

* O podemos declarar su uso para hacerlo directamente:

```php
<?php
namespace App/Controller;
require_once 'app/model/User.php;

use \App\Controller\User;
class UserController {
    public function index {
        $user = new User;
    }
}
```




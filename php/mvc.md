# MVC y  Conexión a BBDD


## Objetivo:

- Vamos a refinar la arquitectura de nuestra aplicación
    - Aplicaremos una arquitectura MVC
    - Aplicaremos el patrón de controlador frontal.
- Vamos a ver como se hace la conexión y acceso a una base de datos. Usaremos un patrón [*Active Record*](https://es.wikipedia.org/wiki/Active_record)
- Completaremos una pequeña aplicación que refleje todo esto
- https://bitbucket.org/rafacabeza/mvc



## Configuración 
- Vamos a usar un nuevo sitio web: mvc.local
- Debes incluirlo en /etc/hosts
- Debes añadir la configuracion en el dockercompose. Ya lo tienes en GitHub.
- Debes crear la carpeta `data/mvc`.


### Carpeta public:

- En una aplicación Web servimos el contenido de un directorio.
- Este directorio es conocido como Document Root en Apache.
- Todo lo que hay en él es accesible desde el exterior
- Resulta interesante separar nuestro código *php* y otros recursos de lo que realmente queremos publicar.


- Por defecto el document root de Apache es `/var/www/html`
- Nosotros queremos cambiarlo a `/var/www/html/public`
- El resto de contenido dentro de html será inaccesible desde el exterior


- Para lograrlo hay que modificar la configuración de Apache:
- En el fichero de configuración:

```
DocumentRoot /var/www/html/public
```


- Nosotros estamos usando docker.
- Debemos crear nuestra carpeta public: es decir data/mvc/public.
- La modificación la realizamos desde nuestro Dockerfile. Debemos añadir:

```
ENV APACHE_DOCUMENT_ROOT=/var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf
```


** ¿Funciona? **

- Crea estos ficheros y conecta tu navegador a `mvc.local`

```php
#Fichero `mvc/public/index.php`
<?php
echo 'Contenido en public<br>';
require "../start.php";
```

```php
#Fichero `mvc/start.php`
<?php
echo 'Contenido fuera de public. Privado<br>';
```



## MVC

![mvc](/assets/mvc.png)


### Ejemplo simple
- Rama mvc00.
- Un fichero "cargador": start.php
- Un controlador: Controller.php
- Un modelo: User.php
- Dos vistas: index y show


El fichero *start.php*
```php
<?php
require_once "Controller.php";

$app = new Controller();

if(isset($_GET['method'])) {
    $method = $_GET['method'];
} else {
    $method = 'index';
}

if(method_exists($app, $method)) {
    $app->$method();
} else {
    http_response_code(404);
    die("No encontrado");
}
```


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
* Es interesante que el enutamiento sea *user friendly*, es decir que evite los parámetros:
    ```
    # parámetros GET
    localhost/index.php?controller=user&method=index
    # user friendly
    localhost/users
    ```


### Rutas amigables:
- Vamos a hacer que todas las peticiones lleguen a una única clase App de entrada.
- Nosotros escribiremos:

    ```http://mvc.local/user/show```

- Apache interpretará:  

    ```http://mvc.local/index.php?url=user/show```


### Módulo rewrite

- Para lograr esto necesitamos que Apache tenga activo el módulo *rewrite*. 
- Mira tu Dockerfile de mvc.
- Necesitamos además un fichero **.htaccess** en el *public*

```
#Options +FollowSymLinks
#activar motor de reescritura
RewriteEngine On  
#no sobreescribir directorios o ficheros si existen
#importante para css, js, imágenes, ...
RewriteCond %{SCRIPT_FILENAME} !-d 
RewriteCond %{SCRIPT_FILENAME} !-f 
#regla de reescritura de la url
#RewriteRule ^(.+)*$ index.php?url=$1 [QSA,L]
RewriteRule ^(.*)$ index.php?url=$1 [QSA,L]
```


### Procesar la ruta:

- Nuestro controlador frontal va a ser una clase llamada App.
- Nuestro start.ph debe crear un objeto App.
```php
<?php
require "../core/App.php";
$app = new App();
```
- El constructor de App llevará toda la ejecución de la aplicación, de acuerdo a la ruta recibida:


* En nuestro proyecto de clase el front controller carga un controlador de manera fija de acuerdo a la ruta y después ejecuta uno de sus métodos:
* Esquema que usaremos: `/controlador/metodo/argumento1/argumento2`
* Por ejemplo la ruta `/user/index` carga el controlador UserController y ejecuta su método `index()`
* Si no existe controlador tomamos uno por defecto, por ejemplo `index` o `home` .
* Si no existe método tomamos uno por defecto, por ejemplo `index`


```php

<?php
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

        $controllerObject = new $controllerName;
        if (method_exists($controllerName, $method)) {
            $controllerObject->$method($arguments);
        } else {
            header("HTTP/1.0 404 Not Found");
            echo "No encontrado";
            die();
        }
    }
}
```


### Un poco de orden:

- Necesitamos usar clases controladoras, modelos y vistas.
- Vamos a borrar las clases Controller y User de días anteriores, y el directorio views.
- Vamos a crear los siguientes directorios:

    ```
    mvc/app/controllers
    mvc/app/models
    mvc/app/views
    ```


** Probando un controlador **

```php
<?php
# app/controllers/UserController
class UserController  
{
    public function __construct()
    {
        echo "en el controller<br>";
    }
    public function index()
    {
        echo "en el index<br>";
    }
    public function show()
    {
        echo "en el show<br>";

    }
}
```



### Primeros controladores y organizar vistas

- Vamos a crear tres controladores básicos que sólo saluden.
- Vamos a añadir un poco de estilo: Bootstrap
- Vamos a dar cierta estructura a nuestras vistas:
    ```
    cabecera
    cuerpo
    pie
    ```


* Controladores básicos * 
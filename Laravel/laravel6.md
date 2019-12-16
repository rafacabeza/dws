# Laravel 6.*

- Laravel se ha convertido en el framework php más utilizado para desarrollo web.
- En septiembre/2019 se publicón la versión 6.0
- Desde entonces usa versionado semántico.
    - Febrero y agosto: versiones mayores: 6 (LTS), 7, ... 
    - Semanalmente pueden aparecer versiones menores (segundo dígito) y de parcheo (tercero). 
    - Las versiones mayores pueden incluír cambios de ruptura, el resto no.


- Usa arquitectura MVC (Modelo Vista Controlador) y mucho más:
- Sistema de enrutamiento potente
- Usa Blade como gestor de plantillas
- Su propio ORM: Eloquent
- Y muchos otros componentes que lo hacen muy interesante: Query Builder, Database Migrations, Seeders, Passport, Dusk, Socialite, ... y muchos otros componentes brindados por terceras partes.


- Enlaces:
    - [Wikipedia](https://en.wikipedia.org/wiki/Laravel) mejor en inglés.
    - [Sitio oficial](https://laravel.com/)
    - [Documentacion](https://laravel.com/docs)
    - [Laracast](https://laracasts.com/skills/laravel): videotutoriales oficiales
    - [Styde.net](https://styde.net/): numerosos videotutoriales, cursos y publicaciones
    - [Código fuente](https://github.com/laravel/laravel). Se puede clonar para iniciar proyecto.



## Instalación

- https://laravel.com/docs/6.x/installation
- Requisitos: 
    - php7.2 y algunos complementos
    - Composer como gestor de dependencias
- Podemos desarrollar sin servidor o con él (Apache on Nginx)
- Podemos dockerizar el entorno de desarrollo


- Instalación via composer (directorio blog):

```
composer create-project --prefer-dist laravel/laravel blog
```

- O partiendo del código de GitHub:

```
git clone https://github.com/laravel/laravel.git
composer install
```

- Se puede probar ejecutando:

```
php artisan serve  # para iniciar el servicio
chrl+C             # para cerrar
```


- Usando docker y nuestro entornods:
    - Descargamos nuestro entornods y nos ponemos en la rama laravel
    - Añadimos al fichero hosts:
    ```
    127.0.0.1   laravel.local
    ```
    - Levantamos nuestro servicio:

        ```
        cd entornods 
        docker-compose up -d
        ```
    - Colocamos nuestro Laravel limpio en _data/laravel_
    - ¿Cómo?


- __OPCION 1__ (composer)
    - Levantamos nuestro _entornods_. 
        - nota: hay que cambiar los permisos de data/laravel a 777.
    - Entramos en el contenedor _laravel_: 
    ```
    docker exec -it --user devuser laravel bash
    composer create-project --prefer-dist laravel/laravel blog
    ```

    - Ya podemos acceder a http://laravel.local


__ OPCION 2 __ (git + composer)
- Clonamos el código desde https://github.com/laravel/laravel.git o desde nuestro repositorio. 
- Lo hacemos en el directorio `entornods/data/laravel`

```
cd entornods/data
git clone <identificador ssh/https del repositorio de partida>
cd laravel
composer install
```

- El directorio de partida puede ser `git@github.com:laravel/laravel.git` o el de nuestro proyecto particular


OJO!!! 
- El _entornods_ viene configurado para una aplicación ubicada en data/laravel
- El resto de instrucciones también
- Si usamos otro nombre deberemos ajustar alguno de estos comados y el contenido del fichero _docker-compose.yml_


- Entramos en el contendor laravel y ejecuamos un par de comandos: 

    ```docker
    docker exec -it --user devuser laravel bash
    composer install
    cp .env.example .env   # El fichero .env define las variables de entorno
    php artisan key:generate  # genera una clave de cifrado guardada en .env
    ```

- Vamos al navegador con la url http://laravel.local


- El uso de la consola va a ser importante en el desarrollo con Laravel
- Es muy recomedable definir aliases de comandos pero mucho más si usamos docker
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

- Vamos a usar el  repositorio: [laravel19](https://bitbucket.org/rafacabeza/laravel19)
- Recomendación: hacer un fork del mismo. De esta manera siempor podrás hacerte con el código de la clase:
    - Ve a bitbucket y realiza un fork
    - Clona dentro de `entornods/data` tu repositorio
    - Obten una copia del fichero `.env`
    - Ejecuta (recuerda que laraa es un alias): 
        ```
        laraa key:generate
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


- https://laravel.com/docs/6.x/routing

* En nuestro framework mvc "casero", un controlador era responsable de una tarea concreta, o conjunto de tareas relacionadas entre sí \(CRUD sobre una tabla, por ejemplo\).

* Para nosotros el _path_ o ruta de nuestra url representaba /controlador/metodo/arg1/arg2/argN. 

* Laravel tiene un sistema de enrutado mucho más flexible y potente. 


* Las rutas se definien en el directorio `routes`:
    - `web.php` define todas las rutas de la aplicación web
    - `api.php` define las rutas asociadas a un servicio web RESTfull
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


### Rutas resource: REST

- Un CRUD necesita definir 7 rutas para 7 acciones.
- Si definimos rutas de tipo resource se mapearán estas 7 rutas.
- En la medida de lo posible estas rutas siguen el paradigma REST.
- Son las acciones necesarias para un CRUD completo y usando los distintos verbos HTTP: get, post, put y delete.
- Para ver como gestionar estas rutas y este tipo de controladores: https://laravel.com/docs/6.x/controllers#resource-controllers


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


- No siempre nos vale el uso de rutas resource.
- Si queremos definir una ruta y un método la sintaxis es:

```php
Route::get('foo', 'Photos\AdminController@method');
```

- Para otras cuestiones leer la documentación: 
    - Uso de middlewares
    - Definir rutas resource con except/only methods
    - Asignar nombres a rutas
    - ...



## MVC
- Laravel es mucho más que una arquitectura MVC
- No obstante esta es una parte fundamental del framework


### Controladores

- Su ubican en _app/Http/Controllers_ (Podríamos usar subdirectorios)
- Para crear un controlador usaremos:
```
php artisan make:controller ControllerName
```
    - Deberemos definir los métodos que necesitemos y asociarlos a una ruta del fichero de rutas (web.php).


- Si queremos usar rutas de tipo resource:
```
php artisan make:controller ControllerName --resource
```
    - Ya nos vendrá con los 7 métodos asociados a una ruta resource

* OJO! Los formularios HTML no pueden usar PUT, PATCH ni DELETE. Podemos usar campos ocultos para falsear los vervos. El metodo helper `method_field` lo puede hacer por nosotros:

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



## Migraciones y Seeders



## Test



## Request y formularios



## Modelos y BBDD



    ## Eloquent & Query Builder    
    


    ## Relaciones
    


## Errores



## Multi Idioma



## Ajax



## Sesiones y Autenticación



## Middleware



## Autorización



## Response



## Paginación



## Mail



## Pdf



## Trabajo


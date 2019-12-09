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
    - Colocamos nuestro Laravel limpio en `data/laravel`
    - ¿Cómo?


- Clonamos el código desde https://github.com/laravel/laravel.git o desde nuestro repositorio. 
- Lo hacemos en el directorio `entornods/data/laravel`

```
cd entornods/data
git clone <identificador ssh/https del repositorio de partida>
cd laravel
composer install
```

- El directorio de partida puede ser `git@github.com:laravel/laravel.git` o el de nuestro proyecto particular


- Si no tenemos instalado php 7.2 ni composer podemos usar docker:

    ```
    cd entornods 
    docker-compose up -d
    ```

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
- El directorio de configuración es _config_. Buena parte de la configuración es condicionada al contenido del fichero `.env`



## Arquitectura



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


### Controladores


### Vistas: Blade


### Modelo



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



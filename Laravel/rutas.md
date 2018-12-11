## Rutas

- https://laravel.com/docs/5.3/routing
- https://laravel.com/docs/5.3/controllers#resource-controllers
* En nuestro framewor "casero", un controlador era responsable de una tarea concreta, o conjunto de tareas relacionadas entre sí \(CRUD sobre una tabla, por ejemplo\).

* Para nosotros el _path_ o ruta de nuestra url representaba /controlador/metodo/arg1/arg2/argN. Vamos a ver coomo realizar esto con Laravel.

* En Laravel el archivo /public/index.php es nuestra entrada al frontend controller pero el fichero _.htaccess_ es distinto. La ruta es tomada de $\_SERVER\['REQUEST\_URI'\].

* Vamos a la documentación oficial: [https://laravel.com/docs/5.3/routing](https://laravel.com/docs/5.3/routing)


* Podemos considerar ejemplos como:

```
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
- Podemos revisar la lista de rutas mediante `artisan route:list`

### Rutas resource: REST
- Si definimos rutas de tipo resource se mapearán las 7 acciones básicas asociadas a un recurso según el paradigma REST. 
- Son las acciones necesarias para un CRUD completo y usando los distintos verbos HTTP: get, post, put y delete.
- Para ver como gestionar estas rutas y este tipo de controladores: https://laravel.com/docs/5.3/controllers#resource-controllers
- Definición de una ruta
```
Route::resource('photos', 'PhotoController');
```
- Creación de un controlador resource:
```
php artisan make:controller PhotoController --resource
```

- Mapeo de rutas y métodos:
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

## Request y response

- El request es un objeto con toda la información de la petición Http. Es fundamental para recuperar datos enviados por formularios.
- El objeto Re sponse hace referencia a la respuesta http.

- Enlaces:
- http://laraveles.com/docs/5.0/requests
- https://laravel.com/docs/5.3/requests
- https://laravel.com/docs/5.3/responses
- Laravel API: https://laravel.com/api/5.3/Illuminate/Http/Request.html

- Hay dos maneras de acceder al objeto request: inyección de dependencias o clase mediante la clase fachada (_Facade_).
- Medidante "inyeccion de dependencias":
- Debemos añadir el "use Illuminate\Http\Request" en la cabecera.
- Debemos añadir un objeto $request indicando con _type hinting_ la clase Request:
```php
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;

class UserController extends Controller
{
public function update(Request $request, $id)
{
//
}
}
```

- Mediante Facade Request:
```php
$name = Request::input('name');
```


### Información de la URL, IP del cliente,.....
Ejemplos. Para más explicación ir a los enlaces de arriba:
```php
$method = Request::method();

if (Request::isMethod('post'))
{
//comprobación del método
}

if (Request::is('admin/*'))
{
//comprobación de la ruta
}

// Sin la Query String...
$url = $request->url();

// Con la Query String...
$url = $request->fullUrl();
```


### Formularios
- Laravel cuenta con un mecanismo de seguridad para evitar envíos desde formularios que no sean de la propia aplicación
- Es el middleware de protección CSRF "VerifyCsrfToken".
- Dos opciones:
 - O desactivamos el middleware:
 
  En el archivo app/Http/Kernel.php encontrarás el "global HTTP middleware stack". Comenta la línea de VerifyCsrfToken::class.
`//\App\Http\Middleware\VerifyCsrfToken::class,`

 - O incluimos el campo con el _token_ en nuestros formularios. Nosotros haremos esto.

```php
<form method="POST" action="/profile">
    {{ csrf_field() }}
    ...
</form>
```

### Parámetros enviados por get o post.
```php
$name = $request->input('name'); //simple
$name = $request->input('name', 'Sally'); //con default
$name = $request->input('products.0.name'); //accediendo a arrays de controles con puntos

$input = $request->all(); //todos
$input = $request->only(['username', 'password']); //sólo...
$input = $request->only('username', 'password');//sólo...
$input = $request->except(['credit_card']);//todos salvo...
$input = $request->except('credit_card');//todos salvo...
```


### Parámetros flasheados para validación de formularios

(lo veremos más adelante).


### Cookies
```php
//recuperar cookie del request
$value = $request->cookie('name');
//enviar cookie al response
return response('Hello World')->cookie(
'name', 'value', $minutes
);
```
- Otra posibilidad es usar la clase fachada Cookie y del helper cookie().
 - Declaramos en la cabecera que vamos a usar la clase:
 
 ```php 
 use Cookie;
 ```
 - Ponemos en cola cookies para enviarlas tan pronto podamos:
```php
//uso básico, si no ponemos minutos --> tiempo máximo, 5 años.
$micookie = cookie('nombre', 'valor', 'minutos')
Cookie::queue($micookie);
```
 - Las leemos 
 ```php
 Cookie::has('nombre'); //comprueba la existencia
 $miCookie = Cookie::get('nombre'); // leemos la cookie
 $miCookie = Cookie::get('nombre', 'default'); //si no existe carga el valor default.
 ```
### Subida de ficheros.

(pendiente)
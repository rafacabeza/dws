## Middleware

- Los middleware son filtros que se ejecutan antes de que el control pase a las rutas.

- Podemos asciar un middleware a una ruta:
```php
Route::get('profile', 'UserController@show')->middleware('auth');
```
- O a un controlador, en su constructor:
```php
class UserController extends Controller
{
    public function __construct()
    {
        $this->middleware('auth'); //aplicable a todos los métodos
        $this->middleware('log')->only('index');//solamente a ....
        $this->middleware('subscribed')->except('store');//todos excepto ...
    }
}
```
### kernel.php

- En el kernel.php de la aplicación se definen varias cuestiones relativas a los middleware.
- Un grupo de middleware que se aplican a todas las peticiones. Atributo $middleware.
- Se asignan nombres a los middleware para ser usados en las rutas. Atributo $routeMiddleware.
- Dos grupos de middleware:
    - _web_ que se aplica a todas las rutas definidas en el fichero de rutas web.php
    - _api_ que se aplica a las rutas del fichero api.php

### Ejemplos de middleware

- _CheckForMaintenanceMode_ Se aplica a todas las rutas. Se activa su funcionalidad con los comandos:
```
php artisan down
php artisan up
```
 - Después de down genera un error 503
 - Comprueba la existencia del fichero /storage/framework/down
 - Down crea el fichero, up lo borra.
 
 -Vamos a ver los que se aplican a todas las peticiones web.
 - EncryptCookies. Encripta las cookies
 - AddQueuedCookiesToResponse. Añade las cookies creadas mediante el método `Cookie::queue($micookie)`;
 - StartSession. Inicia sesión de forma automática.
 - ShareErrorsFromSession Hace visible el objeto errors tras la validación.
 - VerifyCsrfToken. Verifica los token CSRF.
 - SubstituteBindings ¿?
 
- Además, dispondemos de los siguientes definidos en route:
 - auth. Pasa el filtro si estás autenticado.
 - guest. Pasa el filtro si no has hecho login
 - can. Filtro usado para autorización.
 - throttle. Limita el número de peticiones por minuto aceptadas desde un host determinado.
 
### Construir un middleware:
 - Usamos artisasn:
 ``` 
 php artisan make:controller MiddlewareEsAdmin
 ```
 - Código. 
  - La lógica está en la función handle.
  - Si se dan las condiciones adecuadas se pasa el filtro (`return $next($request);`
  - Si no se dan las condiciones abortar o redirigir.


 ```php
 <?php

namespace App\Http\Middleware;

use Closure;

class IsAdmin
{
    public function handle($request, Closure $next)
    {
        $user = $request->user();
        if ($user && $user->username == 'admin') {
            return $next($request);
        }
        abort(403, 'acceso denegado');
    }
}
```

- Registrar el middleware en el kernel dentro de $routeMiddleware
  
- Ya podemos usarlo como el resto:

```php
Route::get('families/ajax', 'FamilyController@ajax')->middleware('admin');
```
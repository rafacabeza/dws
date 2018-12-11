## Sesiones

* Los parámetros de configuración de sesiones se definen en `config/session.php`. Importante es el almacenamiento y el tiempo de vida de la sesión.
* Con Laravel no es preciso usar directamente la variable $_SESSION, ni debemos usar las funciones \_session\_start\(\)_ o _session\_destroy\(\)_

* Podemos acceder a un dato guardado en sesión de tres modos:

  * El request, esto requiere inyección de dependencias:
    ```php
    public function showProfile(Request $request, $id)
    {
      $value = $request->session()->get('key');
    }
    ```
  * La fachada Session
    ```php
    $value = Session::get('key');
    ```
  * La función helper session\(\):
    ```php
    $value = session('key');
    ```

* Obtener todos los datos de una sesión
  ```php
  $data = $request->session()->all();
  $data = Session::all(); //usando el Facade
  ```
* Podemos guardar datos en sesión de dos modos:

  ```php
  $request->session()->put('email', 'me@example.com');
  session(['email' => 'me@example.com']); //usando el helper
  ```

* Podemos eliminar datos de sesión:

  ```php
  $request->session()->forget('key'); //elemento key
  $request->session()->flush();   // todos
  Session::flush();               //todos con la fachada
  ```

* Guardar datos sólo durante una petición \(request\) sólamente. Esto se usa en validaciones para recordar los datos enviados tras un fallo de validación:
  ```php
  $request->session()->flash('mensaje', '¡El usuario fue eliminado!');   //después de una petición se elimina. $request->session()->reflash();  //guardar los datos flasheados un request más.
  $request->session()->keep(['mensaje', 'email']); //guardadr datos flasheados.
  ```

## Autenticación

* Documentación oficial: [https://laravel.com/docs/5.3/authentication](https://laravel.com/docs/5.3/authentication)
* Autenticarse es identificarse, que la apliación nos reconozca como un usuario concreto.
* Para hacerlo debemos hacer uso de variables de sesión.
* Laravel viene con un sistema base muy completo. Para instalarlo:
  ```
  php artisan make:auth
  ```
* Las vistas generadas están construidas con Twiter Bootstrap y se almacenan en el directorio: resources/views/auth.
* Rutas:
  * Para login se usa `/login`
  * Tras el login se usa `/home`. Esto puede personalizarse en el LoginController con la variable $redirectTo o creando un método con el mismo nombre si queremos añadir cierta lógica a la redirección.
  * Por defecto el nombre de usuario es su e-mail.
  * Para cerrar la sesión: `logout`
  * Para resetear la contraseña: `password/reset`
* Rutas auth. Vienen definidas en el fichero Router.php
  \(vendor/laravel/framework/src//Illuminate/Routing/Router.php\)
* Podemos consultar las rutas con artisan:

* Para el reseteo de contraseñas se envia un correo con un token.

* Para pasar los mails a log y no enviar realmente:

  * En el fichero `.env` cambiar el parámetro: \`MAIL\_DRIVER=log
    \`
  * Completar en config/mail.php:
    ```php
    'from' => [
      'address' => env('MAIL_FROM_ADDRESS', 'admin@example.com'),
      'name' => env('MAIL_FROM_NAME', 'Adminstrador'),
    ],
    ```
  * Los correos se guardan en storage/logs
  * Podemos ver el correo de resetear contraseña "enviado"

* Si queremos acceder a la información del usuario actual debemos usar la clase fachada Auth.

```php
//Mostrar la información de usuario
Route::get('usuario', function () {
    $user = Auth::user();
    dd($user);
});
```




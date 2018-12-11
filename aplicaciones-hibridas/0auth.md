## Aplicaciones web híbridas

- Con este término nos referimos a aplicaciones web que requieren de la aportación de otras aplicaciones para su funcionamiento a través de API's de terceros.

- Ante esta posibilidad caben dos alternativas:
  - Usar una API o conjunto de librerías creadas para un lenguaje determinado (php, java, .NET)
 - Podemos usar un servicio o API web basado en un estándar como json o xml.
 
### 0Auth

- Segun el estándar https://tools.ietf.org/html/rfc6749, 0Auth permite el acceso a aplicaciones de terceros a servicios HTTP, servicios web, o para que una aplicación acceda a un servicio web en representación del propietario del recurso.

- Montar un API que gestione su acceso mediante 0Auth sería muy interesante pero por cuestiones de tiempo no ha sido posible en este curso.

- Lo que sí podemos hacer es hacer uso de 0Auth como clientes. Un uso muy habitual es hacerlo para obtener las credenciales de un usuario ofrecidas por servicios de autenticación de terceros: Google, GitHub, Facebook, Twitter, ...

#### Login a través de Google.
- Vamos a permitir el acceso a nuestra aplicación de forma que sea Google quien certifique quien es el usuario que está solicitando acceso a nuestra aplicación.
- Vamos a hacerlo a partir de un componente de Laravel: el paquete Socialite. Veamos como hacerlo:


1. Actualizar laravel. Ver manual. https://laravel.com/docs/5.4/upgrade

2. Obtener credenciales google
 - https://developers.google.com/identity/sign-in/web/sign-in
 - Registrarlas en config/services.php
 - Activar la Google+ API en https://console.developers.google.com/apis/library
 
3. Añadir el paquete _Socialite_ a Laravel: https://github.com/laravel/socialite
4. Crear el  controlador SocialAuthController con artisan y añadir estos métodos

```php
class SocialAuthController extends Controller
{
    public function redirect()
    {
        return Socialite::driver('google')->redirect();
    }

    public function callback()
    {
        try {
            $user = Socialite::driver('google')->user();
        } catch (\Exception $e) {
            return redirect('/login')->with('status', 'Something went wrong or You have rejected the app!');
        }

        $authUser = $this->findOrCreateUser($user);
        Auth::login($authUser);
        return redirect('/home');    
    }
    
    
    // aquí nos faltaría el método findOrCreateUser
}
```
- El método redirect va asociado a una ruta.
- El método callback es invocado desde la ruta de _callback_ registrada en Google
- El fichero de rutas podría ser algo así:

```php 
Route::get('/redirect/google', 'SocialAuthController@redirect');
Route::get('/callback/google', 'SocialAuthController@callback');

```
- Para completar el código del anterior controlador:
```php
    public function findOrCreateUser($user)
    {
        $authUser = User::where('email', $user->email)->first();

        if ($authUser) {
            return $authUser;
        }

        return User::create([
            'name' => $user->name,
            'email' => $user->email,
            'surname' => $user->user['name']['familyName'],
            'password' => 'empty',
            // 'google_id' => $user->id,
            // 'avatar' => $user->avatar
            // Cada proveedor da unos datos, facebook por ejemplo no provee email
        ]);
        // recordatorio: create equivale a new() + asignar campos + save()
    }
```

<!--
https://blog.damirmiladinov.com/laravel/laravel-5.2-socialite-facebook-login.html#.WLpmon-YLQ0

https://blog.damirmiladinov.com/laravel/laravel-5.2-socialite-google-login.html#.WLpiwX-YLQ0

https://www.ladrupalera.com/drupal/desarrollo/javascript/como-usar-una-api-de-google-con-autenticacion-traves-de-oauth2

https://github.com/greggilbert/recaptcha

https://styde.net/como-integrar-google-recaptcha-en-formularios-de-login-y-registro-de-laravel-5-2/
-->

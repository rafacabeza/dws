## Autorización: reglas y políticas
- Laravel nos brinda dos sistemas para autorizar acciones: reglas y puertas.
- Reglas
 - Las reglas se definen asociadas a la clase _Gate_.
 - Son una solución más simple.
 - Se definen normalmente usando _closures_.
- Políticas
 - Son soluciones más elaboradas que engloban la autorización referida a un modelo concreto.
 - Se definen en clases dentro guardadas en _app/Policies_
- Tienen una analogía a rutas y controladores, simpliciad vs complejidad y orden.
- En un proyecteo usaremos una u otra (o ambas) de acuerdo a su complejidad y envergadura.

### Definir reglas

- Las reglas se definen en el AuthServiceProvider.php, dentro de su función boot.
- Podemos definir una regla con un closure (función anónima) dentro de dicho método:
```php
public function boot()
{
    $this->registerPolicies(); 
    //nuestra regla es esta:
    Gate::define('update-post', function ($user, $post) {
        return $user->id == $post->user_id;
    });
}
```
### Usar reglas
- Para comprobar las reglas debemos usar la fachada Gate y alguno de sus métodos (allows, denies). Lo podemos hacer en la propia ruta o más correctamente en el controlador o en un middleware:
```php
if (Gate::denies('update-post', $post)) {
    abort(403);
    //o podríamos redireccionar:
    // return redirect('/home');
    // o mostrar una vista
    // return view('nombreVista');
}
```
- Observa que el $user no es pasado a la clase Gate, toma el autenticado. Si quisieramos pasarlo debemos hacerlo así:  
```php
//preguntando si se le permite
if (Gate::forUser($user)->allows('update-post', $post)) {
    // The user can update the post...
}
//o al revés ...
if (Gate::forUser($user)->denies('update-post', $post)) {
    // The user can't update the post...
}
```
### Políticas
- Si el proyecto es pequeño la solución anterior es válida pero para algo grande puede no ser una buena solución.
- Las políticas separan código para no sobrecargar el código del AuthServiceProvider.
- Las políticas son clases guardadas en app/Policies y que se crean así:
```php
//clase en blanco
php artisan make:policy PostPolicy
//clase con los cuatro métodos CRUD definidos
php artisan make:policy PostPolicy --model=Post
```

- Tras crear las políticas, éstas deben ser regitradas en el AuthServiceProvider:
```php
class AuthServiceProvider extends ServiceProvider
{
    //deben incluírsee en el siguiiente array:
    protected $policies = [
        Post::class => PostPolicy::class,
    ];
```

### Escribiendo las políticas:
- Podemos crear los métodos que necesitemos. Por ejemplo el método update lo podemos usar para actualizar un objeto del modelo afectado. Sus argumentos deben ser el user y un objeto del modelo protegido:
```php
public function update(User $user, Post $post)
{
    return $user->id === $post->user_id;
}
//aunque si el método no usa ningún objeto del modelo lo podemos dejar así:
public function create(User $user)
{
    return true; //una expresión booleana...
}
```
- El método before permite tomar decisiones generales sin evaluar la política

```php
public function before($user, $ability)
{
    if ($user->isSuperAdmin()) {
        return true;
    }
}
// si devuelve true -> autorizado. No comprueba nada más
// si devuelve false -> denegado. No comprueba nada más
// si devuelve null -> lo que diga la política
```

<!--
4 videos de como programar los permisos dinámicamente, registrando permisos y su asignación en bbdd
https://youtu.be/-go8BMcpCf4?list=PLdKRooEQxDnW6OsnV4HQuVo-SX8fCgD1s
-->
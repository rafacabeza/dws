## Vistas en Laravel: blade.

* Documentación oficial y enlaces:
  * [http://laraveles.com/docs/5.1/views](http://laraveles.com/docs/5.1/views)
  * [https://laravel.com/docs/5.3/views](https://laravel.com/docs/5.3/views)
* Tutoriales
  * [https://styde.net/tutorial-basico-de-blade-el-sistema-de-plantillas-de-laravel/](https://styde.net/tutorial-basico-de-blade-el-sistema-de-plantillas-de-laravel/)
  * [http://www.desarrolloweb.com/articulos/introduccion-vistas-laravel5.html](http://www.desarrolloweb.com/articulos/introduccion-vistas-laravel5.html)

### Vistas

* Las vistas en php se pueden construir mezclando php y html.
* Ese código híbrido puede resultar muy espeso y difícil de entender.
* Por ese motivo se ha extendido el uso de motores de plantillas que facilitan la inclusión de contenido variable en nuestro html.
* Motores de plantillas son:
  * Blade, usado por Laravel
  * Smarty, muy habitual, no ceñido a ningún framework
  * Twig, usado por Simphony.
* Los motores de plantillas usan un código más limpio. Dicho código se compila de forma transparente al usuario y se genera php como estamos acostumbrados.

### Mostrar una vista.

* Las vistas en Laravel pueden usar php o el motor blade. El nombre del fichero nos indica cómo hemos construido una vista. Hemos de respetar esta convención para que blade funcione.
  ```php
  nombrevista.php
  nombrevista.blade.php
  ```
* Desde nuestro fichero de rutas podemos llamar a una vista. Veremos que también lo podemos hacer desde los controladores.
* Las vistas se guardan en _/resources/views_

```php
Route::get('user', function()
{
    //mostraremos user.php o user.blade.php
    return view('user');
});
```

* Es conveniente usar subdirectorios. No obstante al llamarlas podemos usar barras o puntos para referirnos a ellas.
* La vista guardada en _user/index.blade.php_ se puedBe llamar indistintamente así:
  ```php
  Route::get('user', function()
  {
    return view('user/index');
    return view('user.index');
  });
  ```

### Pasar información

* Lo habitual es pasar información a la plantilla para que se genere una vista realmente dinámica. Para hacerlo usando un array asociativo \(clave-valor\).
  * Ejemplo con un dato

```php
Route::get('user', function()
{
    return view('user.index', ['name' => 'James']);
});
```

* O así si hay más de uno.

```php
view('blog.history', [
        'mes' => $mes,
        'ano' => $ano,
        'posts' => $posts
]);
```

* O también con with:

```php
return view('greeting')->with('name', 'Victoria');
```

* Y por último usando compact:

```php
return view('users', compact('users', 'title'));
```

Sitáxis de blade

---

**NOTA IMPORTANTE**  
Los elementos aquí mostrados entre llaves deben ir con dobles llaves.  
Existe un problema de esta sintáxis con GitBook, motivo por el que se muestran de esta forma imprecisa

---

* Haciendo _echo_:
  ```php
  //modo php
  <?php echo $variable ?>
  //modo blade
  { $variable }
  ```
* Los textos son "escapados siempre". Es una medida de seguridad importante cuando tomamos datos de formularios de usuario.

  * Para no hacerlo debemos usar '{!! $var !!}'

    ```php
    Route::get('escape', function () {
    return view('user.escape', [
        'text' => '<h1>hola mundo</h1>'
        ]);
    });
    ```

  * Crea la siguinte vista con nombre _escape.blade.php_:

    ```php
    <!DOCTYPE html>
    <html>
    <head>
    <title>User</title>
    </head>
    <body>
    Texto "escapado"{ $text }
    <br>
    Texto sin escapar: {!! $text !!}
    </body>
    </html>
    ```

* Podemos usar el operador ternario:

  ```php
  //al estilo php
  { isset($name) ? $name : 'Default' }
  //o al modo blade
  { $name or 'Default' }
  ```

* Condicionales:

```php
@if (count($records) === 1)
    I have one record!
@elseif (count($records) > 1)
    I have multiple records!
@else
    I don't have any records!
@endif

//además contamos con un si negativo (if(!condicion){})
@unless (Auth::check())
    You are not signed in.
@endunless
```

* Bucles

```php
@for ($i = 0; $i < 10; $i++)
    The current value is { $i }
@endfor

@foreach ($users as $user)
    <p>This is user { $user->id }</p>
@endforeach

@forelse ($users as $user)
    <li>{ $user->name }</li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile
```

* Más posibilidades en: [https://laravel.com/docs/5.3/blade\#control-structures](https://laravel.com/docs/5.3/blade#control-structures)

### Layouts

* Resulta interesante poder unificar la construcción de nuestras vistas.
* Podemos usar la directiva `include` para incluir elementos construidos en un fichero a parte.

```php
@include('footer')
//así incluiremos un elemento guardado en views directamente y con extensión php o blade.php
```

* Pero lo realmente práctico es usar _layouts_. Un layout es una plantilla con la distribución base para otras plantillas. La ubicación está en `/resources/views/layouts`  

* Un fichero de layout es un fichero blade normal con la salvedad de usar algunas directivas que etiquetan la ubicación de los elementos a construir en cada una de las vistas que usen ese layout.
* Los elementos variables de nuestras vistas se definene con las directivas `@section`  y sus posibles cierres \(`@endsection|stop|show|append` \) y `@yield` 
  * Section define una sección.
  * Si se cierra con @endsection o @stop \(equivalentes\) se define la sección pero hay que usar @yield después para definir su ubicación en el layout.
  * Si usamos @yield ubicamos una sección si ya estaba definida. Si la sección no existe se crea y se ubica.
  * La diferencia principal entre @section y @yield es que la primera permite añadir contenido a un asección y la segunda no.
  * Si @section se cierra con @show se define además el punto de inserción 

```php
<!-- Stored in resources/views/layouts/app.blade.php -->

<html>
    <head>
        <title>App Name - @yield('title')</title>
    </head>
    <body>
        @section('sidebar')
            This is the master sidebar.
        @show

        <div class="container">
            @yield('content')
        </div>
    </body>
</html>
```

* Sería equivalente lo siguiente \(cambio de show por endsection+yield:

```php
<!-- Stored in resources/views/layouts/app.blade.php -->

<html>
    <head>
        <title>App Name - @yield('title')</title>
    </head>
    <body>
        @section('sidebar')
            This is the master sidebar.
        @endsection  <!-- equivale a @stop -->
        @yield('sidebar')

        <div class="container">
            @yield('content')
        </div>
    </body>
</html>
```

* Ahora una vista puede extender un layout de la siguiente manera

```php
<!-- Stored in resources/views/child.blade.php -->

@extends('layouts.app')

@section('title', 'Page Title')

@section('sidebar')
    @parent

    <p>This is appended to the master sidebar.</p>
@endsection

@section('content')
    <p>This is my body content.</p>
@endsection
```

* NOTA: el uso de `@parent`  permite mantener el contenido de una sección en el layout padre y ampliarlo. Si no se usa el contenido de la sección en el padre desaparece.  




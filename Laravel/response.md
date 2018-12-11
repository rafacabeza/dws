### Response

* Podemos reenviar la respuesta de diferentes modos:
* Reenvio a una ruta concreta: 
* Reenvío a rutas con nombre:

```php
//fichero de rutas:
//ruta destino con nombre definido
Route::get('destino/{sitio}', function ($sitio) {
    return "Destino: $sitio";
})->name('destino');
```



```php
//código en el una fichero de rutas pero que podría ir en el controlador:
//reenvío ordinario por la URL
Route::get('teruel', function () {
    return redirect('/destino/Teruel');
});
//reenvío por nombre
Route::get('zaragoza', function () {
    return redirect()->route('destino', ['sitio' => 'Zaragoza']);
});
```

* Reenvío a la misma ruta de la que venimos, e incluso añadir mensajes de error:

```php
//redirección simple:
return Redirect::back();

//igual pero con errores
return Redirect::back()->withErrors(['msg', 'The Message']);

//en la vista podríamos hacer:

@if($errors->any())
<h4>{{$errors->first()}}</h4>
@endif
```

* Reenviar a un método de un controlador:
  ```php
  //a un método sin argumentos
  return redirect()->action('HomeController@index');
  //con argumentos
  return redirect()->action(
    'UserController@profile', ['id' => 1]
  );
  ```




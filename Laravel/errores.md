## Errores

* Podemos generar códigos de error durante el ciclo de vida de un request con `abort()`

```php
abort(404);  //salida normal
abort(403, 'No autorizado.'); //salida con mensaje
```

### 404

* Existen métodos asociados a modelos Eloquent que generan automáticamente excepciones 404.
  * Model::findOrFail: `User::findOrFail(999)`
  * Model::firstOrFail:  `User::where('email', 'rafa@dw.es')->firstOrFail();`



## Vistas de Error

* Si creamos vistas de error dentro de  views/errors, \('views/errors/404.blade.php' o 'views/errors/500.blade.php'\) estas vistas se generarán automáticamente si se producen excepciones de este tipo.
* Estas vistas también se pueden devolver directamente:
  * Los argumentos de view son, por este orden: plantilla de vista, datos, código de status

```php
$response()->view('errors.404', [], 404)
```






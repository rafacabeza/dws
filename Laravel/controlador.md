## Controladores

* Enlaces:

  * [http://laraveles.com/docs/5.0/controllers](http://laraveles.com/docs/5.0/controllers)
  * [https://laravel.com/docs/5.3/controllers](https://laravel.com/docs/5.3/controllers)

* Ya sabemos el papel que desempeñan los controladores. Veamos qué tienen de particular en Laravel.

* Ubicaión 'app/Http/conttollers'.

* Creación de un controlador con artisan. A continuación vemos el comando base y las opciones de controlador "resource" y de controlador plano o vacío.

```
php artisan make:controller NombreController
php artisan make:controller NombreController --resource
```

* Si usamos el método _plain_ el controlador no tendrá ningún método. Cada método público que necesitemos deberá se asociado a una ruta del fichero de rutas.

* Si usamos el métood --resource podemos usar una única ruta para un controlador de tipo recursos RESTful.

  * [https://laravel.com/docs/5.3/controllers\#resource-controllers](https://laravel.com/docs/5.3/controllers#resource-controllers)
  * Muy útil para servicios web de tipo RESTful pero utilizable para aplicaciones web convencionales.

* Los formularios HTML no pueden usar PUT, PATCH ni DELETE. Podemos usar campos ocultos para falsear los vervos. El metodo helper `method_field` lo puede hacer por nosotros:

```php
{{ method_field('PUT') }}
{{ method_field('DELETE') }}
```




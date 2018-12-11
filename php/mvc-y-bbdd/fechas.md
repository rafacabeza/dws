# Fechas

* En PHP podemos manejar datos de fecha de diferentes modos. Al menos los siguientes:

* Fecha como un entero

  * En este caso una fecha se guarda como un entero que mide la fecha Unix, es decir, número de segundos transcurridos desde el inicio de 1970.
  * Podemos convertir el texto leído de la base de datos \(formato "año-mes-dia"\) en cualquier momento pero lo más adecuado es hacerlo en el constructor del modelo:

```php
class User extends Model
{

    function __construct()
    {
        $this->birthdate = strtotime($this->birthdate);
    }
```

* Fecha como objeto de la clase DateTime
  * Se trata de una clse predefinida en php y que nos facilita las operaciones realcionadas con fechas
  * Igualmente lo mejor es convertir el texto recibido de la base de datos en el constructor:

```php
class User extends Model
{

    function __construct()
    {
        $this->birthdate = new \DateTime($this->birthdate);
    }
```

* Para formatear esta fecha debemos usar la opción adecuada, según lo que hayamos optado previamente, fechas de forma simple o con la clase DateTime:

```php
//forma simple
<?php echo date('d/m/Y', $user->birthdate) ?>
//con clase DateTime
<?php echo $user->birthdate->format('d/m/Y') ?>
```




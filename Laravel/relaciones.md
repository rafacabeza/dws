###  Relaciones
#### Claves ajenas



#### Relaciones 1:1
- En todas las relaciones debemos crear las claves ajenas a nivel de base de datos. Necesitamos hacerlo en las migraciones.
- Migraciones: comando `foreign()...`. Ejemplo
```php
Schema::create('profiles', function (Blueprint $table) {
    $table->integer('user_id')->unsigned();
    $table->primary('user_id');
    $table->text('bio');
    $table->string('company', 150);
    $table->string('technologies', 200);
    $table->timestamps();
    $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
});
```



#### Relaciones 1:1
- En el modelo _User_ podemos acceder directamente al modelo relacionado mediante _hasOne()_
```php
public function profile() {
    return $this->hasOne('App\Profile');
}
```

- Desde _Profile_ podemos acceder al modelo _User_ mediante _belongsTo()_
```php
public function user()
{
    return $this->belongsTo('App\User');
}
```

- Acceso a los datos

#### Relaciones 1:N

#### Relaciones N:M


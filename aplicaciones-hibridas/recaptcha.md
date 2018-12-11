## ReCAPTCHA


- Usaremos el paquete https://github.com/greggilbert/recaptcha el cual funciona en todas las veriones de Laravel.
- Registramos nuestro sitio para usar reCAPTCHA en https://www.google.com/recaptcha/admin
- Instalamos el paquete con composer:

`
"greggilbert/recaptcha": "dev-master"
`

- Accede al enlace de arriba y sigue las intrucciones de GitHub:
 - Registra el paquete en config/app.php
 - Registra el alias

 
- Ejecuta `php artisan vendor:publish --provider="Greggilbert\Recaptcha\RecaptchaServiceProvider".`

- En `/config/recaptcha.php` coloca:
 - Tus claves pública y privada.
 - En options, añade `'lang' => 'es',`-

- En el fichero `resources/lang/[lang]/validation.php` añade el mensaje de error: `    'recaptcha' => 'El campo :attribute no es correcto.',`

- Añade el reCAPTCHA  a tu vista: `{!! Recaptcha::render() !!}`
- Añade la validación a tu controlador:
```php
'g-recaptcha-response' => 'required|recaptcha',
```

- Podemos añadir mensajes de error asociado a este campo especial.
- Ya podemos usar reCAPTCHA en nuestro formulario.


- Enlaces de ayuda:

 - https://github.com/greggilbert/recaptcha
 - https://styde.net/como-integrar-google-recaptcha-en-formularios-de-login-y-registro-de-laravel-5-2/
 - https://www.google.com/recaptcha/admin#site/337097093?setup
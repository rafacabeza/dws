## Instalación de Laravel

* Documentación: [https://laravel.com/docs/5.3](https://laravel.com/docs/5.3)
* Los requerimientos deben consultarse con _phpinfo\(\)_. Por si lo has olvidado, edita un fichero en tu servidor con lo siguiente y consúltalo.
  ```php
  <?php
  phpinfo();
  ```

### Composer

* La instalación de composer puede evolucionar por lo que mejor guiarnos por la web oficial. 
* No obstante esta web puede ser interesante, especialmente su vídeo.
    [https://styde.net/aprende-laravel-5-instalacion-y-uso-de-composer/](https://styde.net/aprende-laravel-5-instalacion-y-uso-de-composer/)
* Para la instalación de Laravel se recomienda acceder a: 
    [https://laravel.com/docs/5.1](https://laravel.com/docs/5.1)
* Una vez instalado composer:

```
# necesitamos soporte zip para php
sudo apt-get install php7.0-zip 
#necesitamos composer
sudo apt-get install composer

#Ya podemos instalar usando composer:
composer create-project --prefer-dist laravel/laravel proyectoblog
#o una vessión concreta
composer create-project laravel/laravel proyctoblog "5.6.*" 


# Otra posibilidad es descargar laravel para usarlo directamente como comando
# (Yo me quedo con la solución de arriba)
# descargamos laravel con composer      
composer global require "laravel/installer=~1.1"
# añadimos la ruta .composer/vendor/bin al PATH

export PATH="~/.config/composer/vendor/bin:$PATH"
# ya podemos crear nuevos proyectos laravel:
laravel new project-name
```

* Una vez instalado tenemos que hacer dos cosas:
  1. Copiar \(o renombrar\) el fichero de entorno: `cp .env.example .env`
  2. Crear una clave de instalación: `php artisan key:generate`

#### 

#### Laravel y git

* Laravel ya viene con el fichero .gitignore preparado:
  * No debe sincronizarse el fichero .env ni el directorio vendor entre otros.
* Una vez instalado un proyecto laravel. ¿Cómo lo subimos a github o bitbucket?
  * Iniciar repositorio: `git init`
  * Añadir todos los ficheros a preparados: `git add .` o `git add -A`
  *  Ralizar commit `git commit -m "mensaje"`
  * Subir el proyecto: `git push origin master`
* Clonar proyecto git:
  * Clonar como cualquier proyecto: `git clone ...`
  * Descargamos todas las dependencias con composer:
    ```
    cd proyectoLaravel
    composer install
    ```
  * OJO: 
    * composer.json y composer.lock se suben
    * `composer install`instala las dependencias exactamente como están en `composer.lock`
    * `composer update`  se fija en el contenido del`composer.json`fdf y si hay versiones actualizadas las renueva en el `.lock`
    * Por último. Al cambiar de rama o actialziar código de nuestro repositorio es preciso actualizar el mapa de clases por lo que hay que ejeutar `composer  dump-autoload`

  * No hay fichero .env pero sí uno de muestra, así que copiamos de él: `cp .env.example .env`
  * Generamos clave aleatoria usada para cuestiones de seguridad: `php artisan key:generate`




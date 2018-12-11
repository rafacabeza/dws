## Configuración

* Tal vez no es lo que más nos interesa ahora pero veamos una pequeña reseña para poder venir a estas líneas cuando nos haga falta.

* En marcha:

  * Simple. Deja una consola bloqueada
    ```
    php artisan serve          #iniciar
    chrl+C                     #cerrar
    ```
  * Sin bloquear consola, algo más incómodo
    ```
    php artisan serve &                 //inicia el servidor, el "&" para liberar la consola

    ps                                  //listado de procesos
    kill NUMERO                         //apaga el servidor.
    ```

* Modo mantenimiento

```
php artisan down                    //modo mantenimiento
php artisan up                      //modo producción
```

* El directorio de configuración es _config_. Las variables de configuración definidas ahí hacen referencia al entrono de producción.
* El fichero .env define variables utilizadas únicamente en el entorno de desarrollo. Este fichero debe excluirse de _git_.




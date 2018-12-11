# CRUD o mantenimiento completo de la tabla _books_.

A continuación se describe paso a paso lo que debemos hacer para completar este ejercicio. 


1. Crea los ficheros y clases necesarios:
    - El modelo, clase Book
    - El controlador, clse BookConroller
    - Un directorio views/book donde guardarás las vistas.
    

2. Listado. Método index al controlador.
    - Añade un `echo` en el método para comprobar que todo va bien.
    - Necesitas cargar todos los registros en un array (p.ej. $books).
    - En el modelo, crea el método de conexión, y su método estático `all()` que debe leer toda la tabla. No olvides añadir la clase Book al controlador con `require_once`.
    - Comprueba que los resultados se reciben en index con un `var_dump`
    - Crea la vista `views/book/index.php`. En ella debes recorrrer todos los registros y construir una tabla.

3. Creación 1. Método `create` para mostrar el formulario.
    - Añade un enlace a la vista `index` que nos lleve a `/book/create`.
    - Prueba con `echo` que el método se ejecuta.
    - Construye una vista con el formulario de alta y haz que se muestre en el método create.
    - Para el formulario, `method="post"` y `action="store"`

4. Creación 2. Método `store` para guardar el contenido del formulario.
    - Crea un objeto de la clase Book.
    - Vuelca en él el contenido del formulario.
    - Crea un método `store` en el modelo que se ocupe de ejecutar el correspondiente INSERT en la base de datos. Usa consulta preparada.
    - Envía un `header('location....')` para que el navegador vuelva al listado.
    
5. Creación del método de borrado, `delete`.
    - Añade un enlace por cada registro del index con la ruta `delete/$id`.
    - Crea el método y comprueba con un echo que se ejecuta y que recibes el $id del elemento a borrar.
    - Crea el método `delete`en el modelo que ejecute el DELETE  sobre la base de datos. Usa consulta preparada.
    - Envía un `header('location....')` para que el navegador vuelva al listado.
6. Edición individual. Método `show`. 
    - Añade un enlacce en la vista de index por cada registro que nos lleve a `show/$id`.
    - Crea el método y comprueba que se ejecuta con echo.
    - Añade un método estático `find($id)` en el modelo. Usa consulta preparada.
    - Crea un objeto $book en el controlador que lea los datos con el citado `find`.
    - Crea la vista `book/views/show.php` y muestra los datos del libro.
    - Añade a la vista enlaces para: volver al listado, editar y borrar el registro.

7. Edición  1. Método `edit` para mostrar un formulario precargado con los datos del registro.
    - Añade un enlacce en la vista de index por cada registro que nos lleve a `edit/$id`.
    - Crea el método y comprueba que se ejecuta desde los enlaces creados. Usa echo.
    - Crea un objeto $book en el controlador que lea los datos con el citado `find`.
    - Crea la vista `book/views/edit.php` y muestra los datos del libro dentro de un formulario. Cada campo debe alojarse en un input.
    - Recuerda: `method="post" action="../update/$id". 
8. Edición 2. Método `update` para guardar el formulario anterior.
    - Crea el método `update` y usando echo comprueba que se ejecuta al enviar el formulario anterior.
    - Usa el método `find($id)` del modelo para leer el registro que estamos modificando.
    - Crea un nuevo método `save()` en el modelo que ejecute el UPDATE sobre la base de datos. Usa consulta preparada.
    - Ejecuta el citado método `save` y reenvía el navegador al listado.


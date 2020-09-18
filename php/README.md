# PHP


## Entorno de desarrollo.

- Vamos a partir del entorno de desarrollo preparado con docker:

https://github.com/rafacabeza/entornods

- Debemos clonar dicho repositorio en nuestro espacio de trabajo:

```bash
cd ~
git clone git@github.com:rafacabeza/entornods.git
```


- Entorno Docker para el módulo de Desarrollo Web en Entorno Servidor
- Iniciar servicio:
    `docker-compose up -d`
- Parar servicio:
    `docker-compose down`
- Ver máquinas corriendo:
    `docker-compose ps`


- En este entorno se van a usar tres sitios web de prueba. Para poder usarlos debemos *engañar* al DNS.
- Debes editar tu fichero `/etc/hosts` para acceder a los dos sitios web creados por el mismo.

    ```
    127.0.0.1	web1.com
    127.0.0.1	web2.com 
    127.0.0.1	phpmyadmin.docker
    ```

- Prueba a hacer ping a estos sitios. 


- Este entorno creo los siguientes contendedores en su rama master:
    - proxy: Es un proxy nginx para poder mantener simultaneamente múltiples contenedores con diferentes sitios web.
    - db: Contenedor mysql para uso de bases de datos.
    - phpmyadmin: Contenedor para administración web del anterior.
    - web1: sitios web1.com apache y php (Dockerfile web1)
    - web2: sitio web2.com con apache y php (php:7.4-apache)


- Pruebe en su navegador las siguientes url's:

    - http://web1.com/ 
    - http://web1.com/factorial.php?numero=3
    - http://web1.com/factorial.php?numero=10
    - http://web2.com/
    - http://phpmyadmin.docker


- El último sitio nos permite accede a `phpmyadmin`
- Este entorno incorpora un script para inicializar una base de datos en `./data/init-db`
- Conectese a http://phpmyadmin.docker e importe el script citado.
- Pruebe a acceder ahora de nuevo a http://web1.com/ 
- Analiza lo ocurrido y crea otro sitio web en el mismo entorno llamado web3 (web3.com).



## A trabajar!

### Visual Studio Code

- Vamos a añadir algunos complementos:
  - PHP IntelliSense
  - PHP Intelephense
  - PHPml (PHP in Html)
- Atajo JSON: `ctrl+shift+P` + shotcut
  ```json
  {
      "key": "ctrl+shift+d",
      "command": "editor.action.copyLinesDownAction",
      "when": "editorTextFocus"
  }
  ```


### Entorno
- Vamos a cambiar a la rama ejercicios de nuestro "entornods"
- Añadimos una entrada al fichero `/etc/hosts`

  ```bash
  127.0.0.1     ejercicios.local
  ```

- Clonamos el directorio [ejercicios](https://github.com/rafacabeza/ejerciciciosphp)
- Levantamos el servicio: 

  ```bash
  docker-compose up -d
  ```



## Primeros pasos

- Damos por hecho que el alumno tiene una base de programación y de orientación a objetos.
- Vamos a hacer un recorrido rápido sobre las bases de PHP.
- Para completar información respecto a lo que aquí se cuenta el mejor sitio
    es la documentación oficial  <a href="https://www.php.net/manual/es/langref.php"> php.net/manual/es/ </a>


### ¿Qué es PHP?

- PHP (acrónimo recursivo de "PHP: Hypertext Preprocessor") es un lenguaje de código abierto muy popular especialmente adecuado para el desarrollo web y que puede ser incrustado en HTML. 

```php
<!DOCTYPE html>
<html>
    <head>
        <title>Ejemplo</title>
    </head>
    <body>

        <?php
            echo "¡Hola, soy un script de PHP!";
        ?>

    </body>
</html>
```


- PHP está construído a partir de Perl
- Perl está basado en C.
- Las variables son al estilo Perl
- La sintáxis es muy similar a C (y por tanto a Java)


### Sintáxis básica:

- Los ficheros php deben tener extensión ".php"
- El php puede estar embebido en código html
    - Los fragmentos de php deben ir etiquetasdos entre `<?php ?>`
- O pueden ser ficheros código php puro.
    - Sólo debe aparecer al inicio la marca de apertura: `<?php`
    ```php
    echo "Hola mundo!";
    ```


- Las sentencias acaban en ";".
- Los comentarios:

  ```php
  <?php
      echo 'Esto es una prueba'; // Comentario estilo de C de una línea
      // (o C++ o Java o ....)
      /* Esto es un comentario multilínea
      y otra lína de comentarios */
      echo 'Una prueba final'; # Comentario estilo de consola de una línea
      /**
      * Comentario al estilo Phpdoc
      * Como en Javadoc ....
      */
  ?>
  ```


### Tipos

- Los [tipos](https://www.php.net/manual/es/language.types.php) básicos son:
  - Entero: número entero con signo
  - Flotante: número decimal con signo
  - Booleano: vale true o false
  - Cadena de caracteres: cadena de caracteres delimitada por comillas simples o dobles.


- Existen otros más complejos que iremos viendo:
  - Arrays
  - Objetos
  - Recursos
  - ....
- Podemos consultar el tipo de una variable con `gettype()`.


- Y hacer casting para cambiar el tipo: 
  ```php
  $foo = 10;            // $foo es un integer
  $str = "$foo";        // $str es un string
  $fst = (string) $foo; // $fst es tambien un string
  ```

- Casting o forzados permitidos
  - (int), (integer) - forzado a integer
  - (bool), (boolean) - forzado a boolean
  - (float), (double), (real) - forzado a float
  - (string) - forzado a string
  - (array) - forzado a array
  - (object) - forzado a object
  - (unset) - forzado a NULL (PHP 5)


### Variables

- Las variables deben comenzar por dolar: `$miVariable`
- No necesitan ser declaradas, basta inicializarlas.
- No tienen un tipo fijo.
- Deben segir notación de estilo `$camelCase`;
- Podemos consultar si una variable está definida y su tipo de dato:
  ```php
  isset($variable)  //true si existe 
  empty($variable)  //true si no existe o su valor es 0 o ""
  ```


- El ámbito de las variables es:

  - Global si está fuera de una función o una clase
  - Local dentro de funciones

- Podemos definir variables estáticas dentro de las funciones. Su valor no cambia entre invocaciones.
- El ámbito es compartido con ficheros incuídos.


### Constantes

- Las constantes se crear usarndo  `define` o `const`
- El estándar dice que deben ser en mayúsculas:

  ```php
  define("MAXSIZE", 100);

  echo MAXSIZE;
  echo constant("MAXSIZE"); // lo mismo que la línea anterior
  // Funciona a partir de PHP 5.3.0
  const CONSTANTE = 'Hola Mundo';
  echo CONSTANTE;
  ```


### Operadores

- Los [operadores](https://www.php.net/manual/es/language.operators.php) lógicos y matemáticos son casi ideńticos a C o Java.
- Rseñar:
  - Concatenación `.`.
  - Asignación y concatenacion `.=`, similar a `+=`.
- Hay alguna curiosidad en los [los de comparación](https://www.php.net/manual/es/language.operators.comparison.php)


### Estructuras de control

- Las [estructuras de control](https://www.php.net/manual/es/language.control-structures.php) son muy similares a Java
- Podemos empezar a codificar con lo que sabemos.


- Podemos importar el contenido de otros ficheros de código con `include, include_once, require, require_once`
  - Require para la ejecución si no encuentra el fichero.
  - La variante "_once" comprueba que no se ha requerido/incluído el mismo fichero antes. Importante al importar clases.
- La estructura [foreach](https://www.php.net/manual/es/control-structures.foreach.php) permite recorrer arrays (y otros objetos iterables).


## Arrays
### Declarar un array
### Añadir y quitar elementos
### Recorrer un array




## Formularios
### Variables superglobales
### Uso de métodos GET y POST
### Enviar formularios al script de origen
### Usar arrays en formularios
### Usar elementos ocultos en formularios



## Funciones 



## POO



## Namespaces



## Separar vista de lógica
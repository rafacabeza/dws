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

- En Php un array es un tipo de dato compuesto. Es una colección de datos.
- En un array hay una asociación entre cada valor y su clave de acceso.
- Las claves pueden ser:
  - Números: arrays ordenados.
  - Cadenas de texto: arrays asociativos (mapas).


### Declarar un array ordenado:

- Podemos usar los corchetes o la función `array`.

  ```php
  $frutas = ['manzana', 'naranja', 'uva'];
  $frutas = array('manzana', 'naranja', 'uva');
  $frutas = [0 => 'manzana', 1 => 'naranja', 2 => 'uva'];
  $frutas = array(0 => 'manzana', 1 => 'naranja', 2 => 'uva');
  //las cuatro sentencias son equivalentes
  ```

- Ojo, podríamos combinar tipos de datos:

  ```php
  $array = [2, 'naranja', 3.1416];
  ```

- Y acceder así:

  ```php
  echo "Me gusta la $frutas[2]";
  ```


### Añadir y quitar elementos

Podemos añadir elementos. El array crece a demanda:

  ```php
  $frutas[] = 'manzana'; // si fruta está vacío, posición 0
  $frutas[] = 'naranja'; // ahora posición 1
  $frutas[2] = 'uva'; //ahora posición 2 porque lo ponemos, o cualquier otra.
  ```

Y podemos eliminar elementos:

  ```php
  unset($frutas[1]);  
  ```


### Recorrer un array

- Lo ideal es usar un bucle `foreach`. Hay dos variantes:
```php
//si no nos preocupa la clave de cada valor
foreach ($frutas as $fruta){
    echo $fruta . '<br>';
}
foreach ($frutas as $clave => $fruta){
    echo $clave . ": " . $fruta . '<br>';
}
```


### Arrays asociativos

- En un array asociativo las **claves** para referenciar cada **valor** son *strings*.
- Para declarar un array o añadir elementos:

  ```php
  //podemos declarar un array:
  $alumno = array (
      'id' => 5,
      'nombre' => 'Manuel',
      'apellido' => 'García López',
      'edad' => 23
  );
  $alumno['sexo'] = "V"; //y podemos añadir elementos 
  ```


- Igualmente podemos usar foreach en cualquiera de sus variantes.
- Y podemos acceder a un valor usando corchetes: $alumno['nombre'] = "Juan";


### Arrays multidimensionales

- Un elemento de un array puede ser otro array.
- Esto nos permite definir arrays de dos o más dimensiones.

```php
$filas  = [
    0 => [11, 12],
    1 => [21, 22],
    3 => [31, 32]
];
```



## Formularios

- Los formularios son la forma más común de enviar información del cliente al servidor.
- Vamos a ver en qué consiste.


### HTML

- ¿Qué puedo encontrar en un formulario?
  - La etiqueta form que define el método (*method* *get* o *post*)
  - El destinatario (*action*)
  - Y los datos a rellenar o inputs

```html

<form action="/ruta_destino" method="get">
    <input type="text" name="nombre" value="">
    <input type="password" name="password" value="">
    <input type="hidden" name="secreto" value="">
    <input type="submit" value="enviar">
</form>
```


- El método GET envía los datos en la cabecera. 
  - Los datos se ven en la barra del navegador. 
  - Pueden usarse en enlaces.

  ```
  http://misitioweb.com/destino.php?nombre="juan"&edad="16"
  ```

- El método POST envía los datos en el cuerpo. Es más seguro.
  ```
  http://misitioweb.com/destino.php
  ```


- Normalmente el formulario lo crea un fichero y recibe sus datos otro fichero indicado en el *action*.
- El *action* puede:
  - Usar rutas absolutas  (**RECOMENDABLE**)
   
  ```php
  action="/destino.php"
  ```

  - Usar rutas relativas
  ```php
  action="destino.php"
  ``` 
  - Hacer referencia al fichero actual
  ```php
  `action="#"`
  ```


- Los input que debéis usar son los del ejemplo: text, password, hidden y submit.
- No debéis usar tipos como numeric o date. Estos tipos mejoran la esperiencia de usuario pero no permiten probar algunas cosas en el servidor. **No nos interesa usarlos en este módulo**


### Variables superglobales

- Las variables superglobales son creadas por el sistema.
- Las variables que podemos encontrar son:
  <small>

    - $_SERVER. Inf. del script actual y del servidor.
    - $_GET. Datos formulario con método GET.
    - $_POST. Idem con método POST.
    - $_FILES. Ficheros enviados en un formulario.
    - $_COOKIE. Cookies (para más adelante)
    - $_REQUEST. Combina las tres anteriores.
    - $_SESSION. Datos de sesión (para más adelante)
  
  </small>



### Recepción de datos

- Los datos se reciben así:
  ```php
  $nombre = $_GET['nombre']; //si el método es GET
  $nombre = $_POST['nombre']; //si es POST
  $nombre = $_REQUEST['nombre'];//válido en ambos casos
  ```
- Puede ser conveniente comprobar si existe la variable:

```php
if (isset($_GET['nombre'])) {
  $nombre = $_GET['nombre'];
} else {
  $nombre = '';
}
```


### Uso de checkbox y de radio


### Uso de select


### Enviar formularios al script de origen


### Usar arrays en formularios


### Usar elementos ocultos en formularios


#### Ejercicio

- Crea un formulario para enviar los datos de registro de un libro: título, autor, editorial, páginas.


#### Ejercicio

- Crea un formulario para enviar campo nombre. Si el nombre existe se da un saludo. Si no existe se vuelve atrás indicando que el campo es obligatorio.


#### Ejercicio

- Crea un formulario para enviar campo nombre. El nombre debe existir y debe tener un tamaño mínimo de 3 caracteres. Si es válido se da un saludo. Si no lo es se vuelve atras indicando que el campo es obligatorio y mostrando en el "input" el valor anterior no válido.


#### Ejercicio

- Crea un formulario que funcione como calculadora. Debe contener dos input como operandos y un select para elegir operador.
  - Si se reciben los datos muestra el resultado. 
  - Si no son válidos o no existen debe devolver a la página anterior.


#### Ejercicio

- Crea un formulario que envíe un array de 3 nombres. Para hacerlo debes usar el mismo nombre en todos los input (name="nombres[]").


#### Ejercicio

- Inspírate en el anterior. Crea un formulario que envíe un nombre. El action debe ser el mismo script. Muestra el nombre recibido en un input. Añade otro input vacío para un nuevo nombre. A cada pulsación, la lista de nombres debe ir creciendo.


## Funciones 



## POO



## Namespaces



## Separar vista de lógica
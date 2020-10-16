# PHP


## Entorno de desarrollo.

- El enfoque tradicional sería usar una máquina con windows o linux y:
  - Servidor web. Típicamente Apache 2.
  - Servidor de bases de datos. Típicamente Mysqsl.
  - Entorno de administración de bases de datos. Típicamente PhpMyAdmin.
- Yo opto por usar `docker`. Esto me da más seguridad respecto al entorno usado por los alumnos.


- Vamos a partir del entorno de desarrollo preparado con docker:

https://github.com/rafacabeza/entornods

- Debemos clonar dicho repositorio en nuestro espacio de trabajo:

```bash
cd ~
git clone git@github.com:rafacabeza/entornods.git
```


- Obtenemos un entorno docker para desarrollar en php:
  - Uno o más servicios web.
  - Un servicio de base de datos Mysql
  - Un servicio PhpMyAdmin para administrar las bases de datos.


- Para gestionarlo necesitamos la consola y necesitamos ir a la carpeta que lo contine.
  
  ```
  cd entornods
  ```

- Iniciar servicio:
  
  ```
  docker-compose up -d
  ```

- Parar servicio:
  
  ```
  docker-compose down
  ```

- Ver máquinas corriendo:
  
  ```
  docker-compose ps
  ```


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

- El repositorio entornods está en su rama master. Ahí tenemos los servicios que hemos explicado.
- Pero ahora no necesitamos web1 ni web2. Necesitamos un servicio para hacer ver ejemplos y hacer ejercicios.
- El uso de ramas en *git* me permite hacer esto fácilmente


- Vamos a cambiar a la rama ejercicios de nuestro "entornods"
  ```
  cd ~/entornods
  git checkout --track origin/ejercicios
  ```
- Nuestro docker-compose ya está preparado para servir el contendio de "data/ejercicios". Examina el fichero docker-composer.yml para comprobarlo.


- Lo siguiente es crear ese directorio *data/ejercicios*. 
- Podríamos hacerlo desde 0 pero vamos a usar el [repositorio que a hemos preparado para eso](https://github.com/rafacabeza/ejerciciciosphp/).
- Ese repositorio es propiedad del profesor. Me sirve para descargarme cosas pero no para guardar mis soluciones.


- Por eso no me interesa hacer un "clon" sino un "fork".
  - Hacer un clon consiste en descargar el código del profesor (no me intereasa).
  - Hacer un fork consiste en copiar el repositorio del profesor en el espacio de GitHub del alumno.
  - Ese repositorio resultante es el que debe ser clonado.

  ```
  cd data
  git clone git@github.com:USUARIO_ALUMNO_EN_GITHUB/ejerciciciosphp.git
  ```


- Una vez clonado, nuestro repositorio local está vinculado al que guardamos en GitHub
- Podemos comprobarlo ejecutando:
  
  ```
  cd ejercicios
  git remote -v  
  ```

- El resultado indica algo así:

  ```
  origin	git@github.com:USUARIO_ALUMNO_EN_GITHUB/ejerciciciosphp.git (fetch)
  origin	git@github.com:USUARIO_ALUMNO_EN_GITHUB/ejerciciciosphp.git (push)
  ```


- Como vermos más adelante, me puede interesar vincular un repositorio local a dos remotos:
  - El mío (alumno) con mis ejercicios.
  - El del profesor, por si añade ejemplos o enunciados de problemas.
- Para hacerlo:

  ```
  cd ejercicios
  git remote add rafa git@github.com:rafacabeza/ejerciciciosphp.git
  ```


- Nos falta preparar el fichero  `/etc/hosts`. Debemos añadir:

  ```
  127.0.0.1     ejercicios.local
  ```

- Ya casí está. Vamos a "entornods", paramos los servicios y los vovemos a levantar:

  ```bash
  cd ~/entornods
  docker-compose down
  docker-compose up -d
  ```
- Y vamos a nuestro navegador: http://ejercicios.local



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


### Elegir entre varias opciones:checkbox, radio y select

- Para entender las posibilidades debes analizar el ejemplo 11.


- Si queremos elegir uno entre varios podemos usar
  - Radio: 

  ```
  <input type="radio" name="sexo" value="male" checked> Varón <br>
  <input type="radio" name="sexo" value="female"> Mujer <br>
  ```

  - Select:

  ```
  <select name="color">
      <option>rojo</option>
      <option>azul</option> 
      ...
      <option selected>blanco</option>
  </select>
  ```
<small>MIRA BIEN: para preseleccionar una opción debemos usar checker o selected.</small>


- Si queremos seleccionar varios entre varios debemos usar checkbox (o select multiple).
- En ambos casos nos interesa recibir un array con la colección de *varios* elegida.
- En PHP, debemos nombrar a nuestros inputs (o select) con corchetes:

  ```
  <input type="text" name="ideas[]"> 
  <input type="text" name="ideas[]"> 
  ```


- Otra cosa interesante es que un script puede enviarse los datos a sí mismo. Basta con poner en el action del formulario el propio script, o más fácil todavía, dejando el action en blanco:

  ```php
  //Código del ejemplo 13
  <form action="">
    <input type="text" name="nombres[]">
    <input type="submit" value="Nuevo">  
    <hr>
    <?php
    if (isset($_GET['nombres'])) {
      foreach($_GET['nombres'] as $nombre) {
        echo '<input type="text" name="nombres[]" value="' . $nombre . '"><br>';
      }
    }
    ?>
  </form>
  ```


#### Ejercicio

- Crea un formulario para enviar los datos de registro de un libro: título, autor, editorial, páginas.


#### Ejercicio

- Crea un formulario para enviar campo nombre. Si el nombre existe se da un saludo. Si no existe se vuelve atrás indicando que el campo es obligatorio.


#### Ejercicio

-  Envío del script al mismo script. Crea un formulario para enviar campo nombre. El nombre debe existir y debe tener un tamaño mínimo de 3 caracteres. Si es válido se da un saludo. Si no lo es se vuelve a mostar el formulario indicando que el campo es obligatorio y mostrando en el "input" el valor anterior no válido. 


#### Ejercicio

- Envío del script al mismo script. Crea un formulario que funcione como calculadora. Debe contener dos input como operandos y un select para elegir operador. 
  - Si se reciben los datos muestra el resultado. 
  - Si no son válidos o no existen debe devolver a la página anterior.


#### Ejercicio

- Crea un formulario que envíe un array de 3 nombres. Para hacerlo debes usar el mismo nombre en todos los input (name="nombres[]").


#### Ejercicio

-  Crea una lista usando etiquetas ul y li. La lista inicialmente estará vacía pero un formulario con un input servirá para añadir los elementos. Usa input de tipo hiddens para que no "olvidar" los elementos ya añadidos a la lista. 


## Funciones 

- Php es un lenguaje de scripting. El punto de entrada es la primera línea de un script siempre.
- Pero soporta programación estructurada, por tanto funciones, y programación orientada a objetos, por tanto clases.


- No vamos a poner mucha energía en las funciones (sí en POO), pero veamos un ejemplo:

  ```php
    <?php

    function factorial($numero) {
      $resultado = 1;
      for ($i=1; $i <= $numero; $i++) { 
        $resultado = $resultado * $i;
      }
      return $resultado;
    }
    echo "El factorial de 5 es " . factorial(5);
    echo "<br>";
    echo "Y el factorial de 10 es " . factorial(10);
    ?>
  ```



## POO

- Php soporta POO desde la versión 5
- En las versiones 7.* las posibilidades son muy completas.
- Vamos a ver las cuestiones básicas.


- Conceptos base: clases, objetos, atributos y métodos.
- Visibilidad: public, protected, private
- Métodos mágicos: constructor, destructor, toString, ...
- Herencia, interfaces, métodos y atributos estáticos y finales
- Muchas cosas y algunas de ellas no las emplearemos nunca.
- Vamos a ir explicando conceptos ya conocidos segun nos haan falta.


### Notación

- Nombre de la clase *CamelCase*.
- Una clase - un fichero, y ambos con el mismo nombre.
- Atributos y funciones con nombres *camelCase*

```php
class ClaseSencilla
{
    // Declaración de una propiedad
    public $var = 'un valor predeterminado';

    // Declaración de un método
    public function mostrarVar() {
        echo $this->var;
    }
    //NOTA: $this hace referenca a este objeto
    //"->" se usa para acceder a métodos y atributos
}
?>
```


### Métodos mágicos

- Reciben este nombre los que se ejecutan de foma "mágica". Sin petición explícita.
- Siempre empiezan por doble guión bajo. Los más destacables:
  - `__construct()`: constructor. Se ejecuta al crear un objeto.
  - `__destruct()`: destructor. Se ejecuta al eliminar un objeto.
  - `__toString()`: se ejecuta cuando imprimimos un objeto. Lo convierte a string.


### Particularidades

- Para acceder a métodos y atributos usamos **"->"**.
- Para acceder a constantes de clase y métodos estáticos usamos **"::"**
- Para referirnos al propio objeto usamos **$this**


### Mi primera clase

```php

<?php

class Persona  
{
  public $nombre;
  public $apellido;
  public $edad;

  public function __construct($nombre, $apellido, $edad)
  {
    $this->nombre = $nombre;
    $this->apellido = $apellido;
    $this->edad = $edad;
  }

  public function saludar()
  {
    echo "Buenos días!";
  }

  public function __toString()
  {
    return $this->nombre;
  }
}
```


- Y ahora la usamos:

```php
<body>
  <?php
    require('Persona.php');
    $juan = new Persona('Juan', 'García', 15);

    echo $juan-> saludar();
    echo "<br>";
    echo "Soy $juan";
    echo "<br>";
    echo "Mi nombre completo es $juan->nombre $juan->apellido y tengo $juan->edad años";
  ?>
</body>
```


### Una clase como aplicación web

- El script de entrada:

```php
<?php

require_once "App.php";
$app = new App;
$app->run();
```


- La clase

```php
<?php

class App
{
  public function __construct($name = "Aplicación PHP")
  {
    echo "Consturyendo la app <hr>";
    $this->name = $name;
    $this->module = "Desarrollo Web en Entorno Servidor";
    $this->teacher = "Rafael Cabeza";
    $this->student = "Fulano De Tal";
  }

  public function run()
  {
    echo "Moneda al aire... <hr>";
    $moneda = rand(0,1);
    // if ($moneda == 1) {
    if ($moneda) {
      echo "<h3>Ha salido cara:  </h3> <br>";
      $this->index();
    } else {
      echo "<h3> Ha salido cruz: </h3> <br>";
      $this->login();
    }
  }

  public function index()
  {
    echo "Estamos en el index<br>";
    echo "Estos es $this->name<br>";
    echo "Me llamo $this->student<br>";
    echo "Estamos estudiando $this->module con el profesor $this->teacher<br>";
  }
  
  public function login()
  {
    echo "Ahora podría mostrar un formulario de login <br>";
  }  
}
```


### Separar vista de lógica

- Pero podemos ser un poco formales en nuestro código:
  - Los cálculos que debamos hacer los hacemos dentro de un método de la clase. Un fichero php puro.
  - El html lo generaremos en un fichero mixto html/php.
- Esto no es obligatorio. No es requisito del lenguaje. Pero nos ayudará a crear código limpio.


- Podríamos trasformar nuestra clase así:

```php
  public function index()
  {
    echo "Estamos en el index<br>";
    include('views/index.php');
  }
  
  public function login()
  {
    echo "Estamos en login <br>";
    include('views/form.php');
  }  
```

- **Ojo**. Necesiamos un carpeta views


- Y nuestras vistas quedarían así:

```php
...
<body>
  <h1>Home de <?= $this->name ?></h1>
  <div>
  Estamos en el index
  </div>

  Me llamo <?= $this->student ?>
  <br>
  Estamos estudiando <?= $this->module ?> con el profesor <?= $app->teacher ?>
</body></html>
```


```php
...
<body>
  <h1>Login de  <?= $this->name ?></h1>

  <form action="">
    <label for="">nombre</label>
    <input type="text" name="name"> <br>
    <label for="">contraseña</label>
    <input type="password" name="password"> <br>
    <input type="submit">
  </form>
</body>
</html>
```


- Vamos a refinar un poco más nuestra aplicación
- Dejemos de jugar al azar. ¿Cómo decidimos qué método?
- Podemos hacerlo de muchas maneras. Vamos a optar por *argumentos* GET.

```
http://ejercicios.local/ejemplos/18/index.php?method=index
http://ejercicios.local/ejemplos/18/index.php?method=login
//equivalente:
http://ejercicios.local/ejemplos/18?method=index
http://ejercicios.local/ejemplos/18?method=login
```


- Debemos recoger esos argumentos en el *run* de *App*:

```php
  public function run()
  {
    if (isset($_GET['method'])) {
      $method = $_GET['method'];
    } else {
      $method = 'index';
    }
    $this->$method();
  }
```


- Y debemos añadir enlaces en ambas páginas.

  ```php
  <header>
    <ul>
      <li><a href="/ejemplos/18?method=index">Inicio</a></li>
      <li><a href="/ejemplos/18?method=login">Login</a></li>
    </ul>
  </header>
  ```


- ¿Lo ponemos en cada vista? 
  - Mejor creamos un *header.php*
  - Y lo incluímos en ambas vistas

  ```php
    <?php
      require('views/header.php');
    ?>
  ```


- Además podemos añadir un poco de CSS

```css
ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

li {
  float: left;
}

li a {
  display: block;
  padding: 8px;
  background-color: #dddddd;
}
```


#### Ejercicio.

- Crea una aplicación web con una clase App y varios métodos. En todos los casos se trata de obtener una serie numérica. El método debe calcular la serie y guardarla en un array, después hay que incluir una vista que muestre la serie. Puede ser que necesites crear métodos auxiliares (private) para el cálculo del array, por ejemplo: esPrimo(). Los métodos necesarios son:


  - Index (**index**). Presentación de la App y enlaces.
  - Fibonacci (**fibonacci**). Muestra la serie de Fibonacci. Debe mostrar todos los términos menores a un millón.
  - Potencias de 2 (**potencias2**). Debe mostrar los valores de las potencias de 2 hasta 2 elevado a 24 (nº de colores True Color, por ejemplo).
  - Factorial (**factoriales**). Debe mostar los factoriales desde 1 hasta n de tal manera que el último término sea el más próximo cercano al millón.
  - Nº. primos (**primos**). Debe encontrar los números primos entre 1 y 10.000. 



## Cookies

- Una cookie es un fichero de texto que se guarda en el equipo del cliente.
- Este fichero está ligado al sitio web que lo crea y al navegador usado.
- En él se almacena el contenido de un array de variables.


- Se crea mediante la función setcookie(). La función necesita:
  - Clave: nombre de la variable.
  - Valor: contenido de la misma. Debe ser texto (o convertible).
  - Tiempo unix de vida (en segundos). Por defecto, 0, se elimina al cerrar el navegador.
- Se accede a su contenido mendiante la variable superglobal $_COOKIE
- Para eliminar una cookie usa setcookie con tiempo menor al actual: 

```php
setcookie(x,y,time()-1)
```


- Ejemplo de creación de cookies. Incluso arrays y objetos:

```php
    setcookie("user", "Fulanito de Tal", time() + 3600);

    //ojo para guardar arrays:
    $hobbies = ['futbol', 'música rock', 'tocar la guitarra con mis amigos'];
    setcookie("hobbies", serialize($hobbies), time() + 3600);
    setcookie("hobbies2", json_encode($hobbies), time() + 3600);

    //y objetos:
    $persona = new Persona("Juan", "Pérez", 21);
    setcookie("persona", serialize($persona), time() + 3600);
    setcookie("persona2", json_encode($persona), time() + 3600);
```


- Arrays y objetos deben tratarse con:
  - serialize/unserialize: nativo php. Los objetos se recrean en su clase.
  - json_encode/json_decode: estándar. Los objetos se recrean como stdClass.


### Ejercicio

- Vamos a crear una App con estos métodos: 
  - login: muestra un formulario de login (usuario y contraseña).
  - auth: guarda el usuario y su contraseña en una cookie. Después reenvía la petición a home.
  - home: Muestra un saludo y un enlace para cerrar sesión.
  - logout: elimina las cookies (setcookie(...., time() - 1)) y reenvía a login.
- Depura tu código. En login, comprueba que no hay ya un usuario. Si lo hay reenvía a home.


### Ejercicio

Se trata de crear una lista de deseos Usaremos la clase App con los siguietens métodos:

- **login** método que muestra formulario de entrada.
- **auth** método que toma el nombre de usuario tras el login. Tras hacer esto reenvía a **home**.
- **home** método que muestra la lista de deseos. Además muestra un formulario de nuevos deseos. El formulario envía al método **new**
- **new** toma el nuevo deseo y lo incluye en la lista.


- **delete** borra un deseo de la lista. Debe recibir el indice del deseo.
- **empty** vacía la lista de deseos.
- **close** Cierra sesión: borra la cookie.


### Ejercicio

Colorear una página con ayuda de una cookie. Usaremos la clase App con los siguietens métodos:

- ** home ** muestra un mensaje de bienvenida. Comprueba si hay una cookie llamada "color". Si existe la usa para darle color al fondo de la ágina.
- ** colores ** muestra una lista de colores. Cada color tiene un enlace del estilo **?method=cambio&color=red** . Al hacer clic cambiará el color del home.
- ** cambio **. Recibe el color de la página anterior, crea la cookie y reenvia ('header...') al método home.

NOTA: dos vistas (home y colores) y un reenvío (cambio).



## Sesiones

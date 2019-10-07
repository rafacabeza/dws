# PHP

## Por donde empezar


Puedes descargar el material de clase e instalarlo en tu servidor web local. Para hacerlo:

* Instala Apache2
* Instala el módulo "rewrite" en Apache:
    `a2enmod rewrite`
* Añade lo siguiente a la configuración de tu sitio web por defecto. El fichero es `/etc/apache2/sites-enabled/000-default.conf`

  ```
    <Directory /var/www/html>
            AllowOverride All
    </Directory>
  ```
No olivdes reiniciar el servicio. 

* Descarga el código al directorio de tu sitio web de una de las siguientes formas.

  * Con claves ssh

    `alumno@alumno-VirtualBox:/var/www/html# git clone git@bitbucket.org:rafacabeza/introduccionphp.git`

  * Con https, sin claves ssh:

    `alumno@alumno-VirtualBox:/var/www/html# git clone https://rafacabeza@bitbucket.org/rafacabeza/introduccionphp.git`


## Php Html
El código php puede mezclarse dentro del código php. El código php se marca así: <?php mi_codigo_php ?>

```html
<!DOCTYPE html>
<html>
<head>
    <title>Html y php</title>
</head>
<body>
    <?php 
        //aqui va el código php
    ?>
</body>
</html>
```


Los ficheros cuyo contenido es 100% php requieren la etiqueta de apertura '<?php' pero no la de cierre. '?>' 

```php
<?php 

//este sería un fichero de php puro
```

## Configuración PHP

La configuración de php está registrada en el fichero php.ini y puede ser consultada con la función phpinfo()
```php
<?php
phpinfo();
```
Ejemplo 1


## Operadores
Los operadores son en general iguales a C o Java. Podemos revisar la documetación oficial en:
http://php.net/manual/es/language.operators.php

No obstante vamos a remarar algunos casos especiales

- El operador "." sirve para concatenar textos.
- Además de los comparadores == y != existen los operadores === y !==. El operador === devuelve true si el valor y el tipo de datos son iguales. El operador !== devuelve true si el valor o el tipo de datos son distintos.
- De manera similar al operador "+=" Tenemos el operador ".="

```php
<?php
$n1 = "5";
$n2 = 5;
$comparacion1 = $n1 == $n2; //true
$comparacion2 = $n1 === $n2;//false


$a = "Hello ";
$b = $a . "World!"; // ahora $b contiene "Hello World!"

$a = "Hello ";
$a .= "World!";     // ahora $a contiene "Hello World!"
```
Ejemplo 0
    
## Estándar de código

- El uso de estándares de codificación es fundamental en un entorno de trabajo.
- En el caso de PHP existen los estándares [PSR](http://www.php-fig.org/psr/).
  - PSR-1 sienta las bases del estándar de codificaiónc.
  - PSR-2 Sigue las recomendaciones PSR-1 algunas más.
  - PSR-4 Se refiere a la nomenclatura de namespaces.
- Lo más básico:
  - La llave de apertura de las clases DEBEN ir en la siguiente línea que el nombre de la clase.
  - Idem para las funciones o métodos.
  - La llave de las estructuras de control debe ir en la misma línea .que la cabecera y separada de un espacio.
  - Nombre de las clases en estilo StudlyCaps (O CamelCase).
  - Nombre de los métodos en estilo camelCase
  - Nombre de atributo es libre pero seguiremos el standar Zend: camelCase.
  - Nombre de constantes mayúsculas con guiones bajos (SNAKE_CASE).
  - En las clases siempre se debe declarar la visibilidad.
  - Etiquetado `<?php  ... ?>` o `<? ... ?>`. Nosotros usaremos el primero siempre.
  - En los ficheros puros PHP, el marcado de cierre se elimina: `<?php ....`

  
  
 ### ejercicio1.php
 
- Crea un fichero PHP donde te presesntes como alumno.
- Tu información (nombre, apellidos, edad, ...) debe estar guardada en variables.
- Hazlo usando echo y print
- Usa el entrecomillado simple y doble para ver las distintas posibilidades.
- Usa etiquetas html para mejorar la presentación.

### ejercicio2.php

- Vamos a calcular el precio con IVA de un producto.
- Carga los datos en variables (precioUnidad, cantidad, iva, precioSinIva, precioConIva, ...)
- Realiza los cálculos pertinentes y muestra detalladamente cada valor inicial y calculado.

### ejercicio3.php

- Crea un array ordenado con los días de la semana.
- Muestra el contenido del array.
- Hazlo de distintas maneras: declaración del array en una sola sentencia, añadiendo elementos uno por uno, ....
- Usa `<ul>...</ul>` o `<table>...</table>`

### ejercicio4.php

- Crea un array asociativo que contenga el quinteto inicial de un equipo de baloncesto.
- Muestra su contenido con un bucle foreach.
- Intenta distintas varianttes, accediendo o no a la clave -> valor.




## Funciones
- La palabra reservada para funciones es function.
- Para devolver valores usamos return.
- Los argumentos se pasan por valor salvo que se use el operador ampersand (&).
 ```
 function añadir_algo(&$cadenaReferencia)
{
    $cadena .= 'y algo más.';
}
```
- Podemos definir valores por defecto asignando un valor en la declaración de la función.

## Formularios
- Echa un vistazo a la explicación de formularios del material de clase.
- También puede ser interesante ojear el sitio https://www.w3schools.com/html/html_forms.asp


### Ejercicio5.php

- Crea dos ficheros ejercicio5.html y ejercicio5.php.
- El html debe dar la información de registro de un usuario.
  - Nombre
  - Apellidos
  - Edad
  - Aficiones (usar varios checkbox)
  - Sexo (usar _radio_)
  - Deporte favorito (usar un select).
- El fichero php debe mostrar por pantalla la información por pantalla usando una <ul>.

### Ejercicio6.php

- Modifica el ejercicio anterior para que las aficiones se reciban como un array _hobbies[]_

### Ejercicio7.php
- En el ejercicio 7 vammos a usar un sólo fichero _ejercicio7.php_
- El formulario va a enviar los datos a si mismo.
- Vamos a formar una lista de jugadores de futbol, cada vez que enviemos guardaremos un nuevo elemento en la lista. 
- PISTA: usa control(es) de tipo _hidden_.
  
## Revisiar lo aprendido y refactorizar.
- Aunque aún no hablemos de MVC (Modelo Vista Controlador). Vamos a separar la presentación de la lógica del programa. 
- Vamos a separar el código en funciones.
- En php podemos incluir el contenido de un fichero en otro usando:
  - require: se incluye un fichero, si no se encuentra de error.
  - include: se incluye un fichero, si no se encuentra sigue adelante.
  - Existen require_once e include_once. Veremos su uso.

### Ejercicio8.php
- Parte del ejercicio7. 
- Lleva la construcción de html a un fichero ejercicio8vista.php.
- La lógica déjala en ejercicio8.php
- Refactoriza la lógica: usa funciones.

## Cookies

- Una cookie es un fichero que se guarda en el fichero del cliente.
- Este fichero está ligado al sitio web que lo crea y al navegador usado.
- En él se almacena el contenido de un array de variables.
- Se crea mediante la función setcookie(). Esta función admite además otros parámetros pero nos centraremos sólo en $expire, o tiempo de vida en segundos. Por defecto, 0, se elimina al cerrar el navegador.
- Se accede a su contenido mendiante la variable superglobal $_COOKIE
- Para eliminar una cookie usa setcookie con tiempo menor al actual: setcookie(x,y,time()-1)
- Puede ser interesante usar arrays:
  ```
    setcookie("miCookie[posicion1]", $valorDado, 3600);
    $valorGuardado = $_COOKIE["miCookie"]["posicion1"];
  ```
  
### ejercicio9.php
#### Parte 1
- Crea una carpeta: ejercicio9
- Usaremos dos ficheros: index.php y respuesta.php.
<!-- - TODAS LAS PETICIONES LAS DEBE ATENDER index.php. -->
- **index.php** muestra un formulario para rellenar tu nombre.
- Formulario: método GET, action respuesta.php.
- Respuesta.php saluda al nombre enviado ("hola fulanito")
- Antes de saludar, respuesta debe guardar una cookie con el 
nombre recibido.
- En visitas sucesivas, el formulario.php debe aparecer pre-relleno con el nombre enviado en la anterior ocasión. Este nombre está en una cookie. 



### Ejercicio 10

- Modifica el ejercicio anterior para conseguir la misma funcionalidad usando POO.
- Crea una clasee App con dos métodos, index y home.
- El método index muestra el formulario de login (sin contraseña). Si existe cookie el nombre de usuario debe aparece completado.
- El método home recoge los datos del formulario y guarda la cookie. Además da la bienvenida si existe nombre de usuario y si no pregunta _¿Quién eres?_

### Ejercicio 11

Ejercicio de sesiones y POO. Se trata de crear una lista de deseos Usaremos la clase App con los siguietens métodos:

- **login** método que muestra formulario de entrada.
- **auth** método que toma el nombre de usuario tras el login. Tras hacer esto reenvía a **home**.
- **home** método que muestra la lista de deseos. Además muestra un formulario de nuevos deseos. El formulario envía al método **new**
- **new** toma el nuevo deseo y lo incluye en la lista.
- **delete** borra un deseo de la lista. Debe recibir el indice del deseo.
- **empty** vacía la lista de deseos.
- **close** Cierra sesión. Olvida deseos y usuario.

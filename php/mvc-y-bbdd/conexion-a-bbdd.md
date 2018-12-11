## Antes de empezar

### Instalacion de MySql y PhpMyAdmin
Instalación de MySql
``` 
sudo apt-get install mysql-common 
sudo apt-get install mysql-server
sudo apt-get install mysql-client
```


Instalación de phpmyadmin
``` 
sudo apt-get install phpmyadmin
```

### Juego de caracteres y cotejamiento \(Charset & Collate\).

Collate: reglas para ordenar y comparar textos.

Lectura recomendada:

[Sobre Collation en MySql](http://blog.unreal4u.com/2012/08/sobre-collation-y-charset-en-mysql/)

[Sobre juegos de caracteres](https://dev.mysql.com/doc/refman/5.7/en/charset-applications.html)

Se recomienda definir un charset y collate en la creación de la BBDD. Después en las tablas dejar ambos parámetros por defecto, salvo necesidades especiales.

Collate más correctos:

* utf8\_spanish\_ci2 → para orden correcto en español.
* utf8\_unicode\_ci → para orden correcto en multitud de lenguas.
* utf8\_general\_ci → el más rápido

Recomendación

* Default database character set: utf8 \(UTF-8 Unicode\)
* Default database collation order: utf8\_general\_ci \(UTF-8 Unicode\)

### Ejemplo de creación de base de datos

```sql
CREATE DATABASE mydb
 DEFAULT CHARACTER SET utf8
 DEFAULT COLLATE utf8_general_ci;
```

### Conexión a la base de datos.

#### Controladores

Existen controladores para un gran número de sistemas de bases de datos. Por citar algunos, Oracle Database, MS Sql Server, MongoDB, SqlLite,... No obstante, nosotros vamos a trabajar con MySql.

Tres modos de conexión:

MySQL \(Original\) — API original de MySQL . Controlador obsoleto, no usar:

mysql\_connect\('mysql\_host', 'mysql\_user', 'mysql\_password'\)

MySQLi — Extensión MySQL mejorada

PDO — Objetos de datos de PHP . Sistema genérico que permite acceder a múltiples sistemas con unas clases únicas. Muy interesante si cabe la posibilidad de tener que migrar de sistema de bbdd.

#### La conexión

En la Guía rápida del manual podemos tomar contacto. También es interesante el enlace:

[Extensión MySQLi - Parte 1: Consultas de modificación
](http://programandolo.blogspot.com.es/2013/06/extension-mysqli-parte-1-consultas-de.html)

MySQLi ofrece dos modos de trabajo:

* Procedimental.
* Orientado a objetos. Por simplicidad usaremos sólo este en clase.

Un ejemplo de conexión:

```php
//abrir conexión, se pasan unas constantes como parámetros:
$mysqli = new mysqli(DB_HOST, DB_USER, DB_PASS, DB_DATABASE);
//si da error salir del script con die() (equivale a exit())
if($mysqli->connect_errno > 0){
    die("Imposible conectarse con la base de datos [" . $mysqli->connect_error . "]");
}
```

Si usamos [PDO](http://php.net/manual/es/pdo.connections.php):

```php
<?php$mbd = new PDO('mysql:host=localhost;dbname=prueba', $usuario, $contraseña);?>

```

### Los errores

* Die\(\) es una ruptura brusca de la ejecución. Sólo tiene sentido para depuración, no es aceptable en una aplicación acabada.

* Un error se produce cuando la ejecución del programa llega a un punto no esperado: un fichero que no se encuentra, una tabla inexistente en una  bbdd, ... Si no se gestiona adecuadamente puede suponer la finalización brusca del programa.
* Una excepción es la alteración del flujo normal de un programa para la ejecución de un bloque de código que informa de la existencia de una situación especial y que no provoca la finalización del programa de forma brusca.
* Las excepciones pueden ser capturadas dentro de los bloques ```php try{}``` 
* A un ```php try{}``` le debe seguir al menos un ```php catch{}```  y opcionalmente un ```php finally{}``` 



```php
<?php
function inverse($x) {
    if (!$x) {
        throw new Exception('División por cero.');
    }
    return 1/$x;
}

try {
    echo inverse(5) . "\n";
} catch (Exception $e) {
    echo 'Excepción capturada: ',  $e->getMessage(), "\n";
} finally {
    echo "Primer finally.\n";
}

try {
    echo inverse(0) . "\n";
} catch (Exception $e) {
    echo 'Excepción capturada: ',  $e->getMessage(), "\n";
} finally {
    echo "Segundo finally.\n";
}

// Continuar ejecución
echo 'Hola Mundo\n';
?>

```
* Las excepciones pueden lanzarse con ```throw()``` o pueden ser lanzadas por las clases que usemos.

* En el caso de PDO, existen tes modos. Vamos a explicar los dos básicos:
    * El nivel por defecto, PDO::ERRMODE_SILENT, donde los las excepciones se lanzan mediante throw....
    * El nivel PDO::ERRMODE_EXCEPTION donde las excepciones son lanzadas por PDO de forma automática.

```php
<?php
$dsn = 'mysql:dbname=prueba;host=127.0.0.1';
$usuario = 'usuario';
$contraseña = 'contraseña';
try {
    $mbd = new PDO($dsn, $usuario, $contraseña);
    $mbd->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    echo 'Falló la conexión: ' . $e->getMessage();
}?>
```

http://php.net/manual/es/pdo.error-handling.php



### Consultas de selección \(SELECT\)

Una vez establecida la conexión:
- Ejecutamos la consulta
- Tomamos las filas resultado
- Con mysqli tenemos un juego de funciones para ([manual](http://php.net/manual/es/mysqli.summary.php)):
    - Tomar las filas de la consulta
    - Tomar los nombres de los campos
    - Contar el número de filas, ...
- Con PDO 
    - Una consulta devuelve un PDOStatement
    - Podemos usar fetch (fila por fila) y todas sus variantes.
```php
<?php 
$stmt = $db->query('SELECT * FROM table'); 
while($row = $stmt->fetch(PDO::FETCH_ASSOC)) { 
    echo $row['field1'].' '.$row['field2']; //etc... 
}
```
    - O podemos usar fetchAll (todas las filas) y también sus variantes.
```php
<?php 
$stmt = $db->query('SELECT * FROM table'); 
$results = $stmt->fetchAll(PDO::FETCH_ASSOC); 
//use $results
```
    -Existen funciones para obtener más información de la consulta : campos, número de filas, ultimo id insertado, ... ([manual](http://php.net/manual/es/class.pdostatement.php))

### Consultas de ejecución \(UPDATE, DELETE, INSERT\)

- Son similares a las anteriores aunque no devuelven filas, por tanto no hay que usar fetch.
- Ejemplo en Mysqli:
```php
<?php $results = $mysql->query("UPDATE table SET field='value'") or die(mysql_error()); 
$affected_rows = mysql_affected_rows($result); 
echo $affected_rows.' were affected';
 ```
- En PDO
```php
<?php 
$affected_rows = $db->exec("UPDATE table SET field='value'"); 
echo $affected_rows.' were affected'
```


### Consultas preparadas
SOLO LO VEMOS EN PDO
- [Manual](http://php.net/manual/es/pdo.prepared-statements.php)
- Enlace a [wiki](http://wiki.hashphp.org/PDO_Tutorial_for_MySQL_Developers#Running_Statements_With_Parameters)
- Pasos:
    - Preparación de la consulta:
    - Vinculación de valores o variables
    - Ejecución

- Parámetros sin nombre y bindValue
```php
<?php 
$stmt = $db->prepare("SELECT * FROM table WHERE id=? AND name=?");
 $stmt->bindValue(1, $id, PDO::PARAM_INT);
 $stmt->bindValue(2, $name, PDO::PARAM_STR); $stmt->execute();
 $rows = $stmt->fetchAll(PDO::FETCH_ASSOC);
```
- Parámetros con nombre y bindValue

```php
<?php
$stmt = $db->prepare("SELECT * FROM table WHERE id=:id AND name=:name");
$stmt->bindValue(':id', $id, PDO::PARAM_INT);
$stmt->bindValue(':name', $name, PDO::PARAM_STR);
$stmt->execute();
$rows = $stmt->fetchAll(PDO::FETCH_ASSOC);
```
- Directamente en la ejecución:

```php
<?php $stmt = $db->prepare("SELECT * FROM table WHERE id=:id AND name=:name"); 
$stmt->execute(array(':name' => $name, ':id' => $id)); 
$rows = $stmt->fetchAll(PDO::FETCH_ASSOC);
```
- Si usamos bindParam en vez de bindValue se vincula la varible. Interesante en caso de bucles.
####  Repaso ejecución en PDO
- PDO es el objeto de conexión.
- PDOStatement (sentencia PDO) representa una sentencia SQL. Cuenta con métodos de ejecución y de devolución de datos.
- Operaciones ordinarias (no preparadas):
    - PDO::exec — Ejecuta una sentencia SQL y devuelve el número de filas afectadas. Para insert, delete y update.
    - PDO::query — Ejecuta una sentencia SQL, devolviendo un conjunto de resultados como un objeto PDOStatement. Para selección (select ...).


- Sentencias preparadas
    - PDO::prepare — Prepara una sentencia para su ejecución y devuelve un objeto PDOStatement.
    - PDOStatement::execute — Ejecuta una sentencia preparada

- El resultado de exec es un entero.
- El resultado de un PDOStatement (sentecia PDO) se recoge después de la ejecución mediante las distintas posibilidades:
    - fetch : devuelve una fila
    - fetchAll: devuelve todas las filas
    - Según la constante pasada por argumento ambas devolverán: arrays asociativas, arrays ordenadas u objetos (opciones principales).

### Transacciones

[Manual](http://php.net/manual/es/pdo.transactions.php)

- Por defecto cada sentencia constituye una transacción. Se ejecuta de forma independiente. Este es el llamado modo autocommit.
- Una transacción es una operación formada por varias sentencias SQL que se ejecutan como un todo indivisible.
- Las transacciones aseguran Atomicidad, Consistencia, Aislamiento y Durabilidad (ACID por sus siglas en inglés).
- Para usar varias sentencias en una transacción usamos:  PDO::beginTransaction() 
- Para concluir una transacción PDO::commit().
- Para deshacer los cambios de la transacción: PDO::rollBack().
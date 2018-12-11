# Paginación

* Paginar consiste en mostrar una lista parcial de elementos. Una porción o página del contenido de una tabla.
* Para realizar la paginación debemos saber: número de página y elementos por página.
* En MySql hay que usar la cláusuala LIMIT dentro del SELECT:

```sql
$sql = "SELECT * FROM Orders LIMIT 10 OFFSET 15";
//tomar 10 registros de Orders a partir del 16
$sql = "SELECT * FROM Orders LIMIT :pagesize OFFSET :offset";
//para consultas preparadas
```

* Veamos un método paginate en un modelo User:

```php

    public function paginate($size = 10)
    {
        if (isset($_REQUEST['page'])) {
            $page = (integer) $_REQUEST['page'];
        } else {
            $page = 1;
        }
        $offset = ($page - 1) * $size;


        $db = User::db();
        $statement = $db->prepare('SELECT * FROM users LIMIT :pagesize OFFSET :offset');
        $statement->bindValue(':pagesize', $size, PDO::PARAM_INT);
        $statement->bindValue(':offset', $offset, PDO::PARAM_INT);
        $statement->execute();
        $users = $statement->fetchAll(PDO::FETCH_CLASS, User::class);
        return $users;
    }


```

* Con esto tenemos la selcción de los registros de página. Ahora hay que buscar la información para construir una lista de páginas: número de páginas, y posición en la que estamos.

```sql
$sql = "SELECT count(id) as count FROM Orders";
//contar registros para poder preparar los enlaces.
```

* De nuevo en el model User:

```php
    public static function rowCount()
    {
        $db = User::db();
        $statement = $db->prepare('SELECT count(id) as count FROM users');
        $statement->execute();
        $rowCount = $statement->fetch(PDO::FETCH_ASSOC);
        return $rowCount['count'];
    }

```

* Con esta información se puede mostrar un listado paginado y construir una lista de enlaces por página. Código completo en la siguiente rama https://bitbucket.org/rafacabeza/mvc18/src/clase12nov/




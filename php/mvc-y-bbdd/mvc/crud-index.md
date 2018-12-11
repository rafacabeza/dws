# Listado de registros

* Controlador: UserController

```php
<?php
namespace App\Controllers;

use \App\Models\User;
require_once '../app/models/User.php';

class UserController
{

    public function index()
    {
        $users = User::all();
        require "../app/views/user/index.php";
    }
```

* Modelo: User

```php
<?php
namespace App\Models;

use PDO;
use Core\Model;

require_once '../core/Model.php';

class User extends Model
{

    public static function all()
    {
        $db = User::db();
        $statement = $db->query('SELECT * FROM users');
        $users = $statement->fetchAll(PDO::FETCH_CLASS, User::class);

        return $users;
    }
```

* Donde Model es:

```php
<?php
namespace Core;

use PDO;
/**
*
*/
class Model
{

    protected static function db()
    {
        $dsn = 'mysql:dbname=mvc18;host=127.0.0.1';
        $usuario = 'root';
        $contraseña = 'root';
        try {
            $db = new PDO($dsn, $usuario, $contraseña);
            $db->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        } catch (PDOException $e) {
            echo 'Falló la conexión: ' . $e->getMessage();
        }
        return $db;
    }
}
```

* Y el código principal de la vista php/html es:

```php
<h1>Lista de usuarios</h1>

<table class="table table-striped">
<thead>
<tr>
<th>Id</th>
<th>Nombre</th>
<th>Apellidos</th>
<th>Edad</th>
<th>Email</th>
<th>Acciones</th>
</tr>
</thead>
<tbody>
<?php foreach ($users as $user): ?>
<tr>
<td><?php echo $user->id ?></td>
<td><?php echo $user->name ?></td>
<td><?php echo $user->surname ?></td>
<td><?php echo $user->age ?></td>
<td><?php echo $user->email ?></td>
<td>
<a class="btn btn-primary" href="/user/show/<?php echo $user->id ?>">Ver</a>
<a class="btn btn-primary" href="/user/delete/<?php echo $user->id ?>">Borrar</a>
<a class="btn btn-primary" href="/user/edit/<?php echo $user->id ?>">Editar</a>
</td>
</tr>
<?php endforeach ?>
</tbody>
</table>
```




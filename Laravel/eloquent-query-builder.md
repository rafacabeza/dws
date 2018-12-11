### Eloquent
- Ya hemos visto algunos ejemplos de métodos disponibles en los modelos Eloquent. Vamos a revisarlos.
- Todos los registros: `$studies= Study::all();`
- Buscar por el id: `$studies= Study::find($id);`
- Crear un registro.
    - Como vemos en el ejemplo de arriba. Ojo debemos preparar el modelo ($fillable = ...)
    - O asigando campo a campo
```php
$study = new Study;
$study ->code= "IFC303";
$study ->name = "Desarrollo de aplicaciones web";
$study ->save();
}
```
- Modificar registro: 
```php
public function cambiaNombre($id, $nombre){
    $articulo = Articulo::find($id);
    $articulo->nombre= $nombre;
    $articulo->save();
}
```
- Borrar registro
```php
public function delete($id)
{
    // modo 1
    $study = Study::find($id);
   $study->delete();
   // modo 2
    Study::destroy($id);
    ...
}
```
- Pero además podemos buscar con where, ordenar o limitarnos a n-registros.
```php
public function index(){
    $studies= Study::where('code', 'like', 'IFC%')
    ->orderBy('code', 'asc')
    ->take(2)
    ->get();
    return view('study.index', [
        'studies' => $studies
    ]);
}
```
### Fluent Query Builder
... o simplemente _Fluent_.
- Permite realizar consultas sin usar SQL. 
- Por detrás usa PDO y tiene protecciones contra SQL inyection.
- Es independiente del SGBD.
#### Primeras consultas
- Debemos usar la fachada DB.
- Su método _table()_ nos devuelve un objeto _fluent_.
```php
$fluent = DB::table('studies');
``` 
- Sobre el objeto fluent podemos ejecutar distintos métodos:
```php
//obtener todos los registros en un array de objetos
$studies = $fluent->get();
//sólo el primero, en un objeto único.
$study = $fluent->first();
//o todo en una sentencia
$studies= DB::table('studies')->get();
$study= DB::table('studies')->first();
```
- Podemos acceder a los campos de los objetos:
```php
echo $study->code;
echo $studies[0]->code;
```
#### Select ... where ...
- Podemos seleccionar sólo algunas columnas.
```php
$studies = DB::table('studies')->select('name')->get();
$studies = DB::table(['code', 'studies'])->select('name')->get();
```
- Añadir condiciones where simples:
```php
$studies = DB::table('studies')->where('code', '=', 'IFC303')->get();
```
- Con comodines
```php
//en una línea o en varias para mejorar la legibilidad
$studies = DB::table('studies')->where('code', 'like', 'IFC%')->get();
$studies = DB::table('studies')
    ->where('code', 'like', 'IFC%')
    ->get();
```
- Usar varios where (AND)
```php
$studies = DB::table('studies')
    ->where('code', 'like', 'IFC%')
    ->where('name', 'like', '%web')
    ->get();
```
- Operaciones OR.
```php
$studies = DB::table('studies')
    ->where('code', 'like', 'IFC%')
    ->orWhere('code', 'like', 'ADG%')
    ->get();
```
- Pertenencia a un array.
```php
$studies = DB::table('studies')
    ->whereIn('code', ['IFC302', 'IFC303'])
    ->get();
```
- Pero existen más posibilidades: whereBetween, whereNotBetween, whereNotIn, whereNull, whereNotNull, whereColumn (compara columnas), whereDate, whereMonth, whereDay, whereYear, ...
- Para estas y otras posibilidades consultar documentación oficial.
#### Multitabla 
- Para unir varias tablas tenemos un juego de métodos _join_.
- Producto cartesiano (CROSS JOIN -> join(...))
```php
$users = DB::table('sizes')
    ->crossJoin('colours')
    ->get();
```
- Reunión interna (INNER JOIN -> join(...))
```php
$users = DB::table('users')
    ->join('contacts', 'users.id', '=', 'contacts.user_id')
    ->join('orders', 'users.id', '=', 'orders.user_id')
    ->select('users.*', 'contacts.phone', 'orders.price')
    ->get();
```
- Reunión externa (LEFT JOIN -> leftJoin(...) y RIGHT JOIN -> rightJoin(...))
```php
$users = DB::table('users')
    ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
    ->get();
```
#### Y mucho más
- Ordenar
```php
$users = DB::table('users') 
    ->orderBy('name', 'desc')
    ->get();
```

- Agrupaciones
```php
$users = DB::table('users')
    >groupBy('account_id')
    ->having('account_id', '>', 100)
    ->get();
//algo más elaborado usando algo de sql puro
->select('department', DB::raw('SUM(price) as total_sales'))
    ->groupBy('department')
    ->havingRaw('SUM(price) > 2500')
    ->get();
```
- Inserciones:
```php
DB::table('users')->insert(
    ['email' => 'john@example.com', 'votes' => 0]
);
//multiregistro
DB::table('users')->insert([
    ['email' => 'taylor@example.com', 'votes' => 0],
    ['email' => 'dayle@example.com', 'votes' => 0]
]);   
//tomando el id autoincremental
$id = DB::table('users')->insertGetId(
    ['email' => 'john@example.com', 'votes' => 0]
);
```
- Actualizaciones
```php
DB::table('users')
    ->where('id', 1)
    ->update(['votes' => 1]);
// pero hay más si quieres investigar:
DB::table('users')->increment('votes');
DB::table('users')->decrement('votes', 5);
```
- Borrados
```php
DB::table('users')->delete(); //todos los registros
DB::table('users')->where('votes', '>', 100)->delete();//usando where
DB::table('users')->truncate();//borra y resetea id autoincrement
```
#### Para terminar: toSql
- Es interesante poder revisar el sql que estamos construyendo:

```php
//en Fluent
$sql = DB::table('studies')
    ->whereIn('code', ['IFC302', 'IFC303'])
    ->toSql();
//...o en Eloquent
$sql = User::where('foo', 'bar')->toSql();
// y después lo vemos
dd($sql);
```

### Raw SQL

- Debemos usar los métodos _select_ y _raw_ de la clase _DB_.

```php
$result = DB::select(DB::raw("SELECT * FROM products WHERE family_id = 3"));
```


### Ejercicios Eloquent sobre la base de datos shop17





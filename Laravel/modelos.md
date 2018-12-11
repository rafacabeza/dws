##  Modelos y bases de datos en Laravel

### Bases de datos
- Para acceder a bases de datos podemos:
 - Usar consultas de Raw Sql (sql en crudo o a pelo).
 - Fluent Query Builder. Un sistema de creación de consultas independiente del gestor de bbdd utilizado.
 - Eloquent. ORM usado por Laravel y que usa una arquitectura Active Record.
 
- Bases de datos compatibles hasta la versión 3:
 - MySQL
 - Postgres
 - SQLite
 - SQL Server
 
- Configuración:
 - La configuración se registra en el fichero _config/database.php_.
 - Puede ser modificada en el _.env_.
- Podemos usar las bases de datos de forma convencional, creadas previamente. Después veremos como crear la estructura de la base de datos desde el propio Laravel. 
 
 ### Modelo
- Las clases modelo extienden una superclase llamada Model de la que heredan numerosos e interesantes métdoso que permiten realizar multitud de operaciones sin escribir una sóla línea de código a través de Eloquent. 
- El nombre del modelo tiene que ser en singular y mayúscula y la tabla a la que acceden estará en plural y minúscula. Por ejemplo: modelo "User" para tabla "users".
- Por defecto busca siempre la llave primaria con el nombre de campo "id".
- Eloquent usa las columnas created_at y updated_at para actualizar automáticamente esos valores de tiempo.
- Artisan nos ayuda a crear los modelos:
 ```
 php artisan make:model Study
 ```
- Si queremos cambiar los parámetros por defecto respecto a nombre de tabla, clave primaria y uso de columnas temporales:

 ```php 
<?php
namespace App;
use Illuminate\Database\Eloquent\Model;
class Study extends Model
{
   protected $table = 'estudio';
   $primaryKey = 'id_estudio';
   public $timestamps = false;
}
``` 
- Veamos un ejemplo de CRUD Study. Vamos a ver el código de las clases Study y de StudyController.

 - Clase modelo. Muy sencillo porque Eloquent nos provee de los métodos que necesitamos.
 
```php
<?php
namespace App;

use Illuminate\Database\Eloquent\Model;

class Study extends Model
{
    //$fillable indica que atributos se deben cargar en asignación masiva.
    protected $fillable = ['code', 'name', 'shortName', 'abreviation'];
}
```

- Clase controladora. Usamos los métodos que nos facilita Eloquent como son:
 - find($id). Busca un registro
 - findOrFail($id). Similar pero lanza excepción si falla. - all(). Carga todos los registros.
 - paginate(). Carga una página de registros. Ver paginación.
 - save(). Guarda el objeto previamente cargado.
 - push(). Guarda el objeto y si hay objtetos relacionados modificados también lo hace.
 - delete(). Borra el objeto previamente cargado.
 - destroy($id). Localiza y borra un registro (la variable tb. podría ser un array de ids).
 
 
```php
<?php
namespace App\Http\Controllers;

use Illuminate\Http\Request; 
use App\Http\Requests\StudyRequest; //usado para validación avanzada
use App\Study; //modelo Study

class StudyController extends Controller
{
    public function index()
    {
        $studies = \App\Study::all(); //toma todos los registros
        return view('study.index', ['studies' => $studies]);
    }
    
    public function create()
    {
        return view('study.create');
    }
    
    public function store(Request $request)
    {
        $this->validate($request, [
            'code' => 'required|unique:studies',
            'abreviation' => 'required|unique:studies',
            'name' => 'required',
            'shortName' => 'required'
            ]);
        $study = new Study($request->all());
        $study->save();
        return redirect('/study');
    }

    public function delete($id)
    {
        // modo 1
        // $study = Study::find($id);
        // $study->delete();
        // modo 2
        Study::destroy($id);
        return redirect('/study'); //redirigir
    }

    public function update($id)
    {
        $study = Study::find($id);
        return view('study.update', ['study' => $study]);
    }

    public function save(StudyRequest $request)
    {
        $study = Study::find($request->id);
        $study->code = $request->code;
        $study->name = $request->name;
        $study->shortName = $request->shortName;
        $study->abreviation = $request->abreviation;
        $study->save();
        return redirect('/study');
    }
}
```




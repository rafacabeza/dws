## Validación

- La validación es una cuestión fundamental en cualquier aplicación, no sólo web. Cualquiera.

- ¿Validar en cliente o en servidor?.
 - En el cliente es más rápido, no necesitamos conectar con el servidor ni hacer uso de la red.
 - Pero nunca debemos confiar en la validación realizada en el cliente. Puede ser burlada intencionadamente o anulada de forma involuntaria.
 - Validar en el cliente es opcional pero en el servidor es obligatorio.
 
### Nota sobre herencia en PHP
- La herencia en lenguajes como Java se limita a clases hijas (Class A extends B) e interfaces (Class A implements B).
- Php incluye el concepto de rasgos o _traits_ en inglés. 
 - Un trait es un fragmento de código que puede ser incluido y reutilizado en múltiples clases.
 - Definición de un trait: 
 ```php
trait ejemploTrait
{
    function funTrait1()
    {
        /* Codigo fun1Trait1 */
    }
    
    function funTrait2()
    {
        /* Codigo fun1Trait2 */
    }
}
```

 - Uso del Trait:
 ```php
class ClaseUsuariaTrait
{
   use ejemploTrait1;
   /*Podemos usar las funciones del trait como propias*/
}
 ```
- Precedencia. ¿Qué ocurre con métodos de igual nombre si una claseHija hereda de una clasePadre y de miTrait? 
 - El trait sobreescribe el de la clase padre.
 - El método de la clase hija sobreescribe a ambos.
 
### Cómo validar en Laravel
- Existen tres métodos fundamentales
 - Usar el trait ValidatesRequests disponible en todos los controladores (por extender la clase Controller). Ver abajo.
 - Crear manualmente un _Validator_ usando la fachada Validator y su método make. Lo dejamos sin explicar.
 - Para las situaciones más complejas podemos usar validación por Request. Permite reutilizar una validación en distintos lugares y separar el código de validación de los controladores. Lo vemos al final de este artículo.
 
### Validando en el controlador (con el _trait_ de validación).
- El trait citado nos brinda la función _validate(Request $request, array rules)_. Ejemplo:
```php
public function store(Request $request)
{
    $this->validate($request, [
    'title' => 'required|unique:posts|max:255',
    'body' => 'required',
    ]);
    //validado ->  a guardar
} 
``` 
### ¿¿Qué pasa tras la validación ??
- Si la validación falla se produce una excepción y se reenvía la petición a la URL previa.
- Se crea un objeto $errors que permite mostrar los errores de validación.
- Los datos del formulario se _flashean_ a la sesión y estarán disponibles sólo en la próxima peticicón para ser volver a rellenar el formulario como estaba.
 
- OJO. Si usamos Ajax se envía una respuesta al cliente con un código 422 y un JSON con los mensajes de erro.
### Rellenando con los datos _viejos_
- La funcion helper _old()_ nos permite acceder a los datos que no se han validado y están en la sesión.
```php
<label>Nombre:</label>
<input type="text" name="name" value="{\{ old('name')}}">
```
### Mostrando los errores con blade
- Todos los errores de la página
```php
@foreach ($errors->all() as $error)
<li>{\{ $error }}</li>
@endforeach
```
- Mostrar los errores de un campo:
```php
//sólo el primero
{\{ $errors->first('name') }}
//o todos los del campo
@foreach($errors->get('name') as $error)
{\{ $error }}
@endforeach 
```

 ### Reglas
- Se pueden añadir múltiples reglas para cada campo. Entre comillas y separadas por la barra vertical. 
- La regla _bail_ hace que no se sigan evaluando más reglas si esa falla.
- Revisar la documentación oficial: Available Validation Rules.

### Validation Request.
- Permite separar código de validación del controlador
- Permite reutilizar el código de validación.

Veamos como haccerlo 
- Creamos una clase Request:
```
php artisan make:request StoreBlogPostRequest
```
- La clase Request cuenta con dos métodos:
 - Método _authorize()_ que permite filtrar si el usuario tiene o no permiso para realizar esa petición.
 - Método _rules()_ donde se define el array de reglas de validación: 
```php
class ProductoRequest extends Request
{
    public function authorize()
    {
        return false;
    }
    public function rules()
    {
        return [
            'title' => 'required|unique:posts|max:255',
            'body' => 'required',
    ];
}
```
- Incluimos el request en el controlador:
```php
use App\Http\Requests\ProductoRequest;
...
public function store(ProductoRequest $request)
{
// al realizar la inyección de dependencias en la función la validación se hace automáticamente.
}
```
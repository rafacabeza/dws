# Sercicios Web. API REST
<!-- Validación en API REST -->
<!-- https://stackoverflow.com/questions/23162617/rest-api-in-laravel-when-validating-the-request -->


Algunos enlaces interesantes:

- [Tutorial Creación de un API RESTful][tutorial]
- [Códigos de error](https://cloud.google.com/storage/docs/json_api/v1/status-codes)
- [Buenas prácticas para el diseño de una API](https://elbauldelprogramador.com/buenas-practicas-para-el-diseno-de-una-api-restful-pragmatica/)
[tutorial]: https://manuais.iessanclemente.net/index.php/LARAVEL_Framework_-_Tutorial_01_-_Creaci%C3%B3n_de_API_RESTful_(actualizado)


## ¿Qué es un Servicio Web?

- De acuerdo a Wikipedia:

>  Un servicio web (en inglés, web service o web services) es una tecnología que utiliza un conjunto de protocolos y estándares que sirven para intercambiar datos entre aplicaciones.


- Se trata de que una aplicación web intercambie información con otra aplicación mediante el protocolo HTTP.
- Durante bastante tiempo se usó XML y el estándar SOAP para esta finalidad.
- Actualmente se usa JSON y el estándar de facto API REST


### ¿Qué es JSON?

- *JavaScript Object Notation*
- Es una sintáxis creada para codificar objetos en JavaScript
- Su uso se ha extendido mucho como alternativa a XML por lo que se considera un formato independiente de dicho lenguaje. 


### ¿Qué es un API?

- *Application Programming Interface*, Interfaz de Programación de Aplicaciones.
- Es decir una interfaz que permite crear aplicaciones que usan determinado recurso.
- Una API web usar el protocolo HTTP para lograr dicho objetivo.


### ¿Qué es un API REST?

- Es una API web que sigue determinado stándar de facto.
- El intercambio de información se basa en el uso de HTTP.
- Usa un protocolo cliente-servidor sin estado. No pueden usarse cookies para mantener el estado de la sesión.
- Cacheable. Una respuesta puede definirse como cacheable para evitar peticiones recurrentes.


- Hipermedia como motor del estado. A partir de una dirección principal pueden obtenerse enlaces a todos los recursos del servicio.
- Se define un conjunto de operaciones: GET, POST, PUT, DELETE.
- Las operaciones CRUD se pueden asimilar al anterior conjunto y a unas rutas determinadas:


| Ruta      | Verbo |  Descripción |
| ----------- | ----------- |
| /studies           | GET  | Obtenemos todos los estudios | 
| /studies           | POST      | Creamos un estudio  | 
| /studies/{id}      | GET  | Obtenemos un estudio  | 
| /studies/{id}      | PUT  | Modificamos un estudio  | 
| /studies/{id}      | PATCH  | Modificación parcial | 
| /studies/{id}      | DELETE  | Borramos un estudio  | 


Se pueden añadir otras rutas para complementar el acceso:

- Podemos definir búsquedas por un campo: 
```
GET /studies?code=IFC303
```

- Podemos definir búsquedas más complejas: 
```
GET /studies/search?family=IFC&level=GS
```

- Podemos definir ordenaciones, en este caso descendente: 
```
GET /studies?sort=-updated_at
```


Y podemos reflejar las relaciones:
```
//Lista de módulos de un estudio
GET /studies/1/modules 
//Busqueda de un módulo dentro de un estudio
GET /studies/1/modules/3
//Creación de un módulo dentro de un estudio
POST /studies/1/modules
//Modificación de un módulo dentro de un estudio
PUT /studies/1/modules/3
//Borrado de un módulo dentro de un estudio
DELETE /studies/1/modules/3
```



## [Postman](https://www.getpostman.com/)

- Utilizaremos [Postman](https://www.getpostman.com/) para testear nuestra API
  - Independiente de que hagamos tests por otro lado
- Es una herramienta muy extendida


### Comprobación API inicial

- Vamos a crear una ruta inicial:

```php
Route::get('/', function() {
    return "Bienvenido a la API Laravel 20";
});
```

- Abre Postman y prueba su funcionamiento.


- El anterior ejemplo funciona pero es más correcto de la siguiente manera:
  - Definimos el código de estado Http
  - Los arrays ordenados se covierten en arrays JSON
  - Los arrays asociativos se convierten en objetos
  - Los objetos se convierten en objetos:

```php
//usar la función response() y el método json para cambiar el status
//el $data que enviamos podría ser cualqueir cosa: objeto, array,...
Route::get('/', function() {
  $data = ['message' => 'Bienvenido a la API'];
  return response()->json($data, 200);
});
```



## Códigos de estado:

  - 200's usados para respuestas con éxito.
  - 300's usados para redirecciones.
  - 400's usados cuando hay algún problema con la petición.
  - 500's usados cuando hay algún problema con el servidor.


- Lista de códigos HTTP que se deberían utilizar en la API RESTful:
  - 200 OK - Respuesta a un exitoso GET, PUT, PATCH o DELETE. Puede ser usado también para un POST que no resulta en una creación.
  - 201 Created – [Creada] Respuesta a un POST que resulta en una creación. Debería ser combinado con un encabezado Location, apuntando a la ubicación del nuevo recurso.
  - 204 No Content – [Sin Contenido] Respuesta a una petición exitosa que no devuelve un body (por ejemplo en una petición DELETE)


  - 304 Not Modified – [No Modificada] Usado cuando el cacheo de encabezados HTTP está activo y el cliente puede usar datos cacheados.


  - 400 Bad Request – [Petición Errónea] La petición está malformada, como por ejemplo, si el contenido no fue bien parseado. El error se debe mostrar también en el JSON de respuesta.
  - 401 Unauthorized – [Desautorizada] No se ha podido autenticar al usuario. Sin credenciales o credenciales no válidas.
  - 403 Forbidden – [Prohibida] Cuando la autenticación es exitosa pero el usuario no tiene permiso al recurso en cuestión.


  - 404 Not Found – [No encontrada] Cuando un recurso se solicita un recurso no existente.
  - 405 Method Not Allowed – [Método no permitido] Cuando un método HTTP que está siendo pedido no está permitido para el usuario autenticado.
  - 409 Conflict - [Conflicto] Cuando hay algún conflicto al procesar una petición, por ejemplo en PATCH, POST o DELETE.
  - 410 Gone – [Retirado] Indica que el recurso en ese endpoint ya no está disponible. Útil como una respuesta en blanco para viejas versiones de la API


  - 415 Unsupported Media Type – [Tipo de contenido no soportado] Si el tipo de contenido que solicita la petición es incorrecto
  - 422 Unprocessable Entity – [Entidad improcesable] Utilizada para errores de validación, o cuando por ejemplo faltan campos en una petición.
  - 429 Too Many Requests – [Demasiadas peticiones] Cuando una petición es rechazada debido a la tasa límite .


  - 500 – Internal Server Error – [Error Interno del servidor] Los desarrolladores de API NO deberían usar este código. En su lugar se debería loguear el fallo y no devolver respuesta.



## CRUD de Estudios en API

- Antes de empezar, prueba otras rutas y fuerza la página de error "No encontrado".
- Obtenemos un error 404 html.
- Mejor un error 404 limpio en JSON. 
- Debemos definir una ruta de *fallback*, o ruta por defecto si ninguna otra es la solicitada

```php
Route::fallback(function () {
  return response()->json(['error' => 'No encontrado'], 404);
});
```


### Controlador y rutas

- Vamos a necesitar un controlador para gestionar nuestro recurso.
- Ojo, vamos separar los controladores API del resto:

```php
php artisan make:controller Api/StudyController
```


- Basta con incluir una ruta resource y añadir el modificador except:

```php

Route::resource('studies', StudyController::class)->except([
    'create', 'edit'
]);
```


### Index

```php
public function index()
{
  return Study::all();  
  //No es lo más correcto porque se devolverían todos los registros. Se recomienda usar Filtros o paginación.
}
```

```php
// Se debería devolver un objeto con una propiedad como mínimo data y el array de resultados en esa propiedad.
// A su vez también es necesario devolver el código HTTP de la respuesta.
public function index()
{
    // return Study::all();
    $studies = Study::all();

    return response()->json(['status' => 'ok', 'data' => $studies], 200);
}
```


### Show

```php
public function show($id)
{
  // Corresponde con la ruta /studies/{study}
  // Buscamos un study por el ID.
  $study=Study::find($id);

  // Chequeamos si encontró o no el study
  if (! $study)
  {
    // Se devuelve un array errors con los errores detectados y código 404
    return response()->json(['errors'=>(['code'=>404,'message'=>'No se encuentra un studio con ese código.'])],404);
  }

  // Devolvemos la información encontrada.
  return response()->json(['status'=>'ok','data'=>$study],200);
}
```


### Create

```php
    public function store(Request $request)
    {
        $validator = Validator::make($request->all(), [
            'code' => 'required|unique:studies,code|max:6',
            'name' => 'required',
            'abreviation' => 'required',
        ]);

        if ($validator->fails()) {
            return response()->json(['errors' => $validator->errors()], 422);            
        }
        
        $new = Study::create($request->all());
        return response()->json($new, 201);
    }
```


### Update: PUT

```php
public function update(Request $request, $id)
{
    $study = Study::find($id);
    //si no se encuentra 404
    if (!$study) {
        return response()->json([
            'status' => 404,
            'message' => 'No se ha encontrado un estudio con ese código'
        ], 404);
    }
    //si no valida 422
    $validator = Validator::make($request->all(), [
        'code' => 'required|unique:studies,code|max:6',
        'name' => 'required',
        'abreviation' => 'required',
    ]);

    if ($validator->fails()) {
        return response()->json(['errors' => $validator->errors()], 422);            
    }

    //todo ok 201
    $study->fill($request->all());
    $study->save();
    return response()->json([
        'status' => 'ok',
        'data' => $study
    ], 200);
}
```


### Delete

- Estamos repitiendo la comprobación de 404 una y otra vez.
- Sería interesante crear un método para no repetir código de esta manera.

```php
//DRY. Don't Repeat Yourself
public function check404($study)
{
    if (!$study) {
        response()->json([
            'status' => 404,
            'message' => 'No se ha encontrado un estudio con ese id'
        ], 404)->send();
        die();
    }
}
```


```php
public function destroy($id)
{
    $study = Study::find($id);
    $this->check404($study);

    try {
        //status 204: No content
        $study->delete();
        return response()->json([
            'Sin contenido'
        ], 204);
    } catch (\Throwable $th) {
        return response()->json([
            'status' => 'error',
            'message' => 'Borrado fallido',
            'error_message' => $th->getMessage()
        ], 409);
    }
}
```



## Autenticación: JWT

- REST se define como sin estado. Esto implica la no utilización de cookies y por ende sesiones.
- **JSON Web Token** es un estándar usado para este fin.
- Cuando un usuario hace login recibe un token que debe guardar en su aplicación cliente.
- En el resto de peticiones debe entregar ese token para ser identificado.


- Para JWT vamos a usar la librería  [tymondesigns/jwt-auth](https://github.com/tymondesigns/jwt-auth)
- Veamos como usarla para:
  - Hacer login
  - Hacer logout
  - Registrar un usuario
  - ...


### Instalación

- Instalar dependencias:

```php
composer require tymon/jwt-auth
```

- Tomar fichero de configuración y crear clave de cifrado

```
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
php artisan jwt:secret  
```

- La clave de cifrado se guarda en .env. Deberemos repetirlo en cada instalación.


- Debemos modificar el fichero *config/auth.php*. Apartado *guards*

```php
  'api' => [
      'driver' => 'jwt',
      'provider' => 'users'   
  ],
```


### Modificar el modelo User

-  Añadimos un "use" en la cabecera

```php
use Tymon\JWTAuth\Contracts\JWTSubject;
```

- Más abajo en la definición de clase, implements ...

```php
class User extends Authenticatable implements JWTSubject
```


- Por último, añadimos estos métodos:

```php
    /**
    * Get the identifier that will be stored in the subject claim of the JWT.
    *
    * @return mixed
    */
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }
    /**
    * Return a key value array, containing any custom claims to be added to the JWT.
    *
    * @return array
    */
    public function getJWTCustomClaims()
    {
        return [];
    }
```


### Rutas y controlador

- Usaremos un controlador. Lo creamos:

```
php artisan make:controller Api/AuthController
```

- Necesitamos unas rutas en *api.php* para los métodos de autenticación:

```php
//En la parte superiror:
use App\Http\Controllers\Api\AuthController;

// rutas con este prefijo: /api/auth/....
Route::group([
    'middleware' => 'api',
    'prefix' => 'auth'
    ], function ($router) {
        Route::post('register', [AuthController::class, 'register']);
        Route::post('login', [AuthController::class, 'login']);
        Route::post('logout', [AuthController::class, 'logout']);
        Route::post('refresh',  [AuthController::class, 'refresh']);
        Route::post('me',  [AuthController::class, 'me']);
});
```


### Métodos del controlador

- Necesitamos añadir algunos "use":

```php
use JWTAuth;
use Auth;
use App\Models\User;
use Illuminate\Support\Facades\Validator;
use Tymon\JWTAuth\Exceptions\JWTException;
```


- DRY. Para enviar el token al cliente usaremos este método:

```php
protected function respondWithToken($token, $status=200)
{        
    return response()->json([
        'access_token' => $token,
        'token_type' => 'bearer',
        'expires_in' => auth('api')->factory()->getTTL() * 60
    ], $status);
}
```


- Veamos ahora los métodos: 
- El constructor con un middleware.

  ```php
  public function __construct()
  {
      $this->middleware('auth:api', ['except' => ['login', 'register']]);
  }
  ```


- Register

  ```php
  public function register(Request $request)
  {
      $validator = Validator::make($request->all(), [
          'name' => 'required|string|max:255',
          'email' => 'required|string|email|max:255|unique:users',
          'password' => 'required|string|min:6|confirmed',
      ]);

      if($validator->fails()){
          return response()->json($validator->errors(), 400);
      };

      $user = User::create([
          'name' => $request->name,
          'email' => $request->email,
          'password' => bcrypt($request->password),
      ]);

      $token = JWTAuth::fromUser($user);
      return response()->json(compact('user','token'), 201);
  }
  ```


- Login

  ```php
  public function login(Request $request)
  {        
      $credentials = $request->only('email', 'password');
      try {
          if ($token = JWTAuth::attempt($credentials)) {
              return $this->respondWithToken($token); //OK
          } else {
              return response()->json(['error' => 'Credenciales inválidas'], 400);                
          }
      } catch (JWTException $e) {
            \Log::error($e->getMessage());
          return response()->json(['error' => 'No se pudo crear el token'], 500);
      }
  }    
  ```


- Logout

  ```php
  public function logout()
  {
      // auth()->logout();
      Auth::logout();

      return response()->json(['message' => 'Salió con éxito']);
  }
  ```


- La información del usuario: me().

  ```php
  public function me()
  {
      return response()->json(Auth::user());
  }
  ```

- Refresh

  ```php
  public function refresh()
  {
      return $this->respondWithToken(JWTAuth::refresh());
  }
  ```

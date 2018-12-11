## Ajax y Laravel
- Cuando usamos Ajax no necesitamos crear páginas completas sino fragmentos (html, json o xml).
- Por una cuestión de tiempo y frecuencia de uso en la actualidad nos ceñiremos a la creación de json.
- Generar json desde php puede realizarse con la función `json_encode()`.
- Con Laravel haremos lo siguiente:
 - Opción 1: Usaremos la función `return response->json()` 
 - Opción 2: Devolvemos nuestro modelo ya que los modelos implementan la interfaz jsonable: `return $miModelo`

```php
<?php

namespace App\Http\Controllers;

(...)
use Illuminate\Http\Response;
use App\Study;

class StudyController extends Controller
{    
    (...)
    public function ajaxlist()
    {
        $studies = \App\Study::all();
        return response()->json($studies); 
        // opcion 2
        // return $studies; 
    }
    (...)
```


<!--
- http://www.w3schools.com/jquery/jquery_ref_ajax.asp
- http://librosweb.es/libro/fundamentos_jquery/capitulo_7/metodos_ajax_de_jquery.html
-->

- Función json(): ` json(string|array $data = array(), int $status = 200, array $headers = array(), int $options) `

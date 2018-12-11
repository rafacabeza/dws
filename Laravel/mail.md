## Envío de mails con Laravel

### Driver
- Podemos usar distintas coniguraciones para el envío de mails. Algunas son exclusivas de entornos de desarrollo.
- Volcado de mails a fichero log:
 - En el fichero .env indica: `MAIL_DRIVER=log`
 - El contenido del email se guardará en `storage/log/laravel.log`
- Uso de [`mailtrap.io`](https://mailtrap.io). Este sitio sirve como servidor smtp de pruebas. Todos los envíos son interceptados y pueden ser estudiados.
 - Accede a su sitio web y abre sesión. 
 - Toma los parámetros y pásalos a tu fichero de entorno `.env`.
 - Ya pueds enviar correos.
- Uso de servidores SMTP para envíos reales.
 - Deben recogerse las credenciales de un usuario de correo en un servidor SMTP. 
 - Si usas una cuenta de Gmail debes habilitarla para ello: https://support.google.com/accounts/answer/6010255?hl=es

### Plantillas
- Los mails se construyen con html y usando plantillas. 
- Se recomienda crear una carpeta llamada `views/emails`
- Es preferible poner los estilos css en la propia plantilla, inline. Si hacemos referencia a un servidor web puede que no estén disponibles.

### Envío básico

- Para enviar un mail de forma básica hay que usar el facade Mail y un closure.
- Ejemplo directamente en el fichero de rutas:
 - La plantilla del email se encuentra en `views/emails/prueba.blade.php`
 
 ```php
Route::get('mail', function () {
    $user = Auth::user();
    echo 'enviamos mensaje.... <br>';
    Mail::send('emails.prueba', ['user' => $user], function ($message) use ($user) {
        $message->from('no-reply@shop17.com', 'Shop17');
        $message->to($user->email, $user->name)->subject('Your Reminder!');
    });
    return 'mensaje enviado';
});
```
- Ejemplo en un controlador.
 - Ruta:
 ```php
 Route::get('mail2', 'MailController@index');
```
 - Código del método index:
```php
public function index()
{
    $user = Auth::user();
    echo 'enviamos mensaje.... <br>';
    Mail::send('emails.prueba', ['user' => $user], function ($message) use ($user) {
        $message->from('no-reply@shop17.com', 'Shop17');
        $message->to($user->email, $user->name)->subject('Your Reminder!');
    });
    return 'mensaje enviado';
}
```
- Todas las posibilidades que acepta el facade Mail son:

```php
Mail::send('vista', 'datos', function () {})
    $message->from($address, $name = null);
    $message->sender($address, $name = null);
    $message->to($address, $name = null);
    $message->cc($address, $name = null);
    $message->bcc($address, $name = null);
    $message->replyTo($address, $name = null);
    $message->subject($subject);
    $message->priority($level);
    $message->attach($pathToFile, array $options = []);
    ```


### Uso de clases Mailable.
 - Se trata de crear una clase específica por cada tipo de mail que vayamos a enviar y una vista.
 - Creación del _mailable_.
 ```
 php artisan make:mail MailOrder
 ```
 - Ejemplo de uso:
 
```php
 namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;
use Illuminate\Contracts\Queue\ShouldQueue;
use App\Order;

class MailOrder extends Mailable
{
    use Queueable, SerializesModels;

    public $order;

    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct(\App\Order $order)
    {
        $this->order = $order;
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->from('admin@shop17.com')
                    ->view('emails.order2');
    }
}
```
 
- Constructor, método __construct(). Las variables públicas que se asignan a esta clase y se asignan en el método constructor están disponibles en la vista sin hacer nada.
- Método build(). En el se construye propiamente el mensaje.

- Invocación y uso de esta clase:

```php
Mail::to($request->user())->send(new MailOrder($order));
```
 
 
 
 
 
 
 
 
 
 
 
 
 
<!--
generar pdf y enviarlo:
$data = array(
    'name' => Input::get('name'),   
    'detail'=>Input::get('detail'),
    'sender' => Input::get('sender')
);

$pdf = PDF::loadView('letters.test', $data);

Mail::send('emails.invoice', $data, function($message) use($pdf)
{
    $message->from('us@example.com', 'Your Name');

    $message->to('foo@example.com')->subject('Invoice');

    $message->attachData($pdf->output(), "invoice.pdf");
});

-->
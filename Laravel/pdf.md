## PFD
- La creación de ficheros pdf en linea es una funcionalidad necesaria en muchas aplicaciones web.
- Existen multitud de librerías para lograrlo en todos los lenguajes.
- Laravel utiliza por defecto la librería DomPdf que genera pdf a partir de código html lo que la hace particularmente fácil de usar.

### Creación de pdf con DomPdf


### generar pdf y enviarlo:

```php
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
```






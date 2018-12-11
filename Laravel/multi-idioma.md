## Aplicación multiidioma

- Laravel viene configurado para soporte multiidioma
- El idioma por defecto se establece en config/app.php: _locale_
- Se definde además un _fallback_locale_ como idioma a usar si el de defecto no tiene la traducción de un elemento.
- Para configurar tu proyecto con el idioma español por defecto:
 - Accede a la configuración y coloca el español como idioma por defecto.
 - Busca en github el zip con todos los idiomas y añade la carpeta de español a tu proyecto (https://github.com/caouecs/Laravel-lang). 
 
 ### Validaciones
 
- Los textos usados por el sistema de validaciones están completamente traducidos.
- Algunos textos hacen referencia al nombre de elementos de la base de datos.
- Para lograr una traducción completa de los textos de validación debemos:
 - Localizar el fichero "resources/lang/es/validation".
 - Añadir la traducción de todos los elementos que necesitemos al final del fichero.
 - Array _attributes_.
 - Formato: _clave => valor_ . Ejemplo `'shortName' => 'nombre corto',`
 
 
 
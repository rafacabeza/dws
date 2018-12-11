## Práctica fin de trimestre.
- Objetivo. Repaso para el examen. Aunque la entrega no es obligatoria hasta el día lunes 11/diciembre, sí sirve para ver las ideas fundamentales que se pueden pedir en el examen.

### Entrega

- Debes crear dos repositorios git llamados: practicaDsPhp y practicaDsJava.- Sube todo el código de tu trabajo.

### Requisitos

- Usa el siguiente SQL para crear una tabla de trabajo.

```sql
CREATE TABLE client(
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR (50) NOT NULL,
    adddress VARCHAR (100),
    phone INTEGER(9),
    credit DOUBLE
    )
```
- Vamos a mantener la lista de clientes.
- Usa el patrón MVC. 
- Funcionalidades y rutas:
 - /login/index Debe mostrar una pantalla con formulario donde se registre el nombre del usuario. El formulario tendrá un action /login/in, no es precisa contraseña ni validación.
 - Todas las vistas deben mostrar en la parte superior el nombre del usuario o el texto "invitado". Junto a invitado debe aparece un texto que sea "login" o "logout" según corresponda con href /login/index o /login/out.
 - /login/out Debe eliminar la sesión y volver a la página de inicio.
 - /client/index/$page Debe mostrar la lista de todos los clientes. La lista debe ir paginada en bloques de 5 en 5. Si el parámetro index no se indica debe mostrar la página 5.
 - /client/new Debe mostrar un formulario de alta. El _action_ del formulario será /client/insert.
 - /client/edit/$id Debe mostrar un formulario de edición de datos. El _action_ del formulario será /client/update.
 - /client/delete/$id Debe borrar un registro de cliente.
  - /client/search Debe mostrar un campo de búsqueda de cliente por nombre. El _action_ del formulario será /client/index Debes modificar el controlador correspondiente para que si se reciben peticiones de búsqueda la lista se ajuste a dicho parámetro. NOTA: si se busca por el texto "ara" deben aparecer todos los clientes cuyo nombre contenga ese fragmento.



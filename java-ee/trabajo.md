# Trabajo para realizar por parejas


## Tarea 1

Partiendo de los conocimientos previos:

- Creación de Servlets y ficheros JSP
- Uso de RequestDispatcher
- Uso de sesiones

### Parte 1
Considera los modelos Author y Book. Cada miembro de la pareja debe ocuparse de uno de ellos. Se trata de:
1. Crear las clases Book y Author dentro del package model. Las generalizamos por {model}.
2. Crea un formulario de alta de cada uno de ellos. Debe responder a la ruta "/{model}/new".
3. Los formularios se envían a la ruta post "/{model}". Al recibir los datos se crea un objeto y se envía como atributo a una vista `jsp`. La vista muestra los datos del registro recibido. Además cada registro debe añadirse a una lista almacenada en variable de sesion.
4. La ruta get /{model} debe mostrar la lista de elementos guardados en sesión
5. NOTA: Todas las vistas se deben construir con html/jsp. No más println() en los servlet.

NOTA: Repaso de rutas:

1. Listado: GET `/{model}`
2. Formulario de alta: GET `/{model}/new`
3. Alta : POST `/{model}`

** Fecha de entrega 22/12/2017 **

### Parte 2

Completa las operaciones CRUD sobre la variable de sesión:

6. Mostrar detalle: GET `/{model}/{id}`
7. Borrar elemento: GET `/{model}/{id}/delete`
8. Formulario de edición: GET `/{model}/{id}/edit`
8. Procesar la edición: POST `/{model}/{id}
9. Finalizar sesión, vaciar listas por tanto: `close`;

** Fecha de entrega 12/1/2018 **


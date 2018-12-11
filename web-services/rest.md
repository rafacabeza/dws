## REST 

http://media.formandome.es/markdownslides/api-rest/export/api-rest-reveal-slides.html


- La arquitectura REST se ha impuesto para construir web services.
 - Significado: REpresentational State Transfer.
 - Los recursos se representan por nombres en plural.
 - Las rutas tienen un sentido semántico.
 - Las acciones se basan en los verbos http: GET, POST, PUT, DELETE. 
 - El resultado se refleja en los códigos http: 200 OK, 404 Not found, ...
- El conjunto de servicios ofrecidos por un servicio web constituye un API (Application Programming Interface)
- Un api que sigue los principios REST (nombre) es un API restful(adjetivo). 
## Rutas y verbos
- Los recursos siempre en plural.
- El verbo HTML usado es determinante para saber que queremos hacer:
 - Lista: GET http://eventos.com/api/eventos/
 - Detalle de un  elemento: GET http://eventos.com/api/eventos/2
 - Alta de elemento: POST http://eventos.com/api/eventos/ 
 - Modificar elemento: PUT http://eventos.com/api/eventos/2
 - Borrar elemento: DELETE http://eventos.com/api/eventos/2
 
 
- Las rutas puedes ser más elaboradas:
 - Lista de comentarios de un evento: GET http://eventos.com/api/eventos/3/comentarios
 - Añadir comentario al evento: POST http://eventos.com/api/eventos/3/comentarios
 
 ## Respuestas
- Errores:
  - 403: Acceso prohibido
  - 404: No encontrado.
  - 500: Error en el servidor
  
- Mensajes de éxito:
  - GET: 200
  - POST: 201. Creado con éxito.
  - DELETE: 200. OK
  - PUT: 200, modificado correctamente. 201, objeto creado con éxito.











## ¿Qué son los servicios web?

- Se trata de una servicio que no genera html para ser visto por un humano.
- El cocmentido es generar información y para recibir datos de otro sistema informático, de otro software: sitio web, applicación móvil, de escritorio, ...

## ¿JSON o XML?

- Un servicio web puede suministrar datos en distintos xml o json.
- Históricamente, xml fue muy importante.
- Actualmente json se impone por sencillez en su generación y por facilidad para ser procesado con javascript.
- Los header _Accept_ de la petición indican que se espera:

```
GET /v1/geocode HTTP/1.1
Host: api.geocod.io
Accept: application/json

*GET /v1/geocode HTTP/1.1
Host: api.geocod.io
Accept: application/xml
```
- Qué usar??:
    - En principio utilizaremos JSON: sencillo de generar y de consumir.
    - XML no es nuestro amigo: schemas, namespaces...
    - Si no es un requerimiento, evitaremos XML









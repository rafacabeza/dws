****# HTTP

Se recomienda leer las páginas 12-19 del [Tutorial Básico de Java EE](http://www.javahispano.org/storage/contenidos/JavaEE.pdf) publicado por JavaHispano

Veamos las cuestiones más notables:

* HTTP es el protocolo usado para la transferencia de recursos en la web.
* El cliente o navegador se denomina "User Agent"
* Se basa en transacciones según el esquema petición-respuesta.
* Los  recursos se identifican por un localizador único: URL
* Es un protocolo sin **estado**, es decir no guarda información entre conexiones. Esto lo hace flexible y escalable pero es un problema para crear aplicaciones web.
* Http requiere de un extra para _recordarnos_ cuando abrimos sesión en una aplicaión. Lo veremos.

## Las URL

El esquema de una URL es:

`protocolo://maquina:puerto/camino/fichero`

* Protocolo es **http** o **https**
* La máquina se identifica por una IP o por un nombre de dominio que será traducido mediante DNS.
* El puerto suele omitirse por usarse los de defecto: 80 y 443 para http y https respectivamente.
* El camino es la ruta relativa al directorio raíz del sitio web.
* Fichero es el nombre del recurso.

## Peticiones HTTP

Las peticiones HTTP siguen la siguiente estructura:

```
Método SP URL SP Versión Http retorno_de_carro
(nombre-cabecera: valor-cabecera (, valor-cabecera)*CRLF)*
Cuerpo del mensaje

nota: 
CRLF es retorno de carro
SP es espacio
```



Veamos un ejemplo

```
GET /index.html HTTP/1.1 
Host: www.sitioweb.com:8080 
User-Agent: Mozilla/5.0 (Windows;en-GB; rv:1.8.0.11) Gecko/20070312 
[Línea en blanco]
```

Las cabeceras aportan gran información. En el ejemplo vemos el  host con el puerto y la identificación del navegador cliente.

Otro ejemplo con el método POST. En el vemos el cuerpo de la petición con el contenido de un formulario.

```
POST 
/en/html/dummy HTTP/1.1 
Host: www.explainth.at 
User-Agent: Mozilla/5.0 (Windows;en-GB; rv:1.8.0.11) Gecko/20070312 Firefox/1.5.0.11 
Accept: text/xml,text/html;q=0.9,text/plain;q=0.8, image/png,*/*;q=0.5 
Accept-Language: en-gb,en;q=0.5 
Accept-Encoding: gzip,deflate 
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7 
Keep-Alive: 300 
Connection: keep-alive 
Referer: http://www.explainth.at/en/misc/httpreq.shtml 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 39 

name=MyName&married=not+single&male=yes 
```

La lista completa de cabeceras que se pueden usar la podemos consultar en [wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields). Se pueden enviar informaciones como:
- El nombre del navegador
- El tipo de contenido solicitado y el aceptado (p. ej. página html, o un pdf, ...).
- El juego de caracteres.
- El idioma preferido.
- Si se admite contenido comprimido ...

El cuerpo del mensaje en las peticiones GET está vacío.

La lista de métodos es bastante amplia pero de momento sólo nos preocupan los métodos GET y POST.

- GET pide un recurso
- POST envía datos al servidor para ser procesados. Los datos se incluyen en el cuerpo del nensaje.

## Respuestas HTTP

La respuesta se ajusta al siguiente esquema:
```
Versión-http espacio código-estado espacio frase-explicación retorno_de_carro
(nombre-cabecera: valor-cabecera ("," valor-cabecera)* CRLF)*
Cuerpo del mensaje
```

Un ejemplo sería

```
HTTP/1.1 200 OK
Content-Type: text/xml; charset=utf-8
Content-Length: 673
<html>
<head> <title> Título de nuestra web </title> </head>
<body>
¡Hola mundo!
</body>
</html>
```


## Códigos de estado

Un elemento importante de la respuesta es el código de estado.

Podemos consultar una lista completa de los posibles estados en [wikipedia](https://es.wikipedia.org/wiki/Anexo:C%C3%B3digos_de_estado_HTTP)
pero aquí tenemos un amplio resumen

- Códigos 1xx : Mensajes
    - 100-111 Conexión rechazada
- Códigos 2xx: Operación realizada con éxito
    - **200 OK**
    - 201-203 Información no oficial
    - 204 Sin Contenido
    - 205 Contenido para recargar
    - 206 Contenido parcial
- Códigos 3xx: Redireción
    - 301 Mudado permanentemente
    - 302 Encontrado
    - 303 Vea otros
    - 304 No modificado
    - 305 Utilice un proxy
    - 307 Redirección temporal
- Códigos 4xx: Error por parte del cliente
    - 400 Solicitud incorrecta
    - 402 Pago requerido
    - **403 Prohibido**
    - **404 No encontrado**
    - 409 Conflicto
    - 410 Ya no disponible
    - 412 Falló precondición
- Códigos 5xx: Error del servidor
    - 500 Error interno
    - 501 No implementado
    - 502 Pasarela incorrecta
    - 503 Servicio nodisponible
    - 504 Tiempo de espera de la pasarela agotado
    - 505 Versión de HTTP no soportada


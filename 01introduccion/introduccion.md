# Introducción

## Modelos de ejecución de código en servidores y clientes web


El desarrollo de aplicaciones web se apoya en la  arquitectura cliente-servidor. En esta arquitectura, existen dos actores, cliente y servidor.

![Cliente Servidor](/assets/cliente_servidor.png)


- El cliente se conecta al servidor para solicitar algun servicio. **Petición *.
- El servidor se encuentra en ejecución de forma ininterrumpida a la espera de que los diferentes clientes realicen una solicitud. **Respuesta *.
- Se suele tratar de obtener el contenido de una página web. También podemos hablar de servicios web donde no se generan páginas web sino contenido en XML o JSON para ser consumido por una aplicación cliente.


- La solicitud que hacen los clientes al servidor se le llama petición (__request__) 
- Lo que el servidor devuelve a dicho cliente le llamamos respuesta (__request__).
- Estos términos son los usados por el protocolo HTTP y los encontrarés en cualquier lenguaje DWS.


- También hay que tener en cuenta que esta arquitectura cliente-servidor plantea la posibilidad de numerosos clientes atendidos por un mismo servidor. El servidor será un software multitarea capaz de atender peticiones simultáneas de numerosos clientes. 
- Cuando una aplicación o servicio web tiene muchas solicitudes por unidad de tiempo puede ser que un conjunto de servidores o _cluster_ desempeñen este servicio en equipo. 


## Páginas estáticas y dinámicas

- Antes de empezar a hablar de distintas tecnologías para la programación en el entorno servidor, debemos comentar algunas cuestiones:

1. Páginas web
2. Dinamismo del lado cliente y del lado servidor


### Páginas estáticas.

- Decimos que una página es estática cuando está construida enteramente con código HTML y CSS, código escrito directamente en ficheros y que con cambia.

- Para visualizar una página estática sólo es preciso un navegador web. No hace falta ningún otro software.


### Páginas dinámicas.

- Decimos que una página es dinámica cuando el contenido y aspecto es cambiante.
- Estos cambios pueden ser fruto de que el servidor modifica el HTML entre peticionees. En este caso hablamos de páginas dinámicas del **lado del servidor**.
- También pueden ocurrir si ejecutamos código en el navegador, javascript. En este caso podemos hablar de páginas dinámicas del **lado del cliente**.


- La tecnología **AJAX**(_Asynchronous JavaScript And XML _) realiza una combinación de ambos dinamisms.
- Permite actualizar el contenido de una página sin recargarla completamente.
- Javascript solicita datos al servidor 
- Los datos recibidos son usados para renovar el contenido de la página web.


## Desarrollo en capas

- Podemos separar funcionalidades.
- Seguimos trabajando con filosofía cliente-servidor.
- Separamos el servidor en dos máquinas.

![](/node/img/software3capas.png)


- Capa de presentación: Es la capa donde la aplicación se muestra al usuario. Básicamente es la GUI (Graphical User Interface, Interfaz Gráfica de Usuario). En el caso de una aplicación web sería el código HTML que se carga directamente en el navegador web. En cualquier caso, se ejecuta directamente en el equipo del cliente.


- Capa de negocio: Es la capa intermedia donde se lleva a cabo toda la lógica de la aplicación. Siempre se ejecutará en el lado servidor. Esta capa, tras realizar todos los cálculos y/o operaciones sobre los datos, genera el código HTML que será presentado al usuario en la capa siguiente.


- Capa de datos: Es la capa que almacena los datos. Básicamente, en condiciones normales, hace referencia al propio SGBD que es el encargado de almacenar los datos. Dependiendo de la arquitectura de la aplicación, esta capa y la de negocio se pueden encontrar fisicamente en el mismo equipo, aunque también es posible que se tengan que separar por cuestiones de rendimiento. La capa de datos sirve todas la información necesaria a la capa de negocio para llevar a cabo sus operaciones.


**Una aplicación web nos encontramos con:**

- Navegador web: Mozilla Firefox, Internet Explorer o Google Chrome...

- Servidor:  acompañado de un lenguaje de programación web permite desarrollar la parte que ocupa la capa de negocio. Un ejemplo habitual es usar Apache + PHP.

- Base de datos: Cualquier SGBD relacional como MySQL o PostgreSQL o sistemas no relacionales como MongoDB.


### Tecnologías de desarrollo servidor

Una vez aclarado esto nos debemos plantear qué tecnología de servidor podemos utilizar:

1. Java EE: Servidor de aplicaciones con JSP y Servlest JAVA.
2. **PHP en conjunción con un servidor web. Tipicamente Apache.**
4. **Node.js. En este caso el servidor es un módulo más de la propia aplicación.**
5. Otras: .Net, CGI, Rubi, Python ...



# HTTP

Se recomienda leer las páginas 12-19 del [Tutorial Básico de Java EE](http://www.javahispano.org/storage/contenidos/JavaEE.pdf) publicado por JavaHispano

Veamos las cuestiones más notables


* HTTP es el protocolo usado para la transferencia de recursos en la web.
* El cliente o navegador se denomina "User Agent"
* Se basa en transacciones según el esquema petición-respuesta.
* Los  recursos se identifican por un localizador único: URL


* Es un protocolo sin **estado**, es decir no guarda información entre conexiones. Esto lo hace flexible y escalable pero es un problema para crear aplicaciones web.
* Http requiere de un extra para _recordarnos_ cuando abrimos sesión en una aplicaión. Lo veremos.


## Las URL

- El esquema de una URL es:
  `protocolo://maquina:puerto/camino/fichero`

* Protocolo es **http** o **https**
* Máquina: una IP o un nombre (DNS).
* El puerto suele omitirse por usarse los de defecto: 80 y 443 para http y https respectivamente.
* Camino o ruta relativa al directorio raíz del sitio
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
El cuerpo suele ir vacío pero no cuando enviamos formularios, subimos ficheros, ...
```


Veamos un ejemplo

```
GET /index.html HTTP/1.1 
Host: www.sitioweb.com:8080 
User-Agent: Mozilla/5.0 (Windows;en-GB; rv:1.8.0.11) Gecko/20070312 
[Línea en blanco]
```


- Las cabeceras aportan gran información. En el ejemplo vemos el  host con el puerto y la identificación del navegador cliente.

- Otro ejemplo con el método POST.

```
POST /en/html/dummy HTTP/1.1 
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


- La lista completa de cabeceras que se pueden usar la podemos consultar en [wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields). Se pueden enviar informaciones como:

  - El nombre del navegador
  - El tipo de contenido solicitado y el aceptado (p. ej. página html, o un pdf, ...).
  - El juego de caracteres.
  - El idioma preferido.
  - Si se admite contenido comprimido ...


- El cuerpo del mensaje en las peticiones GET está vacío.

- La lista de métodos es bastante amplia pero de momento sólo nos preocupan los métodos GET y POST.

  - GET pide un recurso
  - POST envía datos al servidor para ser procesados. Los datos se incluyen en el cuerpo del nensaje.


## Respuestas HTTP

La respuesta se ajusta al siguiente esquema:
```
Versión-http SP código-estado SP frase-explicación retorno_de_carro
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



## Entorno de  trabajo

- Vamos a usar Linux como entorno de trabajo. En concreto una máquina virtual con Linux Mint ya instalado.
- Enlace...


¿Qué herramientas necesitamos?

- Un editor de código.
- Un sistema de control de versiones
- Un sistema gestor de bases de datos (o más)
- Un servidor web con su intérprete (p.ej. php)
- ....


¿Qué tiene nuestra máquina?

- VSC o Microsoft Visual Studio Code como editor de código.
- Git como Sistema de Control de Versiones.
- Docker como entorno de virtualización. 
- Php (y composer).
- Node (y npm).


### ¿Dónde está el servidor web? ¿Dónde el de Bases de Datos?

- Docker es la respuesta.
- Veremo que vamos a virtualizar estos servicios y que esto va a agilizar mucho la gestión de nuestro entrono de trabajo.


## Cómo se ha instalado esta máquina virtual

- Linux Mint 20
- Usuario/contraseña: alumno/alumno
- Guest-Additions dado que estamos virtualizando en Oracle Virtualbox


### Software:

- Git: 
 
```bash
sudo apt-get install git
```
- Prueba que está: 
```bash
git --version
```


- Docker: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-es
- Comprueba: `docker run hello-world`
- Docker-compose: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04
- Comprueba: `docker-compose -v`


- Php y composer
```bash
sudo apt-get install git
```
  - https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-20-04


- Node (+nvm y npm)

```bash
sudo apt-get install nodejs
sudo apt-get install npm
```

- Comprueba: `node -v`
- Comprueba: `npm -v`


- Visual Studio Code: https://code.visualstudio.com/docs/setup/linux
- Lanzarlo: `code`
- Veremos como usarlo, añadir complementos, configuraciones, ...



## Control de versiones con GIT
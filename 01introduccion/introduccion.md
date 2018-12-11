# Introducción

## Modelos de ejecución de código en servidores y clientes web

El desarrollo de aplicaciones web se apoya en la  arquitectura cliente-servidor. En esta arquitectura, existen dos actores, cliente y servidor.

- El cliente se conecta al servidor para solicitar algun servicio.
- El servidor se encuentra en ejecución de forma ininterrumpida a la espera de que los diferentes clientes realicen una solicitud.

En el caso que nos ocupa se suele tratar de obtener el contenido de una página web. También podemos hablar de servicios web donde no se generan páginas web sino contenido en XML o JSON para ser consumido por una aplicación cliente.

La solicitud que hacen los clientes al servidor se le llama petición (__request__) y a lo que el servidor devuelve a dicho cliente le llamamos respuesta (__request__).

Normalmente a la solicitud que hacen los clientes al servidor se le llama petición (request) y a lo que el servidor devuelve a dicho cliente le llamamos respuesta (request).


También hay que tener en cuenta que esta arquitectura cliente-servidor plantea la posibilidad de numerosos clientes atendidos por un mismo servidor. El servidor será un software multitarea capaz de atender peticiones simultáneas de numerosos clientes. 

Cuando una aplicación o servicio web tiene muchas solicitudes por unidad de tiempo puede ser que un conjunto de servidores o _cluster_ desempeñen este servicio en equipo. 

## Páginas estáticas y dinámicas

Antes de empezar a hablar de distintas tecnologías para la programación en el entorno servidor, debemos comentar algunas cuestiones:

1. Páginas web
2. Dinamismo del lado cliente y del lado servidor

### Páginas estáticas.

Decimos que una página es estática cuando está construida enteramente con código HTML y CSS, código escrito directamente en ficheros y que con cambia.

Para visualizar una página estática sólo es preciso un navegador web. No hace falta ningún otro software.

### Páginas dinámicas.

Decimos que una página es dinámica cuando el contenido y aspecto de la misma cambia entre invocaciones. Para que esto ocurra, generalmente, el servidor web ejecuta determinado código que es el responsable del HTML generado y visualizado. En este caso hablamos de páginas dinámicas del **lado del servidor**.

Podemos hablar de páginas dinámicas del **lado del cliente** cuando la página se acompaña de codigo ejecutado por el navegador, normalmente código javaScript.

La tecnología **AJAX** (_Asynchronous JavaScript And XML _) realiza una combinación de ambos dinamismos para que las páginas puedan cambiar sin ser recargadas completamente. Para lograrlo el código javascript solicita datos al servidor que son usados para renovar el contenido de la página web.

Para visualizar una página dinámica se necesita, además de un navegador web, una aplicación que haga las funciones de servidor y que sea capaz de ejecutar el correspondiente código.

## Desarrollo en capas

 Desde un punto de vista de desarrollo una aproximación más detallada para este modelo de ejecución es lo que se conoce como modelo en 3 capas. Es un modelo donde se muestra más en detalle como se distribuye el software que participa en cualquier desarrollo web. Sigue estando presente la arquitectura cliente-servidor (todo se basa en ella) pero aparecen más detalles como el software utilizado en cada uno de los dos actores y como interactúan las diferentes tecnologías o aplicaciones.

Para comprender mejor que es el modelo de desarrollo de 3 capas podemos echar un ojo al siguiente esquema donde se muestra cada una de esas capas y de que se encarga cada una de ellas: 

![](/img/software3capas.png)

- Capa de presentación: Es la capa donde la aplicación se muestra al usuario. Básicamente es la GUI (Graphical User Interface, Interfaz Gráfica de Usuario). En el caso de una aplicación web sería el código HTML que se carga directamente en el navegador web. En cualquier caso, se ejecuta directamente en el equipo del cliente.
    
- Capa de negocio: Es la capa intermedia donde se lleva a cabo toda la lógica de la aplicación. Siempre se ejecutará en el lado servidor. Esta capa, tras realizar todos los cálculos y/o operaciones sobre los datos, genera el código HTML que será presentado al usuario en la capa siguiente.

- Capa de datos: Es la capa que almacena los datos. Básicamente, en condiciones normales, hace referencia al propio SGBD que es el encargado de almacenar los datos. Dependiendo de la arquitectura de la aplicación, esta capa y la de negocio se pueden encontrar fisicamente en el mismo equipo, aunque también es posible que se tengan que separar por cuestiones de rendimiento. La capa de datos sirve todas la información necesaria a la capa de negocio para llevar a cabo sus operaciones.

Una aplicación web nos encontramos con:


- Navegador web: En este caso, Mozilla Firefox, Internet Explorer o Google Chrome podrían ser cualquiera de las aplicaciones que ocuparían esta capa

- Servidor:  acompañado de un lenguaje de programación web permite desarrollar la parte que ocupa la capa de negocio. Un ejemplo habitual es usar Apache + PHP.

- Base de datos: Finalmente en la capa de datos podemos colocar cualquier SGBD, como pueden ser MySQL o PostgreSQL. Actualmente es habitual el uso de sistemas no relacionales como MongoDB.


## Tecnologías de desarrollo servidor

Una vez aclarado esto nos debemos plantear qué tecnología de servidor podemos utilizar:

1. Java EE: Servidor de aplicaciones con JSP y Servlest JAVA.
2. PHP en conjunción con un servidor web. Tipicamente Apache.
4. Node.js. En este caso el servidor es un módulo más de la propia aplicación.
5. Otras: CGI, Rubi, Python...


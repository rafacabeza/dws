# JSP

* Crear HTML en los servlets es muy pesado. 
* JSP es la herramienta adecuada para generar HTML de forma dinámica en un servidor Java EE.
* JSP no es buen sitio para esccribir mucho código Java, aunque sería posible hacerlo.
* Servlets y JSP se complementan: uno realiza la tarea de controlador y el otro de constructor de la vista.
* Para el servidor Java, los ficheros jsp son servlets. La primera vez que se solicita uno de estos ficheros el contenedor los compila y los guardará como un servlet más.

## Despliegue

* Las páginas JSP se ubican en el mismo directorio que las páginas HTML. Esta ubicación es siempre accesible desde su URL.
* Si queremos que nuestras páginas no sean accesibles directamente por su URL deben guardarse en el directorio WEB-INF \(directorio donde se guardan las clases compiladas\).
* No deben reflejarse en el descriptor de despliegue.
* Si están dentro de WEB-INF su acceso será mediante un redireccionamiento.

## Primera página

Veamos la primera página del curso básico de Java EE.

```java
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Hola mundo JSP</title>
</head>
<body>
<h1>Hola mundo JSP</h1>
<p>La fecha actual en el servidor es <%= 2+2 %> </p>
</body>
</html>

```

## Sintáxis JSP

### Comentarios que no llegarán al navegador:

```java
  <%-- Comentario de página JSP-- %>
```

### Declaración de variables y métodos

```java
  <%!... %>
```

Por ejemplo:

```java
<%-- Declaración de variable-- %>
<%! int ultimo = 0;%>

    ...
<%-- Declaración de método-- %>
<%! public String hora() {
    return (new java.util.Date()).toString();
}%>
```

### Scriplets.

* Los scriplets son fragmentos de código que se ejecutan directamente. 
* El HTML y los scriplets son almacenados son copiados al método \_jspService.
* Su sintaxis es:

  ```java
  <%-- Declaración de variable-- %>
  ```

  Por ejemplo

  ```java
  <% ultimo += 10; %>
  ```

  También podemos intercalar códgigo Java y html como en este ejemplo:

  ```java
  <% if (educado) { %>
    <p> ¿Cómo está usted?</p>
  <% } else { %>
    <p> ¿Qué tal?</p>
  <% } %>
  ```


### Etiquetas de expresiones

Muestran el resultado de una expresión java.

```java
  <%= expresión Java %>
```

Es algo similar a hacer un _echo expresion_ en PHP.

### Etiquetas de directivas

```java
  <%@ directiva [atributo= "valor"]* % >
```

* Directivas de página \(_page_\). Veamos algunos ejemplos:

Son modificadores de la página JSP y condicionan la forma en que ésta es traducida a un sevlet.

```java
//importar dependencias
<%@ page import= "java.util.Date" % >
<%@ page import= "java.util.*, java.text,java.sql.Date" % >
//indicar páginas de error
<%@ page errorPage= "GestionaError.jsp" % >
//Acceso a la sesión de usuario
<%@ page session= "false" % >
```

* Directivas include. Para incluir fragmentos html o jsp de otro fichero.

```java
<%@include file="includes/header.jsp" %>
```
Ojo!! Esta directiva realiza la inclusión en tiempo de compilación. Si queremos que la inclusión se realice en tiempo de ejecución deberíamos usar _<jsp:include ....>_ . Veamos un ejemplo:

```java
<jsp:include page="footer.html" />
<jsp:include page="footer.jsp" />

```
## Variables impícitas

Son variables a las que tenemos acceso desde JSP sin declaración previa.

* aplication. Se corresponde al ServletContext.
* config. Se corresponde al ServletConfig.
* exceptcion. Sólo disponible si se define la directiva page _ isErrorPage="true" _ 
* out. Flujo de salida enviado al usuario.
* page. La propia página, equivale a _this_
* PageContext.  
* request. HttpServletRequest del usuario.
* response. HttpServletResponse del usuario. 
* session. HttpSession del usuario.

# JSTL
JSP (Standard Tag Library) es un conjunto de etiquetas organizadas en 4 grupos distintos, las cuales tienen como propósito fundamental evitar que el programador introduzca código Java (scriptlets) en las paginas JSP. 

Mezclar JSP y HTML es desordeando, y pesado. JSTL es simple y mejora la lógica de las vistas.

Los cuatro grupos de librerías JSTL, son: core, fmt, sql y xml.


## Etiquetas core

- Debemos añadir la librería JSTL a nuestro proyecto.
- Debemos añadir la siguiente etiqueta en la cabecera de los ficheros JSP:

```html
<%@taglib uri="http://java.sun.com/jstl/core" prefix="c" %>
```


Disponemos de las siguientes etiquetas:

#### Manipular variables y try...catch

Dichas etiquetas son : `<c:out>, <c:set>, <c:remove> y <c:catch>`

  Ejemplo de  manipulación de variables:        
```html 
<p>
    <!-- Creamos la variable texto con un valor String -->
    <c:set var="texto" value="valor_de_la_variable"/>
    <!-- Mostramos el valor de la variable texto -->
    El valor de la variable <b>texto</b> es : <c:out value="${texto}"/>
        </p>
<p>
    <!-- removemos la variable texto -->
    <c:remove var="texto" scope="page"/>
    <!-- Mostramos nuevamente el valor -->
    El valor de la variable <b>texto</b> ahora es :
    <c:out value="${texto}" default="Es Nulo"/>
</p><pre>
<h1>ddd</h1>
</pre>
```
  Ejemplo try...catch:            

```html
<c:catch var="excepcion">
    <%=3/0%>
</c:catch>
<c:if test="${excepcion != null }">
    Ocurrió una excepción : <c:out value="${excepcion}"/>
</c:if>
```
 
### Condicionales.
`<c:if>, <c:choose>, <c:when> y <c:otherwise>`


 Ejemplo condicional (if, else):

```html
<c:set var="a" value="20"/>
<c:if test="${a > 100}">
    La variable <b></b> es <b>mayor</b> que 100.
</c:if>
<c:if test="${a < 100 }">
    La variable <b></b> es <b>menor</b> que 100.
</c:if>
```
 Ejemplo condicionales múltiples (if, else if, else):

```html

<c:set var="a" value="50"/>
<c:choose>
    <c:when test="${a == 1}">
        <b>a</b> es 1.
    </c:when>
    <c:when test="${a == 2 }">
        <b>a</b> es 2.
    </c:when>
    <c:otherwise>
        <b>a</b> tiene un valor diferente de 1 y de 2.
    </c:otherwise>
</c:choose>
```

### Bucles:
Etiquetas `<c:forEach> y <c:forTokens>`

 Ejemplo para recorrer una lista:
 
```html
<p>
    <c:forEach var="nombre" items="${listaDeNombres}">
    <b><c:out value="${nombre}"/></b> <br/>
</c:forEach>
</p>
<!-- O con objetos -->
<p>
    <c:forEach var="objeto" items="${objetos}">
    <b><c:out value="${objeto.atributo}"/></b> <br/>
</c:forEach>
</p>
```
### Etiquetas de importación y mnipulación de URL's

Etiquetas: `<c:import>, <c:url>, <c:redirect> y <c:param>`

```html
 <c:import url="nuevaJSP.jsp"/>
```

Las otras tres sirven para configurar mostrar urls con parámetros y par forzar el reenvío a otras direcciones con parámetros:

```html
<!-- reenvio a la url nuevaJSP.jsp?nombre=Juan  -->
<c:redirect url="nuevaJSP.jsp">
    <c:param name="nombre" value="Juan"/>
</c:redirect>
```

## Etiquetas de formato: fmt
- Debemos añadir al inicio de nuestro jsp la siguiente etiqueta:

```html
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
```

- Para formatear un número usamos formatNumber con los atributos number para el número y pattern para el patrón de formateo a aplicar. 

```html
<fmt:formatNumber value="1000.001" pattern="#,#00.0#"/> 
```  

- Si queremos parsear un número a partir de un string usamos parseNumber:
```html
<fmt:parseNumber value="${currencyInput}" type="currency" var="parsedNumber"/> 
```  

- Para formatear una fecha usamos formatDate y para parsear un string parseDate:
```html
<jsp:useBean id="now" class="java.util.Date" />
<fmt:formatDate value="${now}" timeStyle="long" dateStyle="long"/> 
<fmt:formatDate value="${now}"  pattern="MM dd, YYYY" /> 
<fmt:parseDate value="${dateInput}" pattern="MM dd, YYYY" />
```  
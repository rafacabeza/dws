# Spring

Spring es un framework Java creado por Rod Johnson. Es un framework muy potente basado en inversión de control para ayudar a desacoplar los componentes de un proyecto.

Vamos a ver como usar Spring paso a paso en poyectos Java EE. Lo vamos a usar en combinación con Maven como gestor de dependencias y JPA como framework ORM, mapeo objeto-relacional.

Todo lo explicado en este texto está reflejado en el proyecto https://bitbucket.org/rafacabeza/shopspring, úsalo para seguir el estudio del mismo

## Enlaces interesantes.

- https://docs.spring.io/spring/docs/current/spring-framework-reference/
- https://www.uv.es/grimo/teaching/SpringMVCv3PasoAPaso/part1.html
- http://simplej2eetutorials.blogspot.com.es/2014/05/spring-mvc-tutorial.html
- https://www.udemy.com/spring-framework-desarrollo-web-spring-mvc/?couponCode=SPRINGYTCHANNEL
- https://www.youtube.com/playlist?list=PL5leSwLYapPZh9e1Mlo9u0HkIufl8RFWF

### Entorno.
Vamos a usar STS, un paquete basado en Eclipse y con todas los elementos necesarios para usar Spring.

- Descargamos STS (Spring Tool Suite)
- Descomprimimos e iniciamos.

### Proyecto Web Dinámico
Vamos a crear un proyecto java y vamos a completarlo para que sea un proyecto Web dinámico que use Spring:

- Creación de proyecto base.
 - new project -> maven basico
   - Group id: `com.rafacabeza`
   - Artifact id: `shop`
- Convertirlo en proyecto web
 - Propiedades facets:
   - Añadimos Dynamic Web Module para convertirlo en un web project
   - Further configuration: crear web.xml y si queremos cambiamos la ruta del WebContent
   - OK
      
- Hay que cambiar el JRE (Properties > Java Build Path > Libraries > Add Library : JRE System Library > Java 1.8)
- Hay que añadir las librerías del servidor, si no no conoce la clase servlet
- ¡Ya funciona! (pero sin spring)

Podemos probar a ejecutar el proyecto sobre el servidor de aplicaciones.

### Proyecto Spring MVC

Código disponible en: https://bitbucket.org/rafacabeza/shopspring

#### Añadir Spring al proyecto:

Debemos añadir la dependencia en el pom.xml, fichero de Maven.
     
    ```
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.0.0.RELEASE</version>
        </dependency>
    ```
- Observa en el arbol del proyecto las dependencias descargadas.
- Es necesario configurar el project para que al desplegar se incluyan las dependencias maven:
  -  propiedades > deployment assembly > add/añadir, buscar las dependencias (Java Build Path Entries) y acabar
    
    > Tag v1.0: git checkout v1.0
    
#### Controlador frontal
- Con Spring ya disponemos de un Controlador Frontal por el que pasan todas las peticiones. 

![](/img/java/mvc.jpg)


- Debemos indicar su uso en el descriptor de despliege, _web.xml_.

```xml

<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xmlns="http://java.sun.com/xml/ns/javaee"
 xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
 id="WebApp_ID" version="3.0">
 <display-name>Spring MVC Test</display-name>
 <servlet>
  <servlet-name>dispatcher</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <load-on-startup>1</load-on-startup>
 </servlet>
 <servlet-mapping>
  <servlet-name>dispatcher</servlet-name>
  <url-pattern>/</url-pattern>
 </servlet-mapping>
</web-app>

```

En este xml indicamos que:
 - El controlador frontal es un objeto de la clase `org.springframework.web.servlet.DispatcherServlet`, que vamos a crear un objeto de dicha clase en el inicio y que su nombre es "dispatcher".

- El controlador frontal debe configurarse via xml. 
- El fichero de configuración se debe llamar, por defecto, `nombreservlet-servlet.xml` y ubicarse junto al `web.xml`. En nuestro caso `dispatcher-servlet.xml'.

- Podemos cambiar el nombre y ubicación del xml, por ejemplo:

```xml

<servlet>
    <servlet-name>Spring MVC Dispatcher Servlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/config/web-application-config.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
		
        
```


### Configuración del servidor frontal

- Debe hacerse en un fichero llamado `dispatcher-servlet.xml`.
- Existe un asistente: Sobre el directorio destino, botón derecho, new > Spring Bean Configuration File.
- Le damos nombre y debemos añadir tres espacions de nombres necesarios par nuestro proyecto: beans, context y mvc.

- Hay que añadir unas líneas de xml que configuran varias cosas:
 - context: hace referencia a la ubicación de los controladores
 - mvc: ahí indicamos que vamos a configurar nuestros controladores via anotaciones en las clases afectadas.
 - bean: ahí configuramos dónde se guardan los ficheros de vistas y cual es su extensión.
 
- Tu fichero debería quedar algo así:

![](/img/java/servletxml.png)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd">


	<context:component-scan base-package="com.rafacabeza.shop.controller"></context:component-scan>
	<mvc:annotation-driven />
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/views/">
		</property>
		<property name="suffix" value=".jsp"></property>
	</bean>

</beans>
```

### Primera controlador y primea vista 


- Creamos el paquete indicado en el `dispatcher-servlet.xml`: `com.rafacabeza.controller`.

- Creamos una clase básica Java en el mismo: `HomeController`.

- Le añadimos un método, por ejemplo `home` y añadimos las anotaciones necesarias:
 - Controller para indicar que es un controlador
 - RequestMapping para indicar la ruta a la que atiende el método
 - Devuelve un texto que es el nommbre de la vista: `home`
 - De acuerdo a la configuración eso devolverá un fichero `/WEB-INF/views/home.jps` que debemos crear.

```java 

@Controller
public class HomeController {
    @RequestMapping(value = "/home", method = RequestMethod.GET)
	public String goHome()
	{
	   return "home";
	} 
}
```

#### Pasar datos a las vistas

- Uso de Model
- Uso de EL (Expression Language):
```java
${ variable }
${ objeto.atributo }
```

> Tag v1.1: git checkout v1.1

#### Pasar una lista a la vista

- Uso de JSTL
- En el pom.xml

```
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
```
- En el JSP:

```java

<%@ taglib prefix = "c" uri = "http://java.sun.com/jsp/jstl/core" %>
```

> Tag v1.2: git checkout v1.2


#### Manejar tipos más complejos: clases Modelo

- Crear paquete `com.rafacabeza.shop.model`
- Crear clase Article
- Añadimos los atributos privados
- Generamos el constructor, los getters/setters y toString
- Vamos a modificar nuestro código para que muestre una tabla de artículos con todos sus atributos

> Tag v1.3: git checkout v1.3

### Recursos
Para la configuración de recursos estáticos que se usen en nuestra aplicación web debemos hacer lo siguiente: 

1. Crear carpeta "resources" donde se almacenarán los recursos estáticos. Por ejemplo:
 - css: otros archivos CSS
 - images: archivos de imagen (png, jpg, etc).
 - js: nuestros archivos con funciones Javascript.
 
2. Agregar la siguiente configuración al archivo XML del DispatcherServlet.
```html 
<mvc:resources mapping="/resources/**" location="/resources/" />
```
 La ubicación física la indica "mapping" y "location" indica la ruta que debemos usar en nuestras url's. Los recursos no se cachean en el navegador salvo que añadamos el atributo cache-period, unidades segundos:
 ```html 
<mvc:resources mapping="/resources/**" location="/resources/" cache-period="10000" />
```
3. Incluir el tag library de Spring en el archivo JSP.	
```html 
<%@taglib uri="http://www.springframework.org/tags" prefix="spring"%>
```	
4. Agregar una variable al modelo con la URL relativa a resources.
```html 
<spring:url value="/resources" var="urlPublic" />
```
5. Utilizar la variable antes creada para acceder a los archivos estáticos.

```html 
<link href="${urlPublic}/css/myStyle.css" rel="stylesheet">
<img src="${urlPublic}/images/cinema.png">
```

> Tag v1.4: git checkout v1.4


### Controladores y rutas dinámicas y con parámetros:

- Ejemplo de URL dinámica:
```html
<a href="detail/${article.id}">Consulta Artículo</a>
```
- **@PathVariable**. Una URL dinámica debe tratarse así:
```java
@RequestMapping(value = "/article/{id}",method=RequestMethod.GET)
public String mostrarDetalle(@PathVariable("id") int idArticle){
System.out.println("PathVariable: " + idArticle);
return "detalle";
}
```

- Otra forma de dinamismo es a través de parámetros:
```html
<a href="detalle?idMovie=${article.id}">Consulta Horarios</a>
```
- **@RequestParam**. En este caso debemos tomar los parámetros así:
```java
@RequestMapping(value = "/detalle", method=RequestMethod.GET)
public String verDetalle(@RequestParam("idArticle") int id){
// Procesamiento del parámetro. Aquí, ya se hizo la conversión a String a int.
System.out.println("RequestParam: " + id);
return "someView";
}
```
Podemos indicar que un parámetro es obligatorio así `@RequestParam(name="id",required=false))`



### RequestMapping 

Apartir de la versión 4.3 Spring agregó variaciones de la anotacion @RequestMapping
Adecuadas para aplicaciones web estándar:
- @GetMapping
- @PostMapping
Adecuadas para REST Web Services:
- @PutMapping
- @DeleteMapping
- @PatchMapping



```java
@Controller
public class NoticiasController {
	//@RequestMapping(value="/create",method=RequestMethod.GET)
	@GetMapping(value="/create")
		public String crear() {
	    return "noticias/formNoticia";
	}
	//@RequestMapping(value="/save",method=RequestMethod.POST)
	@PostMapping(value="/save")
	public String guardar(@RequestParam("titulo") String titulo) {
	    System.out.println("Guardando : " + titulo);
	    return "noticias/formNoticia";
	}
	//@RequestMapping(value="/update/{id}",method=RequestMethod.GET)
	@GetMapping(value="/update/{id}")
	public String actualizar(@PathVariable("id") int idNoticia) {
		System.out.println("Actualizando: " + idNoticia);
	    return "noticias/formNoticia";
	}
}
```

> Tag v1.5: git checkout v1.5

## Un poco de diseño y de orden:

- Vamos a añadir Bootstrap a nuestro proyecto vía CDN.

- Usa la siguiente plantilla para reconstruir tu `/home`

> Tag v1.6: git checkout v1.6



- Vamos a usar includesi para colocar partes repetitivas de las vistas en ficheros separados
 
 - Separa el código html de los elementos cabecera, menú y pie a ficheros separados en un directorio WEB-INF/includes.
 - Usa la etiqueta 
 ```
 <jsp:include page="../includes/header.jsp"></jsp:include>

```


> Tag v1.7: git checkout v1.7



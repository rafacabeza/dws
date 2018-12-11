# Root Context 

¿Qué es un Root ApplicationContext?
- Tiene la declaración de los componentes que pueden ser utilizados en toda la aplicación: 
 - Componentes de lógica de negocio
 - Componentes de acceso a datos

- Los componentes se declaran como _Beans_ en en un archivo XML
 
- Solo hay un Root ApplicationContext por cada aplicación web.
- A diferencia de un WebApplicationContext que pueden ser varios (uno por cada DispatcherServlet declarado).
- El Root ApplicationContext es inicializado por un Listener (ContextLoaderListener) definido en el archivo web.xml de nuestra aplicación.
- Si no es especificado el nombre del archivo de configuración del Root ApplicationContext, Spring por default buscará un archivo llamado applicationContext.xml en el directorio WEB-INF.
```xml
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```
- Podemos definir otra ubicación y nombre del xml:

```xml
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/spring/root-context.xml</param-value>
</context-param>
```
- El Root WebApplicationContext tiene componentes que pueden ser usados para toda la aplicación:
 - Componentes de Servicio
 - Componentes de Datos

- Los objetos del WebApplicationContext pueden acceder al RootApplicationContext.

## Configurar el listener

- Vamos al web.xml
- Ctrl + Espacio. En el desplegable, abajo del todo, seleccionamos ContextLoaderListener

- Debemos dejar/quitar el "param" de ubicación. Si lo dejamos hay que adecuarlo a nuestro caso. En caso contrario, usar el nombre por defecto (applicationContext.xml en el directorio WEB-INF).

```xml
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>/WEB-INF/root-context.xml</param-value>
</context-param>
```

- Ahora creamos el fichero root-context.xml. 
 - Menú "nuevo > Spring Bean Configuration File"
 - Nombre y ubicación correspondiente
 - Añadimos namespaces de "beans" y "context"
 
 
 ## Añadir un componente de acceso a datos (sin BBDD)

Vamos a ver como crear clases que nos provean servicios y que sean inyectadas _automáticamente_ en nuestros controladores.

- Creamos un paquete: _com.rafacabeza.shop.service_ .
- Le indicamos a nuestro  Root Context que sea capaz de cargar todos los servicios incluidos en ese paquete. Para hacerlo añadimos el siguiente _bean_:

```xml
<context:component-scan base-package="com.rafacabeza.shop.service"></context:component-scan>

```
- Vamos a crear un interfaz y una clase dentro de ese paquete.
- Para facilitar distintas implementaciones vamos a definir un interfaz: IArticleService. Le añadimos un método _all()_ que devuelva la lista de artículos.
- Vamos a crear una clase que nos provea la lista de artículos: ArticleServiceImplementation:

```java
package com.rafacabeza.shop.service;

import java.util.ArrayList;
import com.rafacabeza.shop.model.Article;

public interface IArticleService {
	ArrayList<Article> all();
}

```

- Vamos a crear una clase que nos provea la lista de artículos: ArticleServiceImplementation. 
   
>   ¡¡ IMPORTANTE!! La etiqueta `@Service` es la responsable de la carga automatica de los servicios. Ojo! etiqueta en la Clase.

```java
@Service
public class ArticleServiceImplementation implements IArticleService {

    private ArrayList<Article> articles = null;		

    public ArticleServiceImplementation() {
        System.out.println("creando servicio de articulos");
        articles = new ArrayList();		
        articles.add(new Article(1, "ART1", "articulo 1", 315));
        articles.add(new Article(2, "ART2", "articulo 2", 325));
        articles.add(new Article(3, "ART3", "articulo 3", 341));
        articles.add(new Article(4, "ART4", "articulo 4", 35));
        articles.add(new Article(5, "ART5", "articulo 5", 225));
    };
    @Override
    public ArrayList<Article> all() {
       return articles;
    }
}
```

- Por último vamos a usar este servicio en nuestro controlador:

> ¡¡IMPORTANTE!! La etiqueta `@Autowired` es la responsable de completar la carga automática del servicio ArticleService. Ojo!, que la variable la hemos declarado sobre la interfaz.

 
```java

public class ArticleController {
    @Autowired
    private IArticleService articleService;

    @RequestMapping(value = "/articles", method = RequestMethod.GET)
    public String articles(Model model, @RequestParam(value = "page", required = false) Integer page) {
        ArrayList<Article> articles = articleService.all();
        model.addAttribute("articles", articles);
        return "article/index";
    }	
}	

```
 > Tag v1.8: git checkout v1.8, lista de artículos con _all()_
> Tag v1.9: git checkout v1.9, detalle de artículos con _find(id)_



- Usamos el Interfaz porque permitiría cambiar el servicio de persistencia sin alterar nuestro código. No obstante si usasemos la clase ArticleServiceImplementation sin herencia del interfaz todo funcionaría igual. (Tarea para el alumno)
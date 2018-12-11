# Servlets

Un servlet es una clase Java que amplía las capacidades de un servidor. Generlmente se usan para aplicaciones alojadas en servidores web.

El servidor de aplicaciones se asemeja a un sistema operativo. Controla la ejecución de aplicaciones, la asignación de memoria, CPU, ... El servidor de aplicaciones recibel la petición HTTP, almacena la información en objetos Java. Si hay algún programa asignacio a la URL, se invocará dicho programa.

## Directorios (Netbeans)

* Directorio **build**. Guarda el resultado del código compilado. 
* El dirctorio **dist** guarda la aplicación lista para ser distribuida e implantada.
* El directorio **doc** guarda el resultado del javadoc.
* El dirctorio **lib** guarda librerías de terceros.
* El directorio **setup** guarda ficheros de configuración.
* El directorio **src** guarda el código fuente. Su contenido se estructura en subdirectorios de acuerdo a los distintos paquetes usados.
* El directorio **web **es el fundamental. En él se aloja toda la aplicación web y su contenido será copiado al directorio dist.

## Nuestro primer servlet

* Java EE se puede ver como un contenedor de servlets.
* A Java EE le decimos qué ruta en la URL se asocia a la ejecución de un servlet

  * Podemos hacerlo en el propio Servlet con una anotación \(@WebServlet ...\).
  * O podemos hacerlo en el fichero web.xml conocido como descriptor de despliegue.

* En general, los servlets son clases que extienden la clase HttpServlet.

* Entre otros, HttpServlet define los métodos doGet y doPost. Estos métodos son ejecutados cuando se requiere el servlet desde ls métodos get y post. 

### Descripción en _web.xml_
TODO:

### Descripción con anotaciones
TODO:

#### Ejercicio 1

Vamos a crear un proyecto en Netbeans para observar lo comentado hasta aquí:

* Crea un proyecto Java Web. Por ejemplo Servlet1.
* Añade un Servlet que puedes llamar Hola. No añadas la ruta en el descriptor. Analiza el código.

  * Métodos doGet y doPost.
  * Analiza y haz pruebas en la anotación \(@WebServlet...\)

* Añade otro Servlet que puedes llamar Adios. Añade la ruta en el descriptor web.xml. Observa la diferencia con el anterior.


## Formularios

Cuando la petición llegue al servidor, tanto si se envía mediante el método GET como
 mediante el método POST, el contenedor de Servlets recogerá todos los parámetros del formulario y los guardará en el objeto HttpServletRequest.

Podemos acceder a ellos a través del método **String getParameter\(String\) ** de dicho objeto. El parámetro de este método es el valor del atributo "name" de cada uno de los campos del formulario.

```java
//tomar el parámetro "name"
//málido para GET y POST
//si "name" no existe tomará null
String name = request.getParameter("name");
```

Si un formulario cuenta con varios elementos con el mismo atributo "name", lo que recibimos es una array. En este caso debemos usar el método **String [] getParameterValues (String)**.

```html
Aficiones:<br>
Deporte <input type="checkbox" name="aficiones" value="deporte"><br>
Lectura <input type="checkbox" name="aficiones" value="lectura"><br>
Música <input type="checkbox" name="aficiones" value="musica"><br>
```
OJO: el name de los controles es el mismo "aficiones" pero diferente a lo que necesita php que sería "aficiones[]"

```java
String[] aficiones = request.getParameterValues("aficiones");
for(String aficion : aficiones)
{
  out.println("<br>" + aficion); //salida html
  System.out.println(aficion);  //salida a consola
}        
```

Además podemos obtener la lista de todos los parámetros recibidos:


#### Ejercicio 2

Crea un fichero ejercicio01.html. En el primero construye un formulario para el registro de libros en una biblioteca. En él usa todos los tipos básicos de input: text, password, select, option, checkbox y submit. 
Crea un servlet que debe mostrar por pantalla los valores devueltos. Prueba con los métodos GET y POST.

## Ciclo de vida

El ciclo de vida de un servlet es controlado por el contenedor de servlets, es decir por nuestro servidor de aplicaciones \(Glasfish, Tomcat, ...\).

Las fases del ciclo de vida son:

* Antes de poder atender ninguna petición el servlet debe ser iniciado. El contender lo inicia ejecutando su método _init\(\)_. Nosotros podemos reescribir este método.
* Tras iniciarlo, el servlet está en memoria y puede atender múltiples peticiones en paralelo. Cada petición se atiende ejecutando el método _service\(\)_. De ello se ocupa el contendor que es quien lanza los métods doGet o doPost. No es habitual sobreescribir este método.
* Cuando el contenedor elmina un servlet se ejecuta el método _destroy\(\)_. También podemos reescribirlo para realizar determinadas tareas.
* El recolector de basura lo elimina de memoria de la JVM.

### Parámetros de inicialización.
Uno de los usos del método *init()* es el acceso a parámetros de inicialización. 

Como sabemos, Java requiere de compilación. Podemos añadir parámetros de configuración de cada servlet en el descriptor de despliegue, además de en el propio código del servlet. Esta información se puede cargar antes de servir ninguna petición mediante el citado método. Veamos cómo:

- Crear los parámetros en el código

```java
@WebServlet(name="ConfigurableServlet",
urlPatterns={"/ConfigurableServlet"},
initParams={@WebInitParam(name="parametro1", value="
```

- O en el descriptor de despliegue, *web.xml*.

```xml
<servlet>
    <servlet-name>ConfigurableServlet</servlet-name>
    <servlet-class>tema5.ConfigurableServlet</servlet-class>
    <init-param>
        <param-name>parametro1</param-name>
        <param-value>Valor1</param-value>
    </init-param>
    ...
</servlet>
```
- Y como cargar dichos valores al iniciar el servlet. Esta información se guarda en el objeto ServletConfig que podemos acceder en el método *init()*.
```java
@Override
public void init(ServletConfig config){
    Enumeration<String> nombresParametros = config.getInitParameterNames();
    while (nombresParametros.hasMoreElements()) {
        String nombreParametro = nombresParametros.nextElement();
        mapaDeParametrosDeConfiguracion.put(nombreParametro, config.getInitParameter(nombreParametro));
    }
}
```

## Compartir informacion entre servlets.

Existen varios mecanismos que podemos usar en distintas circunstancias para compartir información entre servlets.

- **Cookies**. Las cookies son elementos de información que almacena el navegador del usaurio. Son elementos de texto que siguen el patrón (clave, valor). Sólo se puede almacenar texto. La duración de la cookie es controlada por el servidor, por nuestro código, aunque el usuario puede alterar tanto la duración como el contenido de la misma.

Cada cookie se asocia a un servidor y a un directorio. 
En posteriores conexiones, si el cliente tiene alguna cookie asociada al servidor la enviará junto a su petición, siempre y cuando se acceda al mismo directorio o sus subdirectorios.

- **Sesiones**. Una sesión es un objeto java ligado a un usuario concreto en el que podemos guardar información de cualquier tipo. Sigue el esquema (clave, valor) pero podemos guardar cualquier objeto Java. Se almacena en memoria RAM.

- **Contexto**. El contexto es un elemento asociado al contenedor de servlets. Un servlet puede añadirle elementos de información (parámetros) y todos los demás pueden acceder a ellos, sean usados por el mismo o por otros usuarios. Eso sí, siempre en el mismo contenedor. Este mecanismo no es válido si nuestra aplicación corre en un cluster de servidores.

- **HttpRequest**. Igualmente podemos añadir elementos al HttpRequest. Esto es útil para transferir el control de un servlet a otro, como veremos, y que el request vaya acumulando los datos que nos interesen. Estos datos duran mientras se atiende el HttpRequest, una vez atendido se pierden.

### Cookies

Recordamos que una cookie tiene un nombre y valor asociados. Además tiene un tiempo de vida, siempre en segundos, y un path o ruta del servidor a la cual el navegador debe devolver la cookie.

Veamos cómo se crean las cookies y como se trabaja con ellas. Podemos:
* Añadir cookies al objeto response:
```java
void addCookie(Cookie) //del objeto HttpServletResponse.
```

* Recuperar las cookies del objeto request:
```java 
Cookie[] getCookies() //del objeto HttpServletRequest 
```

Y esos son los métodos asociados a una cookie:
* Cookie(String name, String value). Método constructor de la clase Cookie.
* int getMaxAge(). Devuelve la edad máxima en segundos.
* String getName(). Devuelve el nombre de la cookie.
· String getPath(). Devuelve la ruta del servidor.
· String getValue(): devuelve el valor actual de la cookie.
· void setMaxAge(int expiry): establece la edad máxima de la cookie. Si el valor es negativo la cookie será eliminada cuando el navegador web se cierre. 
· void setPath(String uri). Especifica la ruta o path.
· void setValue(String newValue). Cambia el valor de la cookie.

#### Ejercicio 3
Crea un formulario con los inputs usuario, contraseña, un submit y un checkbox llamado recordar. Tras hacer submit el servlet debe mostrar el usuario y contraseña. Además, si *recordar* está activado debemos guardar en una cookie el nombre del usuario y, en próximos accesos, mostrar el formulario con el input ya relleno. La cookie debe mantenerse durante 2min.



### Sesiones

Las sesiones son un objeto asociado a un usuario y un contenedor de servlets. Su tiempo de vida se establece en el descriptor de despliegue. 
```xml
<session-config>
    <session-timeout>
        30
    </session-timeout>
</session-config>
```

La sesión se guarda en RAM. Si se usa un cluster de servidores, las sesiones pueden ser transferidad por red entre los servidores. La sesión en este caso debe ser serializable.

El HttpServletRequest nos brinda el método sobrecargado getSession
- HttpSession getSession(). Devuelve la sesión actual y si no existe la crea.
- HttpSession getSession(boolean create). Idem pero si el argumento pasado es flse no creará una nueva sesión.


Por otra parte el objeto HttpSession brinda un juego de métodos entre los qe destacamos los siguientes:

- Object getAttribute(String name). Lee un atributo.
- java.util.Enumeration getAttributeNames(). Carga la lista de atributos.
- ServletContext getServletContext(). Devuelve el ServletContext al cual - pertenece esta sesión.
- void invalidate(). Invalida la sesión y elimina los objetos almacenados.
- void removeAttribute(String name). Elimina un atributo.
- void setAttribute(String name, Object value). Añade un atributo.

Existen métodos para consultar y establecer los tiempos de duración y acceso de  la sesión, su identificador (*id*), ...


### Contexto

El objeto que guarda la información de contexto se llama ServletContext. Se puede acceder a este objeto desde el ServletConfig, disponible en el *init()* del servlet. También se puede acceder desde el objeto HttpSession o en el propio HttpRequest.

Para hacerlo debemos usar el método *ServletContext getServletContext()*. En él encontramos métodos para leer, añadir o eliminar atributos del contexto:
- Object getAttribute(String name).
- java.util.Enumeration<String> getAttributeNames().
- void removeAttribute(String name).
- void setAttribute(String name, Object object).

También se puede acceder a los parámetros de inicio:
- String getInitParameter(String name).
- java.util.Enumeration<String> getInitParameterNames().
- boolean setInitParameter(String name, String value).


### HttpRequest

> Ver: https://bitbucket.org/daw2rafa/introjavaee, rama dispatching

Veamos cómo se añaden atributos al HttpRequest y como se pasa esa información a otros elementos. 
- Para añadir información se usa el método *setAttribute(nombre, valor)*
- A continuación se pasa el control al nuevo recurso que puede ser un servlet pero podría ser una página html o jsp. Para hacerlo se requiere un objeto RequestDispatcher que es el que permite el reenvío (*forward*).

El dispatcher lo obtenemos de tres modos distintos, uno a traves del propio request, y los otros dos desde el context (el objeto aplicación).
- RequestDispatcher request.getRequestDispatcher(String name). `name` se refiere a la url relativa o absoluta a la que responde el servlet destino.
- RequestDispatcher context.getRequestDispatcher(String name). En este caso `name` sólo acepta rutas absolutas.
- RequestDispatcher context.getNamedDispatcher(String name). `name` se refiere al nombre del servlet registrado en el web.xml o en las anotaciones `@WebServlet`

```java
//ejemplos:
//a partir del request
RequestDispatcher dispatcher = request.getRequestDispatcher("/ruta");
dispatcher.forward(request, response);

//a partir del context con ruta
ServletContext context = request.getServletContext();
RequestDispatcher dispatcher = context.getRequestDispatcher("/ruta");
dispatcher.forward(request, response);

//a partir del context con nombre de servlet
ServletContext context = request.getServletContext();
RequestDispatcher dispatcher = context.getNamedDispatcher("NombreServlet");
dispatcher.include(request, response);


```
#### Ejercicio 4

Completa la siguiente  tabla:

| Tipo |Objeto Java |Acceso |Tipo de datos |Elemento  |
|-------------|------------|--------|----------|-----------|
|Conf. inicial| | | | |
|Contexto | | | | |
|Petición | HttpRequest | | | |
|Respuesta | | | | |
|Sesión | | | | Attribute |
|Cookies | | Mismo sitio web, misma ruta y navegador de cliente | | |




### Problemas de concurrencia.

Muchos de los servidores de apliaciones son multihilo. Esto quiere decir que pueden atender simultaneamente varias peticiones sobre el mismo servlet. 

Los atributos de clase son compartidos por todas las peticiones. Esto puede generar problemas de concurrencia. Si este recurso es actualizado durante los métodos doGet o doPost pueden darse situaciones de valores inconsistencia.

Para solucionarlo lo más recomendable es usar [sentencias sincronizadas](https://docs.oracle.com/javase/tutorial/essential/concurrency/locksync.html) (*sychronized(objeto){sentencias}* ):

```java
public class ManualConcurrenciaServlet extends HttpServlet {

    public static final int REPETICIONES = 3000000;
    public final Object lockDeValor = new Object();
    private int valor;

    protected void processRequest(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        synchronized (lockDeValor) {
            valor = 0;
            int[] numerosAleatorios = new int[REPETICIONES];
            for (int i = 0; i < REPETICIONES; i++) {
                double a = Math.random() * 10;
                numerosAleatorios[i] = (int) a;
                valor += numerosAleatorios[i];
            }
            for (int i = 0; i < REPETICIONES; i++) {
                valor -= numerosAleatorios[i];
            }

            response.setContentType("text/html;charset=UTF-8");
            PrintWriter out = response.getWriter();
            try {
                out.println("<html>");
                out.println("<head>");
                out.println("<title>Problema de concurrencia</title>");
                out.println("</head>");
                out.println("<body>");
                out.println("El resultado es " + valor + "");
                out.println("</body>");
                out.println("</html>");

            } finally {
                out.close();
            }
        }
    }
    // doGet, doPost, y demás métodos
}


```

#### Ejercicio 5

Crea un formulario. Mediante dos cajas de texto debes registrar usuario y teléfono. Un servlet debe tomar los datos y guardarlos en un atributo o variable de sesión. Haz que el servlet muestre el contenido de dicho atributo.


#### Ejercicio 6

Crea un parámetro de inicio en el web.xml llamado autor. En él registra tu nombre. Modifica el servlet anterior para que dicho parámetro aparezca al pie de la lista de usuarios.


## Gestión de errores.
Podemos gestionar los errores y excepciones de dos modos:
- Forma programática.
- Forma declarativa. 
### Forma programática:
Es el código donde decidimos qué hacer en cada situación de error u excepción.

### Forma declarativa.
Se trata de definir las situaciones de error y excepciones en el fichero de despliegue. Es ahí donde decimos también qué se debe hacer en cada caso.
#### Errores HTTP.


```xml
<error-page>
    <error-code>404</error-code>
    <location>/tema8/404error.html</location>
</error-page>
```

#### Excepciones.


```xml
<error-page>
    <exception-type>NullPointerException</exception-type>
    <location>/GestorDeExcepcionesServlet</location>
</error-page>
<error-page>
    <exception-type>NumberFormatException</exception-type>
    <location>/GestorDeExcepcionesServlet</location>
</error-page>
```
## Listeners

Los listeners permiten que se ejecute determinado código se produce determinado evento.

Para hacerlo JEE nos brinda varios listeners. Se trata de interfaces que definen métodos que pueden ser reescritos para realizar las tareas  que necesitemos. Destacamo dos interfaces:
- ServletContextListener asociado al arranque/parada de la aplicación. Este interfaz define dos métodos.
    - public void contextInitialized(ServletContextEvent sce)
    - public void contextDestroyed(ServletContextEvent sce)
- HttpSessionListener asociado a la creación/eliminación de la sesión. Este interfaz define dos métodos.
    - public void sessionCreated(HttpSessionEvent se)
    - public void sessionDestroyed(HttpSessionEvent se)

Debemos registrar el listener en el descriptor de despliegue o mediante anotaciones. Además el servlet debe implementar uno u otro interfaz.

```xml
<listener>
    <listener-class>
        tema11.ServletListenerDeContexto
    </listener-class>
</listener>
```

```java
@WebListener
public class ServletListenerDeContexto implements
ServletContextListener {
    ...
```

## Filtros

- Los filtros permiten interceptar las peticiones que llegan al servidor si cumplen con un patrón definido. 

- Los filtros se definen en el descriptor de despliegue. Se aplican en el mismo orden en que aparecen en éste.

- Para definir un filtro debemos crear una clase que implemente el interfaz javax.servlet.Filter. Su métodos son
    - public void init(FilterConfig filterConfig). Cuando se crea inicia el filtro. Similar al init de los servlets.
    - public void destroy(). Cuando se destruye.
    - public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain). Cuando se intercepta una peticion.

- Para registrarlo en el descriptor de despliegue:
```xml
<filter>
    <filter-name>Filtro</filter-name>
    <filter-class>tema11.Filtro</filter-class>
    <init-param>
        <param-name>parametro</param-name>
        <param-value>valor</param-value>
    </init-param>
</filter>
```

- Para definir su patrones.
```xml
<filter-mapping>
    <filter-name>Filtro</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

    O mediante anotaciones:
```java
@WebFilter(filterName = "Filtro",
urlPatterns = {"/*"},
initParams = {
    @WebInitParam(name = "parametro", value = "valor")})
```

- Ejemplo de filtro 

```java
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;

@WebFilter(filterName = "Filtro", urlPatterns = {"/*"})
public class Filtro implements Filter {
    ...
}
```

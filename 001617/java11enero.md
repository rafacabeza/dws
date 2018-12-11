##Clase Java hoy:

 ####  Eliminar los warnings:
  - Añadir `return` después de cada forward.
  - Quitar los objetos printer de los servlet.
  
 #### Añadir un método para cada operación CRUD:
 
```java
    public void index()
    {
        String name = "index";
        LOG.info("En ModuleController->" + name);
        request.setAttribute("msg", name );
        dispatch("/WEB-INF/view/module/index.jsp");
    }
    
    public void create()
    {
        String name = "create";
        LOG.info("En ModuleController->" + name);
        request.setAttribute("msg", name );
        dispatch("/WEB-INF/view/module/index.jsp");
    }
    
    public void insert()
    {
        String name = "insert";
        LOG.info("En ModuleController->" + name);
        request.setAttribute("msg", name );
        dispatch("/WEB-INF/view/module/index.jsp");
    }
    
    public void edit()
    {
        String name = "edit";
        LOG.info("En ModuleController->" + name);
        request.setAttribute("msg", name );
        dispatch("/WEB-INF/view/module/index.jsp");
    }
    
    public void update()
    {
        String name = "update";
        LOG.info("En ModuleController->" + name);
        request.setAttribute("msg", name );
        dispatch("/WEB-INF/view/module/index.jsp");
    }
    
    public void delete(String id)
    {        
        String name = "delete";
        LOG.info("En ModuleController->" + name);
        request.setAttribute("msg", name );
        dispatch("/WEB-INF/view/module/index.jsp");
    }
    
```
 
 
  - Preparar el index.jsp
  
 ```jsp
  ....
  <jsp:useBean id="msg" class="String" scope="request" />
  ...
      <h1>Module Controller</h1>
      <h2>Método <%= msg %> </h2>
```  
  #### study/index: lista de usuarios
  
  ##### Crear el modelo
    
```java    
    public class Study {
        private long id;
        private String code;
        private String name;
        private String shortName;
        private String abreviation;
    }
    //añadir constructor
    //añadir getters y setters
```
 ##### Crear Persistencia
    
 ```java
 
  abstract class StudyDAO {
    
    public static final String DB_DRIVER = "com.mysql.jdbc.Driver";
    public static final String DB_URL = "jdbc:mysql://ipServidor/matricula";
    public static final String DB_USER = "usuario";
    public static final String DB_PASS = "usuario";    
    protected Connection connection;
    
    public void connect()
    {
        Class.forName(DB_DRIVER);
        connection = DriverManager.getConnection(DB_URL, DB_USER, DB_PASS);
    }
    public void disconnect()
    {
        connection.close();
    }
    
    public ArrayList<Study> getAll()
    {
        PreparedStatement stmt = null;
        ArrayList<Study> studies = null;
        
        try {
            stmt = connection.prepareStatement("select * from studies");
            ResultSet rs = stmt.executeQuery();
            studies = new ArrayList();

            int i = 0;
            while (rs.next()) {
                i++;
                Study study = new Study();
                study.setId(rs.getLong("id"));
                study.setCode(rs.getString("code"));
                study.setName(rs.getString("name"));
                study.setShortName(rs.getString("shortname"));
                study.setAbreviation(rs.getString("abreviation"));

                studies.add(study);
                LOG.info("Registro fila: " + i);
            }
        } catch (SQLException ex) {
            Logger.getLogger(StudyDAO.class.getName()).log(Level.SEVERE, null, ex);
        }
        return studies;
    }
```


##### Completar el controlador
```java
public void index()
{
    //objeto persistencia
    studyDAO = new StudyDAO();
    ArrayList<Study> studies = null; 
    //leer datos de la persistencia
    LOG.info("Argumento pagina: ");
    synchronized(studyDAO) {
        studyDAO.connect();
        studies = studyDAO.getAll();
        studyDAO.disconnect();
    }

    //pasar datos y reenviar a la vista
    request.setAttribute("studies", studies);
    String nextJSP = "/WEB-INF/view/study/index.jsp";
    RequestDispatcher dispatcher = getServletContext().getRequestDispatcher(nextJSP);
    dispatcher.forward(request,response);
}
```

##### Completar la vista

```html
<%@page import="model.Study"%>
<%@page import="java.util.Iterator"%>
<%@page import="java.util.ArrayList"%>
<%@page contentType="text/html" pageEncoding="UTF-8"%>
<jsp:useBean id="studies" class="ArrayList<model.Study>" scope="request"/>  
<!DOCTYPE html>
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>JSP Page</title>
    </head>
    <body>
        <h1>Lista de estudios </h1>
        <table>
            <tr>
                <th>Id</th>
                <th>Codigo</th>
            </tr>
            <%
                Iterator<model.Study> iterator = studies.iterator();
                while (iterator.hasNext()) {
                    Study study = iterator.next();%>
            <tr>
                <td><%= study.getId()%></td>
                <td><%= study.getCode()%></td>
                </td>
            </tr>
            <%
                }
            %>        
    </body>
</html>

```

  
  
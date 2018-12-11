### Mejorar la vista

- Como vemos se trata de colocar la estructura de cabecera/pie y estilos usados en el primer trimestre
- Además de incluír todos los campos en la lista.

```html
-<jsp:useBean id="msg" class="String" scope="request" />
+<jsp:useBean id="msg" class="java.lang.String" scope="request" />

<!DOCTYPE html>
<%@include file="/WEB-INF/view/header.jsp" %>

<div id="content">
    <h1>Lista de estudios </h1>
    <table>
        <tr>
            <th>Id</th>
            <th>Codigo</th>
            <th>Abreviatura</th>
            <th>Nombre</th>
            <th>Nombre Corto</th>
        </tr>
        <%        Iterator<model.Study> iterator = studies.iterator();
            while (iterator.hasNext()) {
                Study study = iterator.next();%>
        <tr>
            <td><%= study.getId()%></td>
            <td><%= study.getCode()%></td>
            <td><%= study.getAbreviation()%></td>
            <td><%= study.getName()%></td>
            <td><%= study.getShortName()%></td>
            </td>
        </tr>
    </table>
    <%
        }
    %>       
</div>
<%@include file="/WEB-INF/view/footer.jsp" %>
```

- Cabecera

```html
<%@page import="java.util.Enumeration"%>
<%@page contentType="text/html" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <link rel="stylesheet" type="text/css" href="<%= request.getContextPath()%>/static/css/default.css">
        <title>Framework MVC. Curso 2016/2017</title>
    </head>
    <body>
        <div id="header">
            <div id="title">Framework MVC (Java EE). Curso 2016/2017</div>
            <div>
                <a href="<%= request.getContextPath()%>/study">Estudios</a>
            </div>
        </div>

```

- Y pie

```html   
    <div id="footer"> Pie de página</div>
</body>
</html>
```

### Creación de registros:
- Formulario de entrada (view/study/create.jsp):

```html
<%@page import="model.Study"%>
<%@include file="/WEB-INF/view/header.jsp" %>

<div id="content">
    <h1>Alta de estudios </h1>
    <form action="<%= request.getContextPath()%>/study/insert" method="post">
        <label>Codigo</label><input name="code" type="text"><br>
        <label>Abreviatura</label><input name="abreviation" type="text"><br>
        <label>Nombre</label><input name="name" type="text"><br>
        <label>Nombre Corto</label><input name="shortName" type="text"><br>
        <label>Nombre Corto</label><input value="Enviar" type="submit"><br>
    </form>     
</div>
<%@include file="/WEB-INF/view/footer.jsp" %>
```

- Recepción en el controlador (StudyController)

```java
public void insert() 
{
    //objeto persistencia
    studyDAO = new StudyDAO();
    LOG.info("crear DAO");
    //crear objeto del formulario
    Study study = new Study();
    LOG.info("Crear modelo");
    study.setCode(request.getParameter("code"));
    study.setName(request.getParameter("name"));
    study.setShortName(request.getParameter("shortName"));
    study.setAbreviation(request.getParameter("abreviation"));
    LOG.info("Datos cargados");
    synchronized (studyDAO) {
        try {
            studyDAO.insert(study);
        } catch (SQLException ex) {
            Logger.getLogger(StudyController.class.getName()).log(Level.SEVERE, null, ex);
            request.setAttribute("ex", ex);
            request.setAttribute("msg", "Error de base de datos ");
            dispatch("/WEB-INF/view/error/index.jsp");
        }
    }
    redirect(contextPath + "/study/index");
}
```

- Tratamiento de la pesistencia (StudyDAO)

```java
public void insert(Study study) throws SQLException {
    PreparedStatement stmt = null;
    this.connect();
    stmt = connection.prepareStatement(
            "INSERT INTO studies(code, name, shortName, abreviation)"
            + " VALUES(?, ?, ?, ?)"
    );
    stmt.setString(1, study.getCode());
    stmt.setString(2, study.getName());
    stmt.setString(3, study.getShortName());
    stmt.setString(4, study.getAbreviation());

    stmt.execute();
}
```

### Tarea para el alumno

- Edición de registros.
- Borrado de registros.
# Bases de datos y MVC

Vamos a estudiar como realizar una conexión a base de datos usando JDBC, en nuestro caso con MySql, y una arquitectura MVC con DAO.

## Conexión a bases de datos:

### Prepara el entorno
- Para realizar la conexión a un gestor de base de datos necesitamos la librería o controlador propio del sistema. Descargar [aquí](https://dev.mysql.com/downloads/connector/j/).

- El fichero *.jar lo debemos ubicar en la carpeta `lib` de Apache Tomcat, por ejemplo: `/home/alumno/apache-tomcat-8.0.27/lib`
- Una vez hecho esto debemos añadir la librería a nuestro proyecto. Para hacerlo, propiedades del proyecto, apartado _Java Build Path_, pestaña _Libraries_.

### Código en un Servlet

 - En el repositorio de clase, acceder a la rama `database`.
 - Para realizar una operación debemos:
  1. Cargar el driver correspondiente
  2. Establecer una conexión.
  3. Realizar la operación de manipulación de datos correspondiente.
  

```java

package servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
//import java.sql.*;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.ResultSet;

/**
 * Servlet implementation class NamesServlet
 */
@WebServlet("/names")
public class NamesServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    // public static final String DB_DRIVER = "oracle.jdbc.driver.OracleDriver";

    public static final String DB_DRIVER = "com.mysql.jdbc.Driver";
    public static final String DB_URL = "jdbc:mysql://localhost/mvc17";
    public static final String DB_USER = "root";
    public static final String DB_PASSWORD = "root";
    Connection connection = null;

    // protected Connection connection;
    /**
     * @see HttpServlet#HttpServlet()
     */
    public NamesServlet() {
        loadDriver();
        connection = connect();
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse
     *      response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        out.append("Nombres");

        Statement stmt = null;
        ResultSet rs = null;

        try {
            stmt = connection.createStatement();
            // rs = stmt.executeQuery("SELECT * FROM names");

            // or alternatively, if you don't know ahead of time that
            // the query will be a SELECT...

            if (stmt.execute("SELECT * FROM names")) {
                rs = stmt.getResultSet();
            }
            System.out.println("consulta realizada");
            while (rs.next()) {
                out.append("<li>" + rs.getString("name") + "</li>");
            }
        } catch (SQLException ex) {
            // handle any errors
            System.out.println("SQLException: " + ex.getMessage());
            System.out.println("SQLState: " + ex.getSQLState());
            System.out.println("VendorError: " + ex.getErrorCode());
        } finally {
            closeStatement(rs);
        }
    }

    /**
     * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse
     *      response)
     */
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        Statement stmt = null;
        ResultSet rs = null;

        try {
            stmt = connection.createStatement();
            stmt.execute("INSERT INTO names(name) VALUES('" + request.getParameter("name") + "')");
            stmt = connection.createStatement();
            System.out.println("Inserción realizada");
            response.sendRedirect("names");

        } catch (SQLException ex) {
            // handle any errors
            System.out.println("SQLException: " + ex.getMessage());
            System.out.println("SQLState: " + ex.getSQLState());
            System.out.println("VendorError: " + ex.getErrorCode());
        } finally {
            closeStatement(rs);
        }

    }

    private void closeStatement(ResultSet rs) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException sqlEx) {
            } // ignore

            rs = null;
        }
    }

    private Connection connect() {
        Connection connection = null;
        try {
            connection = DriverManager.getConnection(DB_URL + "?user=" + DB_USER + "&password=" + DB_USER);
            System.out.println("Conectado");

        } catch (SQLException ex) {
            System.out.println("Fallo Conexion");
            System.out.println("SQLException: " + ex.getMessage());
            System.out.println("SQLState: " + ex.getSQLState());
            System.out.println("VendorError: " + ex.getErrorCode());
        }
        return connection;
    }

    private void loadDriver() {
        try {
            // The newInstance() call is a work around for some
            // broken Java implementations
            Class.forName(DB_DRIVER).newInstance();
        } catch (Exception ex) {
            System.out.println("no se ha cargado el driver");
        }
    }
}
package servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
//import java.sql.*;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.ResultSet;

/**
 * Servlet implementation class NamesServlet
 */
@WebServlet("/names")
public class NamesServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    // public static final String DB_DRIVER = "oracle.jdbc.driver.OracleDriver";

    public static final String DB_DRIVER = "com.mysql.jdbc.Driver";
    public static final String DB_URL = "jdbc:mysql://localhost/mvc17";
    public static final String DB_USER = "root";
    public static final String DB_PASSWORD = "root";
    Connection connection = null;

    // protected Connection connection;
    /**
     * @see HttpServlet#HttpServlet()
     */
    public NamesServlet() {
        loadDriver();
        connection = connect();
    }

    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse
     *      response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        out.append("Nombres");

        Statement stmt = null;
        ResultSet rs = null;

        try {
            stmt = connection.createStatement();
            // rs = stmt.executeQuery("SELECT * FROM names");

            // or alternatively, if you don't know ahead of time that
            // the query will be a SELECT...

            if (stmt.execute("SELECT * FROM names")) {
                rs = stmt.getResultSet();
            }
            System.out.println("consulta realizada");
            while (rs.next()) {
                out.append("<li>" + rs.getString("name") + "</li>");
            }
        } catch (SQLException ex) {
            // handle any errors
            System.out.println("SQLException: " + ex.getMessage());
            System.out.println("SQLState: " + ex.getSQLState());
            System.out.println("VendorError: " + ex.getErrorCode());
        } finally {
            closeStatement(rs);
        }
    }

    /**
     * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse
     *      response)
     */
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        Statement stmt = null;
        ResultSet rs = null;

        try {
            stmt = connection.createStatement();
            stmt.execute("INSERT INTO names(name) VALUES('" + request.getParameter("name") + "')");
            stmt = connection.createStatement();
            System.out.println("Inserción realizada");
            response.sendRedirect("names");

        } catch (SQLException ex) {
            // handle any errors
            System.out.println("SQLException: " + ex.getMessage());
            System.out.println("SQLState: " + ex.getSQLState());
            System.out.println("VendorError: " + ex.getErrorCode());
        } finally {
            closeStatement(rs);
        }

    }

    private void closeStatement(ResultSet rs) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException sqlEx) {
            } // ignore

            rs = null;
        }
    }

    private Connection connect() {
        Connection connection = null;
        try {
            connection = DriverManager.getConnection(DB_URL + "?user=" + DB_USER + "&password=" + DB_USER);
            System.out.println("Conectado");

        } catch (SQLException ex) {
            System.out.println("Fallo Conexion");
            System.out.println("SQLException: " + ex.getMessage());
            System.out.println("SQLState: " + ex.getSQLState());
            System.out.println("VendorError: " + ex.getErrorCode());
        }
        return connection;
    }

    private void loadDriver() {
        try {
            // The newInstance() call is a work around for some
            // broken Java implementations
            Class.forName(DB_DRIVER).newInstance();
        } catch (Exception ex) {
            System.out.println("no se ha cargado el driver");
        }
    }
}


```
   
## Manipulación de datos

https://docs.oracle.com/javase/tutorial/jdbc/basics/index.html


### Recupear datos
- https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html

- Se puede usar SQL directamente:

```java

public static void viewTable(Connection con, String dbName)
    throws SQLException {

    Statement stmt = null;
    String query =
        "select COF_NAME, SUP_ID, PRICE, " +
        "SALES, TOTAL " +
        "from " + dbName + ".COFFEES";

    try {
        stmt = con.createStatement();
        ResultSet rs = stmt.executeQuery(query);
        while (rs.next()) {
            String coffeeName = rs.getString("COF_NAME");
            int supplierID = rs.getInt("SUP_ID");
            float price = rs.getFloat("PRICE");
            int sales = rs.getInt("SALES");
            int total = rs.getInt("TOTAL");
            System.out.println(coffeeName + "\t" + supplierID +
                               "\t" + price + "\t" + sales +
                               "\t" + total);
        }
    } catch (SQLException e ) {
        JDBCTutorialUtilities.printSQLException(e);
    } finally {
        if (stmt != null) { stmt.close(); }
    }
}

```

- Se pueden usar sentencias preparadas:

    - Ejemplo de SELECT.
     1. Preparar
     2. Pasa parámetros
     3. Ejecutar con `executeQuery()`
     
```java
String selectSQL = "SELECT USER_ID, USERNAME FROM DBUSER WHERE USER_ID = ?";
PreparedStatement preparedStatement = dbConnection.prepareStatement(selectSQL);
preparedStatement.setInt(1, 1001);
ResultSet rs = preparedStatement.executeQuery(selectSQL );
while (rs.next()) {
	String userid = rs.getString("USER_ID");
	String username = rs.getString("USERNAME");
}
```
- Ejemplo de actualizacón (INSERT, UPDATE, DELETE).
1. Preparar
2. Pasa parámetros
3. Ejecutar con `executeUpdate()`
```java
    String updateString =
        "update " + dbName + ".COFFEES " +
        "set SALES = ? where COF_NAME = ?";

    String updateStatement =
        "update " + dbName + ".COFFEES " +
        "set TOTAL = TOTAL + ? " +
        "where COF_NAME = ?";

    try {
        con.setAutoCommit(false);
        updateSales = con.prepareStatement(updateString);
        updateTotal = con.prepareStatement(updateStatement);

        for (Map.Entry<String, Integer> e : salesForWeek.entrySet()) {
            updateSales.setInt(1, e.getValue().intValue());
            updateSales.setString(2, e.getKey());
            updateSales.executeUpdate();
            updateTotal.setInt(1, e.getValue().intValue());
            updateTotal.setString(2, e.getKey());
            updateTotal.executeUpdate();
            con.commit();
        }
    } catch (SQLException e ) {
        JDBCTutorialUtilities.printSQLException(e);
        if (con != null) {
            try {
                System.err.print("Transaction is being rolled back");
                con.rollback();
            } catch(SQLException excep) {
                JDBCTutorialUtilities.printSQLException(excep);
            }
        }
    } finally {
        if (updateSales != null) {
            updateSales.close();
        }
        if (updateTotal != null) {
            updateTotal.close();
        }
        con.setAutoCommit(true);
    }
}
```


## Usando MVC con DAO

Esto supone usar cuatro elementos o paquetes:
- Modelo. Usamos DAO:
- Persistencia mediante clases DAO (Data Access Object)
- DTO, Data Terminating Object, mediante clases Java Bean.
- Vista. JSP
- Controlador. Mediante clases Servlet.

Veamos un ejemplo en el que tratamos de manejar los datos una tabla `articles`. 
### Modelo
- El modelo es una clase `model.Article`
- Sólo debe contar con los atributos necesarios para mapear las columnas de la tabla correspondiente, sus _getters_ y _setters_

### DAO
- La persistencia, asociada al modelo, debe gestionarse con la clase `persistence.ArticleDAO`
- Esta clase es responsable de conectarse a base de datos y realizar todas las operaciones de lectura y modificación de datos.

### Vista: JSP
- Tal como hemos visto en ejercicios previos, el JSP es el responsable de generar el HTML

### Controlador: Servlet

- Los servlets deben ocuparse de:
 - Recibir la petición correspondiente que se traduce en una tarea, método, concreto asociado a la tabla de trabajo.
 - Crear el objeto u objetos de persistencia.
 - Cargar/modificar los datos necesarios con el apoyo de la clase de persistencia (DAO).
 - Si la operación es de lectura de datos, hacer forwarding a un JSP para construir la vista. Debemos apoyarnos en un RequestDispatcher.
 - Si es una uperación de actualización, reenviar a una nueva url: `response.sendRedirect(url)`

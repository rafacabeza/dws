# JPA

## ¿Qué es JPA?
- Java Persistence API (JPA) es una especificación de Java EE que permite a los desarrolladores Java hacer un mapeo entre los objetos y las tablas de una base de datos relacional (Object Relational Mapping).

- Un ORM facilita enormemente el uso de una base de datos en una aplicación.

## Spring Data JPA
- Spring Data JPA es un módulo que forma parte del proyecto Spring Data y básicamente nos ayuda a simplificar el desarrollo de la persistencia de datos utilizando el concepto de repositorios, algo muy parecido al patrón DAO (Data Access Object).
- Spring Data es una forma más sencilla de trabajar con JPA.
- Beneficios de Spring Data JPA
 - Desarrollo sencillo y ágil de la capa de persistencia de datos.
 - No es necesario escribir código SQL aunque también es posible hacerlo.
 - Más fácil que JDBC.
 - Permite al desarrollador olvidarse del manejo de Excepciones.
 - Código más fácil de entender y mantener.
 
### Para abrir boca: Ejemplo de uso:
```java
// 1. Crear objeto a guardar en la BD
Article article= new Article();
article.setId(1);
article.setCode("KB001");
article.setName("Teclado multimedia");
article.setPrice(18.90);
// 2. Guardar objeto en la BD
repositoryArticles.save(article);
```

## Configurar JPA
### Dependencias en pom.xml
```xml
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-jpa</artifactId>
    <version>2.0.0.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-entitymanager</artifactId>
    <version>5.2.11.Final</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>6.0.2.Final</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.44</version>
</dependency>
```

### Configuración en el root-contex.xml

1. Añadir namespaces jpa y tx.
 - Abrir el fichero
 - Abajo, pestaña namespaces
 - Marcar los dos indicados: jpa y tx
2. Añadir beans al root-context.xml

```xml
//Package local donde guardaremos nuestras clases
<jpa:repositories base-package="net.itinajero.app.repository" />

//clase que se ocupa de las conexiones de BBDD
//Parámetros de conexión a nuestro MySql (p.ej.)
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver" />
    <property name="url" value="jdbc:mysql://localhost:3306/root?useSSL=false" />
    <property name="username" value="root" />
    <property name="password" value="root" />
</bean>

//implementación de la interfaz jpaVendorAdapter
//vamos a usar la de Hibernate
//propiedad generateDdl -> si deseamos generar las bbdd de forma automática (si usamos migraciones)
//showSql si queremos ver en la consola el sql ejecutado. True -> veremos el sql en la consola
//databasePlatform para indicar que variante SQL usamos
<bean id="jpaVendorAdapter" class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
    <property name="generateDdl" value="false" />
    <property name="showSql" value="true"></property>
    <property name="databasePlatform" value="org.hibernate.dialect.MySQL5Dialect" />
</bean>

//implementación de la interfaz entityManagerFactory
//componente que gestiona los modelos en la bbdd
// package -> indicamos dnd están los modelos: model
// datasource -> bean datasource de arriba de ese nombre.
// jpaVendorAdapter -> bean de arriba de ese nombre.
<bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
    <property name="packagesToScan" value="net.itinajero.app.model" />
    <property name="dataSource" ref="dataSource" />
    <property name="jpaVendorAdapter" ref="jpaVendorAdapter" />
</bean>

//Necesario para usar transacciones
// entityManagerFactory -> bean de arriba
<bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
    <property name="entityManagerFactory" ref="entityManagerFactory" />
</bean>
```
3. Para terminar creamos el paquete repository

> Tag v2.0: git checkout v2.0

### Probar que la conexión funciona

- Vamos a crear la siguiente clase:

```java
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App 
{
    public static void main( String[] args )
    {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("root-context.xml");
        System.out.println( "Contexto creado!" );        
        context.close();
        System.out.println( "Contexto cerrado!" );        
    }
}
```

- Ojo!! debemos añadir una nueva carpeta `src/test/resources` y ubicar allí el `root-context.xml` para que sea accesible desde la aplicación de consola.
- Si todo va bien no aparecerán errores y nos apareceran los mensajes de la consola.

## Configurar nuestras clases para que usen JPA

### En la clase modelo

- Debemos añadir diversas anotaciones para que JPA mapee nuestra clase modelo a una tabla determinada y darle información como cual es la clave principal, cuales son las columnas, ...
- Mapeo de la tabla: etiquetas `@Entity`y `@Table`. En table hay que añadir la propiedad `name` con el nombre de la tabla.
- Clave principal: `@id`
- Valor autogenerado: `@GeneratedValue`, debe llevar una propiedad `strategy` indicano una constante adecuada. Los autonuméricos de MySql llevan el del ejemplo.
- Columnas. Si las columnas de la tabla se llaman igual que los atributos de la clase modelo no es necesario añadir más anotaciones. En caso contrario debemos usar la etiqueta `@Column` con algunas propiedades:
 - Nombre de la columna: `name="nombreColumna"`
 - Tamaño o longitud: `length=valor`
 - Valor nulo: `nullable=true|false`
 - Ejemplo: `@Column(name="name",length=100,nullable=false)`
 - En nuestra clase `article`, como puedes comprobar no hemos usado ninguna anotación de columnas:

```java
package com.rafacabeza.shop.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "articles")
public class Article {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;
    private String code;
    private String name;
    private double price;

    //TODO: constructor, getters, setters, toString...

}
```

### Añadir interfaz Repository:

- Añadimos interfaz ArticleRepository
 - New Interfaz ...
 - Marcamos que extienda la interfaz `CrudRepository` (org.springframework.data.repository)
 - Antes de acabar debemos establecer dos parámetros: <T, ID>, nombre de la clase modelo y tipo de dato de la clave principal. <Article, integer>
 - Pulsar _finish_, y ya tenemos nuestro repositorio con todas las operaciones CRUD
 - Ejemplo de ArticleRepository: 
 - Para concluir añadimos una anotación `@Repository`
 
```java

```
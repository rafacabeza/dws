# Errores y excepciones

- El control de errores y excepciones puede hacerse de dos formas:
  - De forma declarativa, en el descriptor de despliegue `web.xml`
  - De forma programática, tratando cada error en el propio de los servlets.
  
  
## Forma declarativa 

Ejemplo de declaración de errores 404 y 500.
```
<error-page>
  <error-code>404</error-code>
  <location>/WEB-INF/error/404.html</location>
</error-page>
<error-page>
  <error-code>500</error-code>
  <location>/WEB-INF/error/500.html</location>
</error-page>
```

Las excepciones también pueden tratarse de la misma forma. Veamos un par de ejemplos:
 - Aquí hay un dos tipos de excepciones tratadas mediante un servlet ("/ErrorHandler"), el resto se tratan como error 500.
```
  <error-page>
    <exception-type>java.lang.NullPointerException</exception-type>
    <location>/ErrorHandler</location>
  </error-page>
  <error-page>
    <exception-type>java.lang.ArithmeticException</exception-type>
    <location>/ErrorHandler</location>
  </error-page>  
  <error-page>
    <error-code>500</error-code>
    <location>/WEB-INF/error/500.html</location>
  </error-page>
</web-app>
```

- En el siguiente ejemplo todas las excepciones son tratadas con un servlet
```
  <error-page>
    <exception-type>java.lang.Throwable</exception-type>
    <location>/ErrorHandler</location>  </error-page>
</web-app>
```



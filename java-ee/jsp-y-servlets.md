# JSP y Servlets

- Puedes usasr como apoyo el [curso](http://www.javahispano.org/portada/2011/7/28/tutorial-basico-de-java-ee-por-abraham-otero.html) de Abraham Otero (JavaHispano). Especialmente su texto en [pdf](http://static1.1.sqspcdn.com/static/f/923743/14770633/1416082087870/JavaEE.pdf?token=apk0KPoXc0SFEKkQVHjHVbcvGGk%3D)
- Viniendo de PHP, JSP es un lenguaje equivalente. 
- Se trata de embeber el código Java dentro de Html.
- En la práctica, un fichero con código JSP debe ser compilado como una clase:
    - Esa clase no se ve aparentemente.
    - Esa clase hereda de otra llamda Servlet.
    - La otra opción es crear directamente la clase Servlet.
    
- Todo puede ser programado con JSP, como todo puede ser programado con scripts PHP mezclados con Html. Esto no es lo más adecuado:
    - JSP es adecuado para construir el Html final, las vistas.
    - La lógica principal del negocio debe ser construida mediante Java puro: Servlets.
    - Vamos a centrarnos en JSP para la construcción de vistas.
    - Veremos con más profundidad el uso de Servlets.
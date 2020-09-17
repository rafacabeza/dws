# PHP


## Entorno de desarrollo.

- Vamos a partir del entorno de desarrollo preparado con docker:

https://github.com/rafacabeza/entornods

- Debemos clonar dicho repositorio en nuestro espacio de trabajo:

```bash
cd ~
git clone git@github.com:rafacabeza/entornods.git
```


- Entorno Docker para el módulo de Desarrollo Web en Entorno Servidor
- Iniciar servicio:
    `docker-compose up -d`
- Parar servicio:
    `docker-compose down`
- Ver máquinas corriendo:
    `docker-compose ps`


- En este entorno se van a usar tres sitios web de prueba. Para poder usarlos debemos *engañar* al DNS.
- Debes editar tu fichero `/etc/hosts` para acceder a los dos sitios web creados por el mismo.

    ```
    127.0.0.1	web1.com
    127.0.0.1	web2.com 
    127.0.0.1	phpmyadmin.docker
    ```

- Prueba a hacer ping a estos sitios. 


- Este entorno creo los siguientes contendedores en su rama master:
    - proxy: Es un proxy nginx para poder mantener simultaneamente múltiples contenedores con diferentes sitios web.
    - db: Contenedor mysql para uso de bases de datos.
    - phpmyadmin: Contenedor para administración web del anterior.
    - web1: sitios web1.com apache y php (Dockerfile web1)
    - web2: sitio web2.com con apache y php (php:7.4-apache)


- Pruebe en su navegador las siguientes url's:

    - http://web1.com/ 
    - http://web1.com/factorial.php?numero=3
    - http://web1.com/factorial.php?numero=10
    - http://web2.com/
    - http://phpmyadmin.docker


- El último sitio nos permite accede a `phpmyadmin`
- Este entorno incorpora un script para inicializar una base de datos en `./data/init-db`
- Conectese a http://phpmyadmin.docker e importe el script citado.
- Pruebe a acceder ahora de nuevo a http://web1.com/ 
- Analiza lo ocurrido y crea otro sitio web en el mismo entorno llamado web3 (web3.com).



## Primeros pasos

- Damos por hecho que el alumno tiene una base de programación y de orientación a objetos.
- Vamos a hacer un recorrido rápido sobre las bases de PHP.
- Para completar información respecto a lo que aquí se cuenta el mejor sitio
    es la documentación oficial  <a href="http://php.net/manual/es/"> php.net/manual/es/ </a>


- El objetivo esta documentación es ayudar al aprendizaje rápido del lenguaje 
PHP por parte de estudiantes de 2º curso de Desarrollo de Apliaciones Web. 
No obstante pueden ser útiles por aquellos que ya conozcan otros lenguajes 
de programación como JAVA, C++ o C#.
- Por este motivo se omitirá mucho contenido teórico sobre lo que es un 
lenguaje de programación orientado a objetos.
Igualmente, las explicaciones de sintaxis serán mínimas, pues la sintaxis es
muy parecida a los citadso lenguajes.


### ¿Qué es PHP?

- PHP (acrónimo recursivo de "PHP: Hypertext Preprocessor") es un lenguaje de código abierto muy popular especialmente adecuado para el desarrollo web y que puede ser incrustado en HTML. 

```php
<!DOCTYPE html>
<html>
    <head>
        <title>Ejemplo</title>
    </head>
    <body>

        <?php
            echo "¡Hola, soy un script de PHP!";
        ?>

    </body>
</html>
```


- PHP está construído a partir de Perl
- Perl está basado en C.
- Las variables son al estilo Perl
- La sintáxis es muy similar a C (y por tanto a Java)


### Marcado:

- Los ficheros php deben tener extensión ".php"
- El php puede estar embebido en código html
    - Los fragmentos de php deben ir etiquetasdos entre `<?php ?>`
- O pueden ser ficheros código php puro.
    - Sólo debe aparecer al inicio la marca de apertura: `<?php`
    ```php
    echo "Hola mundo!";
    ```


### Variables y tipos

- Los tipos básicos son:
    - Entero: número entero con signo
    - Flotante: número decimal con signo
    - Booleano: vale true o false
    - Cadena de caracteres: cadena de caracteres delimitada por comillas simples o dobles.


- Las variables deben comenzar por dolar: `$miVariable`
- No deben ser definidas previamente
- No tienen un tipo fijo.
- Deben segir notación de estilo `$camelCase`;
- Podemos consultar si una variable está definida y su tipo de dato:
    - isset($num)


- Las constantes se crear usarndo la palabra


### Operadores


### Operadores


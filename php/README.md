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

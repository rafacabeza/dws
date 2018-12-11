## Google Calendar

- Vamos a ver como acceder a los eventos de un Calendario de Google
- Necesitamos una cuenta de Google vinculada con nuestra aplicación y el identificador del calendario.

### Cómo hacerlo:
- Habilitar el API:
 - Accedemos a https://console.developers.google.com/apis/library?project=i-sylph-160918
 - Buscamos Google Calendar y ya en él habilitamos el API:
![](/img/calendar1.png)
 - Una vez habilitada necesitamos el fichero de credenciales:
  - Menú izquierdo: Credenciales.
  - Crear credenciales: Clave de cuenta de servicio.
  - Seleccionamos "Nueva cuenta de servicio", función "project->propietario". Seguro que hay otra opción con menos privilegios pero con esta no tendremos problemas pra probar.
  ![](/img/calendar2.PNG)
  - Descargamos el fichero. Lo renombramos como cient_secret.json y lo ubicamos en nuestr aplicación. Por ejempllo en _/config_
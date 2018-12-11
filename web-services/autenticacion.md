# OAuth
- Referencias:
    - https://stormpath.com/blog/what-the-heck-is-oauth
    - http://stivencastillo.com/2017/09/29/autenticacion-rest-api-laravel-passport/
    
- No es un API o servicio
- Es un _standard_ abierto para autorización.
- Permite acceso seguro delegado
- Para lograrlo se usan _tokens_ de acceso en lugar de credenciales
- Dos versiones: OAuth 1.0 y OAuth2. Son diferentes e incompatibles. OAuth2 es mucho más utilizado, al hablar de OAuth nos referimos a OAuth2.

## ¿Qué hace OAuth?

- Hay 4 modos de uso o tipos de concesión de autorización.
- En todos ellos un servidor OAuth acaba entregando un testigo o _token_ que permite acceder a determina información o servicio del propio servidor OAuth. Veamos cuales son.

## Concesión de código de autorización.

- Usada en servidores web.
- Se trata de permitir el acceso a la aplicación web usando una identidad de una 
tercera parte (Google, Facebook, Github, ...) o nuestro propio servidor OAuth.
- Al pulsar login en nuestra aplicación se reenvía a una página de la tercera parte 
donde se pide aceptar ciertos permisos.
- Si se aceptan esos permisos, se vuelve a la aplicación inicial devolviendo un código 
de autorización.
- Nuestra aplicación web solicita al API del proveedor (Google, Facebook, ...) 
un _token_ usando el código recibido junto a unas credenciales otorgadas por el proveedor a nuestra aplicación.
    NOTA: La petición del token requiere un user_id de la aplicación y un user_secret otorgados por el proveedor.
- El token es usado por nuestra aplicación para pedir la identidad de usuario a la tercera parte a través de su API.

    ```
    https://login.blah.com/oauth?response_type=code&client_id=xxx&redirect_uri=xxx&scope=email
    https://yoursite.com/oauth/callback?code=xxx
    POST https://api.blah.com/oauth/token?grant_type=authorization_code&code=xxx&redirect_uri=xxx&client_id=xxx&client_secret=xxx
    OJO: response_type=code, grant_type=authorization_code
    ```
    
## Concesión implícita.

- Usada por aplicaciones de lado del cliente (Angular, React, Vue, ...) o aplicaciones móviles.
- En este caso se solicita directamente el token, sin usar el código de autorización intermedio

    
    ```
    https://login.blah.com/oauth?response_type=token&client_id=xxx&redirect_uri=xxx&scope=email
    https://yoursite.com/oauth/callback?token=xxx
    ```
    
    
## Concesión de credenciales de contraseña

- Usado por aplicaciones web o móviles creadas por la misma entidad que el servidor OAuth. NO HAY TERCERA PARTE.
- Se trata de dar acceso a un usuario a nuestro propio servicio web.
- La aplicación pide usuario y contraseña y solicita el token al servicio OAuth directamente. Ojo, sólo debe hacerse si somos los creadores de ambas partes: aplicación y servicio OAuth.
    
    ```
    POST https://login.blah.com/oauth/token?grant_type=password&username=xxx&password=xxx&client_id=xxx
    ```

- El client_id identifica a nuestra aplicación. La petición devuelve un token.
        
## Concesión de credenciales de cliente

- En este caso se trata de conceder permisos de uso, no a un usuario, sino a una aplicación. 
- Por ejemplo podemos crear una aplicación que consulte determinados metadatos de nuestro servicio web y queremos asegurarnos que es nuestra aplicación quien lo pide y no otra.


    ```
    POST https://login.blah.com/oauth/token?grant_type=client_credentials&client_id=xxx&client_secret=xxx
    ```
    
- El client_id y client_secret son suministrados por el proveedor.
- La petición devuelve un  token
    
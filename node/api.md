# Creación de una API
<!-- ## Tiempo estimado: 150 minutos -->


## Configurar Visual Studio Code


## Autocompletar código
- Para que nos ayude a completar código usamos el atajo `ctrl+space`
- Si este atajo lo usamos para otra cosa (p.ej. Albert) debemos añadir los siguientes atajos a nuestro gusto:

```json
{ 
    "key": "ctrl+shift+space",            
    "command": "editor.action.triggerSuggest",
    "when": "editorHasCompletionItemProvider && textInputFocus && !editorReadonly" 
},
{ 
    "key": "ctrl+shift+space",            
    "command": "toggleSuggestionDetails",
    "when": "suggestWidgetVisible && textInputFocus" 
}
```


## Estilo de código

- Puede que nuestro proyecto sea desarrollado por varias personas.
- Queremos un estilo uniforme
- Y si nos detecta fallos mejor
- Vamos a ver que configuración necesitamos.
- Podemos partir desde este *boilerplate*: https://github.com/rafacabeza/vscode_node_boilerplate


## Prettier
- Vamos a añadir el complemento Prettier (Esben Petersen)
- vamos a añadir la dependencia al proyecto: 

```
npm i -D prettier
```

- Vamos a modificar algunas pautas para que prettier uniformice nuestro código (js, css, html, json). 
```json
# Fichero *.prettierrc*
{
  "trailingComma": "none",
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true
}
```


## ESLint: ECMAScript Linter

- Además ESLint nos va a ayudar a depurar el código: detectar errores y corregir algunos.
- ESLint puede usarse desde la consola pero nosotros queremos usarlo desde el editor.
- Su configuración puede ser compleja. La definimos en el fichero *.eslintrc.json*
- Además puede requerir varias dependencias, entre otras cosas para que VSCode lo pueda usar.


- Dependencias de desarrollo que añadiremos a nuestro proyecto:
```
  "devDependencies": {
    "eslint": "^6.8.0",
    "eslint-config-airbnb": "^18.0.1",
    "eslint-config-node": "^4.0.0",
    "eslint-config-prettier": "^6.10.0",
    "eslint-plugin-import": "^2.20.1",
    "eslint-plugin-jsx-a11y": "^6.2.3",
    "eslint-plugin-node": "^11.0.0",
    "eslint-plugin-prettier": "^3.1.2",
    "eslint-plugin-react": "^7.18.3",
    "eslint-plugin-react-hooks": "^1.7.0",
  }
```
- No te olvides de ejecutar ahora: `npm install`


- Crearemos el fichero .eslintrc.json con este contenido:
```json
{
  "extends": ["airbnb", "prettier"],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error",
    "no-unused-vars": "warn",
    "no-console": "off"
  }
}
```
- Las reglas las podemos añadir/modificar en rules
- Vamos a usar el standard de codificación de airbnb.
- Para ficheros javascript, eslin se ocupará de ejecutar prettier


## Configurar VS Code
- Nos falta añadir algunas reglas para que nuestro editor use los pluggins instalados:

  - Que formatee al salvar, salvo el javascript.
  - Que active ESLint para notificar errores de código (menu view->problems)
  - Que arregle nuestro javascript al salvar (esto incluye pasar prettier)
- Esto lo podemos hacer en el json general de VSCode
- O sólo para el proyecto actual guardando el json en un fichero llamado *.vscode/settings.json*


- Contenido guardado en **.vscode/settings.json**

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "[javascript]": {
    "editor.formatOnSave": false
  },
  "eslint.enable": true,
  "eslint.alwaysShowStatus": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```



## Descripción

- *Creación de una API REST estándar y completa que pueda clonarse para distintos proyectos*


## Objetivos

- Conocer el objeto Router de express
- Conocer arquitecturas MVC
- Familiarizarse con base de datos no relacional
- Comprender las ventajas en JavaScript de MongoDB frente a bases de datos relacionales


## Primeros pasos

TODO: ¿Fork o scratch?
- [Fork del repositorio en GitHub](https://github.com/juanda99/curso-node-js-proyecto-api)
- Clonar tu repositorio
- Inicializar proyecto con npm init
- Instalar y configurar eslint extendiendo de standard:

  ```bash
  npm i -D eslint@5.4.0
  node_modules/.bin/eslint --init
  ```

- Personalizar eslint utilizando el fichero *.eslintrc.json*


## Instalar paquetes y configurar npm start

- Instalamos express

  ```bash
  npm i -S express@4.16.3
  ```

- Configuramos package.json para que nuestro servidor arranque mediante *npm start*

  ```json
  "scripts": {
    "start": "node app/server.js"
  },
  ```


## [Postman](https://www.getpostman.com/)

- Utilizaremos [Postman](https://www.getpostman.com/) para testear nuestra API
  - Independiente de que hagamos tests por otro lado
- Es una herramienta muy extendida


## Comprobación API inicial

- Arranca la aplicación y comprueba su funcionamiento mediante [Postman](https://www.getpostman.com/)

```js
var express = require('express') //llamamos a Express
var app = express()

var port = process.env.PORT || 8080  // establecemos nuestro puerto

app.get('/', (req, res) => {
  res.json({ mensaje: '¡Hola Mundo!' })
})

app.get('/cervezas', (req, res) => {
  res.json({ mensaje: '¡A beber cerveza!' })
})

// iniciamos nuestro servidor
app.listen(port)
console.log('API escuchando en el puerto ' + port)
```



## Implementación API: Routers y parámetros


## Añadir rutas

- Añade la ruta **POST /cervezas** con respuesta:

  ```json
  { "mensaje": "Cerveza guardada" }
  ```

- Añade la ruta **DELETE /cervezas** con respuesta:

  ```json
  { "mensaje": "Cerveza borrada" }
  ```

- Muestra el mensaje *API escuchando en el puerto 8080* **justo cuando se levante el puerto**

- Comprueba funcionamiento mediante [Postman](https://www.getpostman.com/)

- Podemos utilizar la extensión [ExpressSnippet](https://marketplace.visualstudio.com/items?itemName=vladmrnv.expresssnippet) para autocompletado.


## Commit con las nuevas rutas

- Hagamos un commit del repositorio, sin la carpeta node_modules

  ```bash
  git status
  git add -A app/server.js
  git commit -m "Añadido post y delete, arreglado error"
  git push
  ```

- Debemos hacer nuevas instantáneas (commits) en cada paso
  - Aquí no se documentarán por brevedad
  - Aquí hemos mezclado un parche con una funcionalidad nueva. ¡No es lo correcto!


## nodemon

- Es un wrapper de node, para reiniciar nuestro API Server cada vez que detecte modificaciones.

  ```bash
  npm i -D nodemon
  ```

- Cada vez que ejecutemos **npm start** ejecutaremos nodemon en vez de node. Habrá que cambiar el script en el fichero *package.json*:

  ```bash
  "start": "nodemon app/server.js"
  ```


## Uso de enrutadores

- Normalmente una API:
  - Tiene varios recursos (cada uno con múltiples endpoints)
  - Sufre modificaciones -> mantener versiones

- Asociamos enrutadores a la app en vez de rutas como hasta ahora:
  - Cada enrutador se asocia a un recurso y a un fichero específico.
  - Se pueden anidar enrutadores (*router.use*)


- El código para un enrutador sería así:

```js
// para establecer las distintas rutas, necesitamos instanciar el express router
var router = express.Router()

//establecemos nuestra primera ruta, mediante get.
router.get('/', (req, res) => {
  res.json({ mensaje: '¡Bienvenido a nuestra API!' })
})

// nuestra ruta irá en http://localhost:8080/api
// es bueno que haya un prefijo, sobre todo por el tema de versiones de la API
app.use('/api', router)
```


## Configura enrutadores

- Crea un enrutador para el versionado  de la API:
  - Ruta **GET /api** (simulará el versionado de la api):

  ```json
  { mensaje: '¡Bienvenido a nuestra API!' }
  ```

- Crea un enrutador anidado para los endpoints de las cervezas
  - *GET /api/cervezas*
  - *POST /api/cervezas*
  - ...


## server.js con enrutador

```js
var express = require('express') // llamamos a Express
var app = express()

var port = process.env.PORT || 8080 // establecemos nuestro puerto

var router = require('./routes')

app.get('/', (req, res) => {
  res.json({ mensaje: '¡Hola Mundo!' })
})

app.use('/api', router)

// iniciamos nuestro servidor
app.listen(port, () => {
  console.log(`App listening on port ${port}`)
})
```


## Enrutador base para el versionado

```js
var router = require('express').Router()
var cervezasRouter = require('./cervezas')

router.get('/', (req, res) => {
  res.json({ mensaje: 'Bienvenido a nuestra api' })
})

router.use('/cervezas', cervezasRouter)

module.exports = router
```


## Enrutador cervezas

```js
var router = require('express').Router()

router.get('/', (req, res) => {
  res.json({ mensaje: 'Listado de cervezas' })
})

router.post('/', (req, res) => {
  res.json({ mensaje: 'Cerveza guardada' })
})

router.delete('/', (req, res) => {
  res.json({ mensaje: 'Cerveza borrada' })
})

module.exports = router
```


## Envio de parámetros

- Cuando el router recibe una petición, podemos observar que ejecuta una función de callback:

```js
(req, res) => {}
```

- El parámetro **req** representa la petición (request)
  - Aquí es donde se recibe el parámetro


## Tipos de envio de parámetros

- **Mediante la url**
  - Se recogerán mediante:

  ```bash
  req.params.nombreVariable   //  http://localhost/variable
  req.query.nombreVariable    //  htp://localhost/url?variable=valor
  ```

- **Mediante post** en http hay dos posiblidades:
  - application/x-www-form-urlencoded ([pocos datos]((http://stackoverflow.com/questions/4007969/application-x-www-form-urlencoded-or-multipart-form-data))
  - multipart/form-data ([muchos datos ¿ficheros?]((http://stackoverflow.com/questions/4007969/application-x-www-form-urlencoded-or-multipart-form-data))


## Parámetros por url

- Vamos a mandar un parámetro *nombre* a nuestra api, de modo que nos de un saludo personalizado.

```bash
GET http://localhost:8080/pepito
```

```js
app.get('/:nombre', (req, res) => {
  res.json({ mensaje: '¡Hola' + req.params.nombre })
})
```

- La variable podría acabar en ? (parámetro opcional)


- Si mandamos una url del tipo:

```bash
GET http://localhost:8080/api?nombre=pepito
```

- El parámetro se recoge mediante *req.query*:

```js
app.get('/', (req, res) => {
  res.json({ mensaje: '¡Hola' + req.query.nombre })
})
```


## Parámetros por post

- ¡Necesitamos **middlewares**!
- **application/x-www-form-urlencoded**:
  - [body-parser](https://www.npmjs.com/package/body-parser): extrae los datos del body y los convierte en json

- **multipart/form-data**
  - [Busboy](https://www.npmjs.com/package/busboy) o [Multer](https://github.com/expressjs/multer)


## Ejemplo con body-parser

- Instalación:

  ```bash
  npm i -S body-parser@1.18.3
  ```

- body-parser actúa como **middleware**
- El código adicional será similar al siguiente:

  ```js
  var bodyParser = require('body-parser')
  app.use(bodyParser.urlencoded({ extended: true }))
  app.use(bodyParser.json())

  router.post('/', (req,res) => {
    res.json({mensaje: req.body.nombre})
  })
  ```


## Rutas de nuestra API

![Rutas API](./img/rutas-api.png)


## Enrutado del recurso cervezas

- Intenta configurar una API básica para el recurso cervezas en base a las rutas anteriores
  - Muestra por consola el tipo de petición
  - Muestra por consola el parámetro de entrada


## Solución enrutado recurso cervezas

- Fichero *app/routes/cervezas.js*:

```js
  var router = require('express').Router()
  router.get('/search', (req, res) => {
    res.json({ message: `Vas a buscar una cerveza que contiene ${req.query.q}` })
  }) // ¡¡¡¡antes que la ruta GET /:id!!!!
  router.get('/', (req, res) => {
    res.json({ message: 'Estás conectado a la API. Recurso: cervezas' })
  })
  router.get('/:id', (req, res) => {
    res.json({ message: `Vas a obtener la cerveza con id ${req.params.id}` })
  })
  router.post('/', (req, res) => {
    res.json({ message: 'Vas a añadir una cerveza' })
  })
  router.put('/:id', (req, res) => {
    res.json({ message: `Vas a actualizar la cerveza con id ${req.params.id}` })
  })
  router.delete('/:id', (req, res) => {
    res.json({ message: `Vas a borrar la cerveza con id ${req.params.id}` })
  })
  module.exports = router
```



## Arquitectura


## Situación actual

- Se han definido recursos
  - *Cervezas*, pero podría haber muchos más
  - Cada recurso se asocia a un enrutador
    - Por el momento *routes/cervezas*
  - A cada enrutador se asocian las rutas del recurso


## Arquitectura MVC

- Todas las rutas de un recurso se gestionan por un controlador.
 Cada ruta por un método del mismo
- El controlador es responsable de:
  - Acceder a los datos para leer o guardar
    - Delega en un modelo
      - Utilizaremos un ODM
      - Mongoose para MongoDB
  - Enviar datos de vuelta
    - Al ser JSON, lo haremos directamente desde el controller
    - No necesitamos la capa Vista :-)


## Fat model, thin controller

- El controlador recoge la lógica de negocio.
  - En nuestro caso es muy sencillo:
    - Recoge parámetros
    - Llama al modelo (obtener datos, guardar)
    - Devuelve json
- El modelo puede ser complejo.
  - Por ej. creación de tokens o encriptación de passwords en el modelo de Usuario
  - Es un código que puede ser reutilizado entre controladores



## Uso de controladores

- Desde nuestro fichero de rutas (*app/routes/cervezas.js*), se llama a un controlador, encargado de añadir, borrar o modificar cervezas.
- **Endpoint -> Recurso -> Fichero de rutas -> Controlador -> Modelo**


- Creamos un directorio específico para los controladores (*app/controllers*)
- Un controlador específico para cada recurso, por ej.  (*app/controllers/cervezasController.js*):
- Un método en el controlador por cada endpoint del recurso


## Mi primer método del controlador

- Vamos a crear el método index para que nos devuelva la lista de todas las cervezas.
- De entrada creamos nuestro controlador que devuelva un mensaje simple:

```js
//cervezaController.js

const list = (req, res) => {
  const page = req.query.page || 1
  return res.json({ mensaje: 'Lista de cervezas, página ' + page })
}

module.exports = {list}
```


## Tarea:

- Realiza los otros métodos básicos para realizar un CRUD sobre cervezas:
  - `show` para ver un elemento, ruta GET (/cervezas/:id)
  - `store` para crear, ruta POST  (/cervezas)
  - `update` para actualizar, ruta PUT (/cervezas/:id)
  - `update` para actualizar, ruta PUT (/cervezas/:id)



## Modelos

- Llega el momento de que el modelo entre es escena.
- Se trata de recoger los datos en un objeto
- Y de su persistencia en BBDD: leer, borrar, crear y actualizar


## Conexión:

- Nuestro modelo necesita usar una conexión a base de datos
- Usaremos el módulo [node-mysql2](https://github.com/sidorares/node-mysql2)

```js

const mysql = require('mysql2')

const connection = mysql.createConnection({
  user: 'root',
  password: 'Root.12345',
  host: 'localhost',
  database: 'cervezas'
})
```


## Configuración del controlador Cervezas

- Debemos definir los métodos siguientes:
  - search
  - list
  - show
  - create
  - update
  - remove


## Listar cervezas

- Desde el controlador: 

```js
const index = function (req, res) {
  console.log('Lista de cervezas')
  const sql = 'SELECT * FROM cervezas'
  connection.query(sql, (err, result, fields) => {
    if (err) {
      res.status(500).json(err)
    } else {
      console.log(fields)
      res.json(result)
    }
  })
}
```


## Listar cervezas con el modelo

- Al separar el acceso a la base de datos tenemos un problema
- ¿Cómo accedemos a los datos obtenidos en una función asícrona?
- solución: una función de callback debe devolver sus datos a través de otra función de callback.


### Modelo

```js
const connection = require('../config/dbconection')

const index = function (callback) {  
  const sql = 'SELECT * FROM cervezass'
  connection.query(sql, (err, result, fields) => {
    if (err) {
    callback(500, err)
} else {
    //   console.log(fields)
      callback(200, result, fields)
    }
  })
}
module.exports = { index };
```


### Controlador

```js
const index = function (req, res) { 
  console.log('Lista de cervezas')
  Cerveza.index((status, data, fields) => {
    console.log('data desde el controlador: ' + data)
    res.status(status).json(data)
  })
}
```


## Mostrar una cerveza


## Crear una cerveza


## Actualizar cerveza


## Borrar cerveza



# MongoDB

- Sistema de base de datos NoSQL
- Guarda los datos en BSON (Binary JSON)
- Es ideal para usarlo con Javascript
- Es muy usado en aplicaciones web: pila MEAN
- MongoDb, Express, Angular, Node.Js


## Instalación de MongoDB

- Utilizaremos **contenedores docker**:
  - Eliminamos conflictos en la máquina base
  - Podemos cambiar de versiones con facilidad
  - Nuestro proyecto es más portable


## Fichero de instalación mediante docker

- Definiremos un fichero *docker-compose.yml*
- Para ver que imagen necesitamos podemos consultar en el [docker hub](https://hub.docker.com/)

```yml
version: '3'
services:
  mongodb:
    hostname: mongodb
    container_name: mongodb
    image: mongo:4.0.1
    volumes:
      - ./mongodb-data:/data/db
    ports:
      - "27017:27017"
```


## Uso básico de MongoDb por consola

- Accedemos al contenedor:

```
docker exec -it mongodb bash
```
- Usar/crear una base de datos

```
use mibasededatos
```

- Ver la o las bases de datos

```
db   //show database
show dbs //ver todas
```


## Colecciones (Tablas)

- Creamos una colección de forma implícita

```
db.usuarios.insert({id: 1, nombre: "pepe"})
```

- O explícita
db.createCollection("cervezas")  //crear coleccion

```
show collections   /ver colecciones
```

- La borramos con drop:

```
db.cervezas.drop()  //borrar colección
db //ver cual es la db actual
db.dropDatabase()  //borrarla
```


## Consultas

Consultar, filtrar y ordenar:

```
db.cervezas.find().pretty()
db.cervezas.find({"precio": 1})
db.cervezas.find({"precio": { $lt : 1}})
db.cervezas.find().sort({precio:1})
```

modificar:

```
db.productos.update({id: 1}, { $set: {name: "Ambar Especial"}})
db.productos.update({id: 1}, { $set: {name: "Ambar Especial"}}, {multi: true})
```
Borrar:

```
db.cervezas.deleteOne({"id": 1}
db.cervezas.delete({"id": 1})
```


## MongoDB: Aplicaciones gráficas

- Vamos a usar [MongoDB Compass](https://www.mongodb.com/products/compass)
- Robo3T guarda un listado de conexiones a MongoDB
![Lista conexiones MongoDB](./img/compass-conexiones.png)



## MongoDB: Aplicaciones gráficas

- [Robo3T](https://robomongo.org/download)
  - Antes llamado **Robomongo**
  - El más extendido
  - [Instalación en ubuntu](https://steemit.com/linux/@kennethpham/how-to-install-robo-3t-former-robomongo-on-linux-ubuntu) 

<!-- ## Robo3T: Instalar y configurar

- Descargamos el paquete de [Robo3T](https://robomongo.org/download)(antes Robomongo)
- Instalamos y ejecutamos -->


## Robo3T: Conexiones a MongoDB

- Robo3T guarda un listado de conexiones a MongoDB

![Lista conexiones MongoDB](./img/robo3t-conexiones.png)


## Robo3t: Configurar conexión a MongoDB

- Creamos una nueva conexión a localhost y al puerto que hemos mapeado en Docker (27017)


## Conceptos en noSQL

![](img/SQL-MongoDB-correspondence.png)


## Schema en noSQL

![](img/no-sql-schema-vs-sql.jpg)


## Inserción de datos

- Utilizaremos el fichero *cervezas.json*, de la carpeta data

- Importar nuestro cervezas.json a una base de datos

```bash
mongoimport --db web --collection cervezas --drop --file cervezas.json --jsonArray
```

- Otra opción es mediante Robomongo:

```js
  db.getCollection('cervezas').insertMany(array de objetos)
```


- Para hacer una búsqueda por varios campos de texto, tendremos que hacer un índice:

```js
  $ mongo # para entrar en la consola de mongo
  use web; #seleccionamos la bbdd
  db.cervezas.createIndex(
    {
      "nombre": "text",
      "descripción": "text"
    },
    {
      name: "CervezasIndex",
      default_language: "spanish"
    }
  )
```

- Comprobamos que el índice esté bien creado

  ```js
  db.cervezas.getIndexes()
  ```



# Modelos con Mongo: Mongoose

- Instalaremos [mongoose](https://mongoosejs.com/) en vez de trabajar con el [driver nativo de MongoDB](https://mongodb.github.io/node-mongodb-native/) (se utiliza por debajo).
- Mongoose es un ODM (Object Document Mapper).
- Equivale a los ORM como Hibernate en Java o Eloquent en Php/Laravel


## Usar Mongoose

- Instalación

```
npm i -S mongoose
```

- Probamos la conexión en nuestro código

```js
  const mongoose = require('mongoose');
  mongoose.connect('mongodb://localhost/test');

  const Cat = mongoose.model('Cat', { name: String });

  const kitty = new Cat({ name: 'Zildjian' });
  kitty.save().then(() => console.log('meow'));
  ```


- Creamos el fichero *app/db.js* donde configuraremos nuestra conexión a base de datos mediante mongoose:

```js
const mongoose = require('mongoose')

const MONGO_URL = process.env.MONGO_URL || 'mongodb://localhost:27017/web'
mongoose.connect(MONGO_URL, { useNewUrlParser: true })

mongoose.connection.on('connected', () => {
  console.log(`Conectado a la base de datos: ${MONGO_URL}`)
})

mongoose.connection.on('error', (err) => {
  console.log(`Error al conectar a la base de datos: ${err}`)
})

mongoose.connection.on('disconnected', () => {
  console.log('Desconectado de la base de datos')
})

process.on('SIGINT', function() {
  mongoose.connection.close(function () {
    console.log('Desconectado de la base de datos al terminar la app')
    process.exit(0)
  })
})
```


- El código de la conexión usa [eventos](https://desarrolloweb.com/articulos/eventos-nodejs.html)
- Nuestro conector se prepara para recibir eventos con ".on". Los eventos los genera mongoose o al cerrar el proceso.

```js
var eventos = require('events');

var EmisorEventos = eventos.EventEmitter; 
var ee = new EmisorEventos(); 
ee.on('datos', function(fecha){ 
   console.log(fecha); 
}); 
setInterval(function(){ 
   ee.emit('datos', Date.now()); 
}, 500);
```


- En nuestro fichero *app/server.js* incluimos el fichero de configuración de bbdd:

```js
require('./db')
```

- Observa que no es necesario asignar el módulo a una constante
  - El módulo solo abre conexión con la bbdd
  - Registra eventos de mongodb
  - No exporta nada


- La conexión a bbdd se queda abierta durante todo el funcionamiento de la aplicación: 
  - Las conexiones TCP son caras en tiempo y memoria
  - Se reutiliza


## Modelos

- Un modelo mongoose debe definir un esquema
- Fichero *app/models/Cerveza.js*):

```js
const mongoose = require('mongoose')
const Schema = mongoose.Schema

const cervezaSchema = new Schema({
  nombre: {
    type: String,
    required: true
  },
  descripción: String,
  graduación: String,
  envase: String,
  precio: String
})

const Cerveza = mongoose.model('Cerveza', cervezaSchema)

module.exports = Cerveza
```


- Ahora podríamos crear documentos y guardarlos en la base de datos

```js
const miCerveza = new Cerveza({ name: 'Ambar' })
miCerveza.save((err, miCerveza) => {
  if (err) return console.error(err)
  console.log(`Guardada en bbdd ${miCerveza.name}`)
})
```


## Guardar documento

- Crea una cerveza nueva con todos los campos
- Comprueba por consola o desde [Robo3T](https://steemit.com/linux/@kennethpham/how-to-install-robo-3t-former-robomongo-on-linux-ubuntu) que en nuevo documento aparece en la colección Cervezas


## Solución guardar documento

```js
require('./db')

// en fichero app/server.js después de conectar a bbdd:

const miCerveza = new Cerveza({
  nombre: 'Ambar',
  descripción: 'La cerveza de nuestra tierra',
  graduación: '4,8º',
  envase: 'Botella de 75cl',
  precio: '3€'
})
miCerveza.save((err, miCerveza) => {
  if (err) return console.error(err)
  console.log(`Guardada en bbdd ${miCerveza.nombre}`)
})
```


## Uso de controladores

- Vamos a crear una vesión 2 de nuestra API, esta con MongoDb
- En nuestro fichero de rutas *routes.js*:

```js
const routerCervezasV2 = require('./routes/v2/cervezas.js')

router.use('/v2/cervezas', routerCervezasV2);

module.exports = router
```


- Creamos un directorio para los controladores v2 (*app/controllers/v2*)
- Un controlador, en este caso  (*app/controllers/v2/cervezasController.js*):
- Un método en el controlador por cada endpoint del recurso


## Configuración final del router Cervezas

```js
var router = require('express').Router()
var cervezasController = require ('../controllers/v2/cervezasController')

router.get('/search', (req, res) => {
  cervezasController.search(req, res)
})
router.get('/', (req, res) => {
  cervezasController.list(req, res)
})
router.get('/:id', (req, res) => {
  cervezasController.show(req, res)
})
router.post('/', (req, res) => {
  cervezasController.create(req, res)
})
router.put('/:id', (req, res) => {
  cervezasController.update(req, res)
})
router.delete('/:id', (req, res) => {
  cervezasController.remove(req, res)
})
module.exports = router
```



## Implementación del controlador


### Test desde el navegador o mediante Postman

- Comprobamos que se genera el listado de cervezas
- Comprobamos que se busca por keyword:

```bash
http://localhost:8080/api/cervezas/search?q=regaliz
```
...


## Configuración del controlador Cervezas

- Debemos definir los métodos siguientes:
  - search
  - list
  - show
  - create
  - update
  - remove


## Listar cervezas

```js
const Cerveza = require('../models/Cerveza')

const list = (req, res) => {
  Cerveza.find((err, cervezas) => {
    if (err) {
      return res.status(500).json({
        message: 'Error obteniendo la cerveza'
      })
    }
    return res.json(cervezas)
  })
}

module.exports = {
  list
}
```


## Listar cervezas por palabra clave

```js
const Cerveza = require('../models/Cerveza')
const search = (req, res) => {
  const q = req.query.q
  Cerveza.find({ $text: { $search: q } }, (err, cervezas) => {
    if (err) {
      return res.status(500).json({
        message: 'Error en la búsqueda'
      })
    }
    if (!cervezas.length) {
      return res.status(404).json({
        message: 'No hemos encontrado cervezas que cumplan esa query'
      })
    } else {
      return res.json(cervezas)
    }
  })
}

module.exports = {
  list,
  search
}
```


## Mostrar una cerveza

- BSON (datos de MongoDb) define el tipo usado para el campo *_id*
- Su nombre es [ObjectId](https://docs.mongodb.com/manual/reference/bson-types/#objectid), 12b en hexadecimal
```js
const Cerveza = require('../models/Cervezas')
const { ObjectId } = require('mongodb')
const show = (req, res) => {
  const id = req.params.id
  Cerveza.findOne({ _id: id }, (err, cerveza) => {
    if (!ObjectId.isValid(id)) {
      return res.status(404).send()
    }
    if (err) {
      return res.status(500).json({
        message: 'Se ha producido un error al obtener la cerveza'
      })
    }
    if (!cerveza) {
      return res.status(404).json({
        message: 'No tenemos esta cerveza'
      })
    }
    return res.json(cerveza)
  })
}
module.exports = {
  search,
  list,
  show
}
```


## Crear una cerveza

```js
const Cerveza = require('../models/Cervezas')
const create = (req, res) => {
  const cerveza = new Cerveza(req.body)
  cerveza.save((err, cerveza) => {
    if (err) {
      return res.status(400).json({
        message: 'Error al guardar la cerveza',
        error: err
      })
    }
    return res.status(201).json(cerveza)
  })
}
module.exports = {
  search,
  list,
  show,
  create
}
```


## Actualizar cerveza

```js
const update = (req, res) => {
  const id = req.params.id
  Cerveza.findOne({ _id: id }, (err, cerveza) => {
    if (!ObjectId.isValid(id)) {
      return res.status(404).send()
    }
    if (err) {
      return res.status(500).json({
        message: 'Se ha producido un error al guardar la cerveza',
        error: err
      })
    }
    if (!ObjectId.isValid(id)) {
      return res.status(404).send()
    }
    if (!cerveza) {
      return res.status(404).json({
        message: 'No hemos encontrado la cerveza'
      })
    }

    Object.assign(cerveza, req.body)

    cerveza.save((err, cerveza) => {
      if (err) {
        return res.status(500).json({
          message: 'Error al guardar la cerveza'
        })
      }
      if (!cerveza) {
        return res.status(404).json({
          message: 'No hemos encontrado la cerveza'
        })
      }
      return res.json(cerveza)
    })
  })
}
module.exports = {
  search,
  list,
  show,
  create,

```


## Borrar cerveza

```js
const Cerveza = require('../models/Cervezas')
const { ObjectId } = require('mongodb')
const remove = (req, res) => {
  const id = req.params.id

  Cerveza.findOneAndDelete({ _id: id }, (err, cerveza) => {
    if (!ObjectId.isValid(id)) {
      return res.status(404).send()
    }
    if (err) {
      return res.json(500, {
        message: 'No hemos encontrado la cerveza'
      })
    }
    if (!cerveza) {
      return res.status(404).json({
        message: 'No hemos encontrado la cerveza'
      })
    }
    return res.json(cerveza)
  })
}

module.exports = {
  search,
  list,
  show,
  create,
  update,
  remove
}
```



##  Mongoose II: La mangosta

- Hemos completado el ciclo CRUD pero no hemos pasado de puntillas sobre Mongo y Mongoose
- Vamos a estudiar como construir nuestras propias colecciones y modelos Mongo/Mongoose


## Modelos y esquemas

- https://mongoosejs.com/docs/models.html
- Mongoose es un Object Document Mapper (ODM). 
- Permite definir objetos con un esquema fuertemente tipado que se asigna a un documento MongoDB.
- Mongoose actualmente contiene ocho tipos de datos o SchemaTypes


Son:
- Date (Fecha)
- Buffer
- Boolean (Booleano)
- Mixed (Mixto)
- ObjectId
- Array (Matriz)


### Sobre cada tipo podemos definir:

- un valor predeterminado
- una función de validación personalizada
- indicar si es requerido
- una función get que le permite manipular los datos antes de que se devuelva como un objeto
- una función de conjunto que le permite manipular los datos antes de guardarlos en la base de datos
- crear índices para permitir que los datos se obtengan más rápido


### Sobre los String podemo:

- convertirlo a minúsculas
- convertirlo a mayúsculas
- recortar datos antes de guardar
- una expresión regular que puede limitar los datos que se pueden guardar durante el proceso de validación
- una enumeración que puede definir una lista de cadenas que son válidas


### Buffer

- Tipo de datos binario
- Por ejemplo para guardar un pdf o una foto en Mongo


### Mixed

  - Todo vale
  - Ojo! perdemos posibilidades con su uso como el de la validación


### ObjectId

- Campo de 12B usado para identificar registrso. Se compone de :
  - 4B timestap desde época unix
  - 5B aleatorios 
  - 3B contador (inicio aleatorio)


### Array 

- El tipo de datos Array le permite almacenar matrices similares a JavaScript. 
- Con un tipo de datos Array, podemos realizar operaciones de matriz JavaScript como push, pop, shift, slice, etc.


## Definir un esquema

- Ejemplo 

```js
var mongoose = require('mongoose');

var userSchema = mongoose.Schema({
    firstName: String,
    lastName: String
});

var User = mongoose.model('User', userSchema);

```


Podemos usar el nombre como un objeto y añadir una fecha de creación:

```js
var userSchema = mongoose.Schema({
    name: {
      firstName: String,
      lastName: String
    },
    created: Date
});
```


Ejemplo más completo:

```js
var authorSchema = mongoose.Schema({
    _id: mongoose.Schema.Types.ObjectId,
    name: {
            firstName: String,
        lastName: String
    },
    biography: String,
    twitter: String,
    facebook: String,
    linkedin: String,
    profilePicture: Buffer,
    created: { 
        type: Date,
        default: Date.now
    }
});
```


## Crear y guardar modelos

```js
var Author = mongoose.model('Author', authorSchema);
```


Añadimos validación:

```js
var authorSchema = mongoose.Schema({
    _id: mongoose.Schema.Types.ObjectId,
    name: {
        firstName: {
            type: String,
            required: true
        },
        lastName: String
    },
    biography: String,
    twitter: {
        type: String,
        validate: {
            validator: function(text) {
                return text.indexOf('https://twitter.com/') === 0;
            },
            message: 'Twitter handle must start with https://twitter.com/'
        }
    },
    facebook: {
        type: String,
        validate: {
            validator: function(text) {
                return text.indexOf('https://www.facebook.com/') === 0;
            },
            message: 'Facebook must start with https://www.facebook.com/'
        }
    },
    linkedin: {
        type: String,
        validate: {
            validator: function(text) {
                return text.indexOf('https://www.linkedin.com/') === 0;
            },
            message: 'LinkedIn must start with https://www.linkedin.com/'
        }
    },
    profilePicture: Buffer,
    created: { 
        type: Date,
        default: Date.now
    }
});
```


### El fichero de modelo

```js
var mongoose = require('mongoose');
 
var bookSchema = mongoose.Schema({
    _id: mongoose.Schema.Types.ObjectId,
    title: String,
    ...
});
 
var Book = mongoose.model('Book', bookSchema);
 
module.exports = Book;
```


```js
var mongoose = require('mongoose');
 
var bookSchema = mongoose.Schema({
    _id: mongoose.Schema.Types.ObjectId,
    title: String,
    summary: String,
    isbn: String,
    thumbnail: Buffer,
    author: { 
        type: mongoose.Schema.Types.ObjectId, 
        ref: 'Author'
    },
    ratings: [
        {
            summary: String,
            detail: String,
            numberOfStars: Number,
            created: { 
                type: Date,
                default: Date.now
            }
        }
    ],
    created: { 
        type: Date,
        default: Date.now
    }
});
 
var Book = mongoose.model('Book', bookSchema);
 
module.exports = Book;
```


### Validadores (validators)

- Todos los tipos tienen incorporado el validador *required*
- Los números tienen *min* y *max*
- Los textos (String), *enum*, *match*, *minlength*, and *maxlength* validators.


### Crear documentos:

```js
var Tank = mongoose.model('Tank', yourSchema);

var small = new Tank({ size: 'small' });
small.save(function (err) {
  if (err) return handleError(err);
  // saved!
});

// or

Tank.create({ size: 'small' }, function (err, small) {
  if (err) return handleError(err);
  // saved!
});
```


### Actualización

Buscar + modificar + salvar

```js
Author.findById('59b31406beefa1082819e72f', function(err, author) {
    if (err) throw err;
     
    author.linkedin = 'https://www.linkedin.com/in/jamie-munro-8064ba1a/';
     
    author.save(function(err) {
        if (err) throw err;
         
        console.log('Author updated successfully');
    });
});
```


O todo en uno con findByIdAndUpdate

```js 
Author.findByIdAndUpdate('59b31406beefa1082819e72f', 
    { linkedin: 'https://www.linkedin.com/in/jamie-munro-8064ba1a/' }, 
    function(err, author) {
        if (err) throw err;
     
        console.log(author);
});
```


## Ejercicio 1:

- Crea una una colección  productos en la BBDD web
- Crea crea un modelo para dicha coleccion.
    - name: requerido, máximo 20 caracteres
    - price: requerido, numérico
    - description: máximo 255 caracteres.
    - created: fecha de creación
- Crea las rutas y el controlador para gestionar los productos.



## Autenticación JWT

- REST significa 
- Sin estado implica no guardar en el servidor ninguna estructura de datos de sesión ni usar cookies.
- La autentinticación en este entorno está basada en tokens.


### Login: rutas y controlador

- Necesitamos crear un fichero de rutas y un controlador.
- Rutas:

```js
const express = require('express')
const router = express.Router()
const userController = require('../../controllers/v2/userController')

router.post('/', userController.register)
  
module.exports = router
```


- Y controlador (probar):

```js

const register = (req, res) => {
  return res.json({ message: 'alta de usuario' })
}

module.exports = {
  register
}

```


### El modelo de Usuario

- Vamos a realizar autenticación basada en email + contraseña
  - La contraseña debe ir encriptada con *bcrypt*
  - El email debe ser único y en minúsculas
- Además guardaremos:
  - Un nombre de usuario
  - Fecha de creación
  - Fecha de último login


- Nuestro modelo: imports/exports

```js
const mongoose = require('mongoose')
const Schema = mongoose.Schema
//cifrado de passwords.
const bcrypt = require('bcrypt-nodejs')

//aquí falta lo importante

module.exports = mongoose.model('User', UserSchema)
```


- Nuestro modelo: esquema

```js
const UserSchema = new Schema({
  email: {
    type: String,
    unique: true,
    lowercase: true // lo guardará en minúsculas
  },
  name: String,
  password: { type: String, select: false },
  date: { type: Date, default: Date.now() },
  lastLogin: Date
})

```


#### Modelos y middleware

- Son funciones invocadas antes o después (**pre** o **post**) de la ejecución de funciones asíncronas.
- Un documento cuenta con las siguientes funciones:
  - *validate*
  - *save*
  - *remove*
  - *init*
- Podemos añadir funciones pro/post de las anteriores
- Este código lo añadiremos al modelo.


- Nuestro modelo: middleware *pre - al salvar*
  - Hacemos uso de los [middleware](https://mongoosejs.com/docs/middleware.html) de Mongoose 
  - NOTA: no usar aquí función flecha.

```js
UserSchema.pre('save', function (next) {
  const user = this
  if (!user.isModified('password')) return next()

  bcrypt.genSalt(10, (err, salt) => {
    if (err) return next(err)

    bcrypt.hash(user.password, salt, null, (err, hash) => {
      if (err) return next(err)

      user.password = hash
      next()
    })
  })
})
```


### Registro y Login

- Controlador userController

```js
//npm i -S jwt-simple
const mongoose = require('mongoose')
const User  =require('../models/User')

function register() {

}

function login() {
    
}

module.exports = { register, login }
```


- Función *register* en el controlador:

```js
const register = (req, res) => {
  const user = new User({
    email: req.body.email,
    name: req.body.name,
    password: req.body.password
  })

  user.save(err => {
    if (err) res.status(500).send({ message: `Error al crear usuario: ${err}` })
    // servicejwt nos va a crear un token
    return res.status(200).send({ token: servicejwt.createToken(user) })
  })
}

```


### Emitir el token

- Vamos a crear una función en el fichero '/services/servicejwt.js'
- Mediante la librería [*jwt-simple*](https://jwt.io/introduction/) vamos a generar nuestro token.

```js
const jwt = require('jwt-simple') // módulo Json Web Token
const moment = require('moment') // módulo para fechas
const config = require('../config/config') // definimos una constante "secreta"

function createToken (user) {
  const payload = {
    sub: user._id, // no es muy seguro pero lo simplificamos así
    email: user.email
    iat: moment().unix(), // fecha creación
    exp: moment()
      .add(30, 'days')
      .unix() // valido 1 mes
  }

  return jwt.encode(payload, config.SECRET)
}

module.exports = { createToken }
```


### Proteger rutas:

- Añadimos este middleware en el enrutador de cervezas o productos. Así protegemos todas las rutas:

```js
const moment = require('moment')

router.use((req, res, next) => {
  console.log(req.headers.authorization)
  if (!req.headers.authorization) {
    return res.status(403).send({ message: 'No tienes permiso' })
  }
  const token = req.headers.authorization.split(' ')[1]

  try {
    payload = servicejwt.decodeToken(token)
  } catch (error) {
    return res.status(401).send(`${error}`)
  }
  res.status(200).send({ message: 'con permiso' })
})
```


- Mejor refactorizar en una función :

```js
const auth = function (req, res, next) {
  console.log(req.headers.authorization)
  if (!req.headers.authorization) {
    return res.status(403).send({ message: 'No tienes permiso' })
  }
  const token = req.headers.authorization.split(' ')[1]
  try {
    payload = servicejwt.decodeToken(token)
  } catch (error) {
    return res.status(401).send(`${error}`)
  }
  res.status(200).send({ message: 'con permiso' })
}
```


Y ahora lo usamos:

```js
//para proteger todas las rutass:
router.use(auth)

//para proteger una sóla ruta
router.get('/', auth, (req, res) => {
  productController.index(req, res)
})

//o así
router.get('/', auth, productController.index})
```


### Tarea: Login

- Una vez registrado tenemos acceso para cierto periodo
- Nos interesa generar nuevos tokens (login) en nuevas máquinas o tras caducar el que tenemos.
- Debemos implementar un método login para obtener un nuevo token.
- Recibimos usuario + contraseña
- Verificamos si la contraseña es válida. Para hacerlo debemos comparar contraseñas (busca [aquí](https://www.npmjs.com/package/bcrypt))
  - Si es válida generamos un token y lo entregamos
  - Si no status 401

<!-- ```js
function login(req, res) {
  User.find({email. req.body.email}, (err, user) => {
    if (err) return res.status(500).json({message: err})
    if (!user) return res.status(404).json({message: 'no existe el usuario})
    req. user = user

    res.status(200).send({
      message: 'login ok',
      token: servicejwt.createToken
    })
  })
}
``` -->


### Tarea:

- Crea un directorio *middlewares* y crea un middleware llamado *auth.js*
- Coloca en él el código de autenticación
- Refactoriza el ocntrolador de cervezas para que use ese middleware
- Úsa el middleware en el controlador de productos.

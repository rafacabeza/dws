# Relaciones

* En general diremos que una tabla de la base de datos supone usar una clase modelo en nuestro código php.

* Esa idea general puede matizarse: usaremos una clase modelo para cada entidad de nuestro modelo ER. Las tablas generadas por relaciones varios a varios no las trataremos como una clase.

* La gran duda es cómo tratar las claves ajenas que reflejan las relaciones entre tablas.

* Por ejemplo, si yo tengo una tabla \`products\` asociada a otra tabla `cathegories`, es deseable que en la clase Product, exista un atributo `cathegory`, y en la clase Cathegorie exista un atributo `products` que contenga la lista de productos asociados a una categoría en particular.

* Para lograr esto vamos a usar el método mágico `__get()`  disponible en php.
* Y crearemos un método en la clase modelo que nos cree el atributo deseado: 
  * En Cathegory usaremos un método product\(\)
  * En Product usaremos un método cathegories\(\)




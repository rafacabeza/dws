## Ramificaciones o ramas (_branches_)

- Ramificar consiste en usar distintas líneas de trabajo en un repositorio. 
- Sólo vamos a plantear su uso en el repositorio local.

### Uso de ramas.

-Listar las ramas:
```
git branch
```

- Crear una rama nueva llamada <branch>
```
git branch <branch>
```

- Borrar ramas:
```
git branch -d <branch>     //Borrado seguro. Error si hay cambios sin comprometer.
git branch -D <branch>    //Borrado forzado, no seguro.
```

- Renombrar la rama actual
```
git branch -m <branch>
```


### De rama en rama
- Cambiar de rama
```
git checkout <existing-branch>
```

- Crear rama y saltar a la vez
```
git checkout -b <existing-branch>
```

- Fundir ramas

 - Supongamos que estamos en la ramaA
 - Queremos fundirla con la ramaB
```
git merge ramaA
```
 Veamos un ejemplo donde trabajamos en una nueva funcionalidad usando una rama:

```
# Iniciar nueva funcionalidad
git checkout -b ramaNueva master

# Editamos ficheros
git add <file>
git commit -m "Iniciar funcionalidad"

# Editamos mas ficheros
git add <file>
git commit -m "Funcionalidad acabada"

# Develop the master branch
git checkout master

# Edit some files
git add <file>
git commit -m "Fundir cambios estables a master"

# Merge in the new-feature branch
git merge ramaNueva
git branch -d ramaNueva

```

### Trabajo en equipo
- Vamos a ver como trabajar en equipo utilizando Bitbucket.org y vamos a ver como hacerlo para un proyeto Laravel. Esto seria similar con cualquier otro framework.
- A dia de hoy, enero/2017, bitbucket permite crear equipos de trabajo gratis de hasta 5 miembros:
 - Se trata de crear un grupo de trabajo y añadir los miembros del mismo.
 - Creamos un proyecto dentro del grupo de trabajo. Un proyecto puede contener uno o más repositorios y puede ser público o privado.
 - Creamos un repositorio dentro del proyecto. 
#### Creación del proyecto Laravel:
 - En nuestra máquina local creamos nuestro proyecto: `laravel new miproyecto`
 - Entramos en el directorio: `cd miproyecto`
 - Iniciamos git: `git init`
 - Añadimos todo: `git add .` o `git add -A`
 - Primer commit: `git commit -m "Laravel"`
 - Añadimos el reposio ` git remote add origin  ruta_del_repositorio_remoto`
 - Subimos el código: `git push -u origin --all`
####  Escribir código
 
- Para esccribir código debemos usar el sistema de ramas explicado anteriormente.
- Cuando queremos añadir una funcionalidad debemos hacer lo siguiente:
 - Crear una nueva rama:  
 ```
 git branch nuevafuncion
 git checkout nuevafuncion
 ```
 - Escribir nuestro código. Lo ideal es no modificar ficheros que otro usuario pueda modificar.
 - Cambiar a la rama master
 ```
git checkout master
```
 - Descargar el estado de la rama master remota (origin/master)
```
 git pull
 ```
 - Fundir con la rama _nuevafuncion_ y subir al repositorio
 ```
 git merge nuevafuncion
 git push
 ```
 
 
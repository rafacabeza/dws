## Creación de repositorios
- Basta crear un directorio, acceder a él mediante la consola y ejecutar `git init`
- Este comando crear el directorio `.git` que guarda toda la información que necesita git. Ese directorio es propiamente el repositorio.
- El repositorio local tiene todas las funcionalidades pero no permite compartir código ni centralizar el almacenamiento y control del mismo.
- Veremos que otra forma es clonar un repositorio existente remoto.

## Estado: tracked, untracked, modified,  ....
- Para git un fichero puede:
 - Ser nuevo (untracked) o antigua (tracked), además puede ser modificado (modified) o borrado (deleted).
 - Estar preparado para su compromiso (commit)
 - Estar comprometido.

- Fichero readme.md. Fichero de presentación del repositorio. Prueba con él.
 
- Consulta del estado actual:

- Añade ficheros y comprueba el estado

- Compromete el repositorio

## Deshacer 
Tenemos varios comandos git: checkout, reset, revert y clean.

### Sacar de preparado
- Quitar del estado _preparado_: 
 `git reset HEAD` quita todos los ficheros
 `git reset HEAD <file>` quita un fichro concreto 
 


### Descartar cambios actuales
- Descartar todos los cambios locales. Es decir todos los ficheros modificados. Ojo, no elimina los ficheros nuevos.

 `git reset --hard HEAD`
 
- Idem para un fichero concreto. Devuelve un fichero al último commit (lo que es lo mismo el HEAD).

  `git checkout HEAD <file>`
  `git checkout -- <file>`


### CHECKOUT. Moverse entre commits
- Ir a un commit
`git checkout <commit>`
 
 - Volver al HEAD
 `git checkout HEAD`
 
 - Si sólo queremos recuperar un fichero
`git checkout <commit> <file>`
 
### REVERT. Recuperar un commit antiguo y guardar los más recientes.


### RESET. Recuperar un commit antiguo y desechar los más recientes.

- no deseable
`git reset <commit>`
 
### CLEAN. Limpiar ficheros nuevos 
 
 <!-- 
- Revertir un commit produciendo uno nuevo con cambios contrarios:

  `git revert <commit>`

- Resetear el HEAD a un commit previo ... 
 - ... y descartar los commit posteriores
 
 `$ git reset --hard <commit>`
 - ... y preservar los cambios como cambios _unstaged_

 `$ git reset <commit>`

 - ... y preservar los cambios no comprometidos
  `$ git reset --keep <commit>`
 
 --<
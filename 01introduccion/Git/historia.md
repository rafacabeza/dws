## Historia

### Revisar

Podemos revisar la información de commits con _git log_. Ejemplos:

```
//orden básica
git log 
//últimas 5 revisiones
git log -5
//poniendo información en una línea
git log --pretty=oneline
git log --oneline
```

* Existen multitud de posibilidades más como limitar la salida a unas fechas o autor, mostrar una gráfica de commits, ... Para esos casos mira aquí:
  * [https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-log](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-log)
  * [https://git-scm.com/book/es/v1/Fundamentos-de-Git-Viendo-el-histórico-de-confirmaciones](https://git-scm.com/book/es/v1/Fundamentos-de-Git-Viendo-el-histórico-de-confirmaciones)

### Revisando estados antiguos

* Con _git checkout_ podemos recuperar el repositorio o ficheros determinado en un estado anterior.  
* Recuperamos un fichero
  ```
  git checkout <commit> <file>
  ```
* O todo el repositorio

  ```
  git checkout <commit>
  ```

* Veamos un ejemplo

```
git log --oneline
//resultado de lo anterior
b7119f2 Continue doing crazy things
872fa7e Try something crazy
a1e8fb5 Make some important changes to hello.py
435b61d Create hello.py
9773e52 Initial import


//recuperando el tercer commit.
git checkout a1e8fb5
```

### Deshacer cosas:

Sacar un fichero del estado preparado:

```
git reset HEAD <file>
```

Deshacer cambios sobre un fichero. Ojo que no hay quien los recupere después:

```
git checkout -- <file>
```

### Tagging

* Consiste en poner nombre a un commit.

[https://confluence.atlassian.com/bitbucket/use-repo-tags-321860179.html](https://confluence.atlassian.com/bitbucket/use-repo-tags-321860179.html)


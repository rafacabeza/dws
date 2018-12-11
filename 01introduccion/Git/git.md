## Git.
SCV o Sistema de Control de versiones
Tomado del siguiente 
Manual GIT
 en 
https://git-scm.com/book/es/v2
:
Un control de versiones es un sistema que registra los cambios realizados en un archivo o conjunto de archivos a lo largo del tiempo, de modo que puedas recuperar versiones específicas más adelante.
SCV distribuidos y centralizados
El enfoque tradicional de los SCV es el de un sistema centralizado. Es el caso de los muy utilizados CVS O Subversion. Esto es fácil de administrar pero obliga a que el servidor siempre esté disponible.
Git, como Mercurial, Bazaar o Darcs, es un sistema distribuido. Los clientes mantienen una copia completa de todo el repositorio.  Esto permite hacer cambios sin estar conectados. En algún momento deben sincronizarse los clientes y el servidor o serviodres.
Git fue creado por Linus Torvaldas, creado de Linux, para dar soporte al desarrollo de este SO.
Git
Las características de git:
Es distribuido
Sencillo y rápido
Permite grandes proyectos con mulititud de ramas
Usa copias completas. Hace copia de los ficheros modificados y enlaces a los no modificados.
Asegura la integridad. Realiza un hash SHA-1 a cada fichero. Si un fichero es alterado git lo detecta.
Instalación en Linux
Instalación de git:
sudo apt-get update

sudo apt-get install git
Configuración necesaria para cada commit que haga:
git config --global user.name "Your Name"

git config --global user.email "youremail@domain.com"
Opcionalmente el editor (si no me gusta el que hay por defecto):
git config --global core.editor vi
Configuración de git
Todo lo aquí contado puede verse con más detalle en el citado manual.
Git tiene 3 niveles de configuración, cada nivel sobreescribe el anterior:
Para todos los usuarios: /etc/gitconfig
Para un usuario: ~/.gitconfig (opción --global)
Para un repositorio: .git/config
Para ver los parámetros configurados:
git config --list
Viene bien tener una 
chuleta de comandos de Git

Configuración de GitHub
Nos registramos en 
Github

Accedemos a nuestra cuenta
Vamos a los settings y asociamos una ssh-key
Evitaremos introducir usuario/contraseña en cada git push
Como creamos nuestra ssh-key:
ssh-keygen
Copiaremos el contenido de ~/.ssh/id_rsa.pub a una nueva clave ssh en GitHub

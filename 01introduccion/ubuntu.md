# Sistema Operativo

Vamos a instalar la versión actual de Ubuntu \(o Lubuntu o Mint si tenemos limitaciones con nuestra máquina anfitriona\).

Descargamos la ISO correspondiente e instalamos la máquina. Si no sabes cómo hacerlo uno de los siguientes enlaces puede ayudarte:

* [Video explicativo](https://www.youtube.com/watch?v=uV5boDESAe0)
* [Página tutorial](http://es.wikihow.com/instalar-Ubuntu-en-VirtualBox)

Para facilitar las cosas en clase daremos de alta el usuario alumno u contraseña alumno.

## Instalar Guest Additions

Necesitamos instalar este paquete para permitir una pantalla con definición aceptable, compartir carpetas con la máquina anfitriona o el portapapeles. Para la instalación, hacemos login con nuestro usuario alumno. Y abrimos terminal.

Debemos actualizar el sistema:

```
sudo apt-get update
sudo apt-get upgrade
```

Instala los siguientes paquetes requeridos

    sudo apt-get install build-essential module-assistant`

Preparar el sistema para montar módulos del kernel corriendo lo siguiente:

```
sudo m-a prepare
```

Ahora en el menú dispositivos de la máquina virtual inserta el CD de instalación de las _Guest Additions_. Elige explorar el contenido.

Corre el comando

```
sudo sh /media/alumno/VBOXADDITIONS_5.1.4_110228/VBoxLinuxAdditions.run
```

La ruta puede cambiar con la versión de las Guest Additions.

### Añadir carpetas compartidas.

Dentro del menú _Dispositivos_ de la máquina virtual accede al diálogo de carpetas compartidas. Ahí añade las carpetas que necesites. Marca la casilla de automontar.

Para acceder a las carpetas compartidas, además, se requieren permisos. Es necesario añadir el usuario alumno al grupo de VirtualBox

```
# usermod -g vboxsf alumno
```

Otra posibilidad es hacer el montaje de forma manual como se explica [aquí](http://www.javiercarrasco.es/2012/07/20/carpeta-compartida-entre-windows-anfitrion-y-ubuntu-en-virtualbox/)

# Servidor Web

Para poder programar y probar código del lado del servidor necesitaremos un servidor capaz de entregar, servir, documentos HTML así como de ejecutar códgigo de alguna de las tecnologías ya citadas.

Tradicionalmente Apache es el servidor por antonimasia. Existen otros como Microsoft IIS o Nginx pero nosotros instalaremos el primero.

## Configurar nuestro servidor Web Apache

Como ya hemos dicho en Ubuntu usaremos la habitual tecnología LAMP \(Linux + Apache + MySql + Php\). Esto requiere instalar: 
Intalar Apache + PHP + MySql. Veamos por partes.

Instalamos Apache:

```
$ apt-get install apache2
```

Instalamos el interpreta de PHP.

```
$ sudo apt-get install php7.0 libapache2-mod-php7.0 
```

Instalamos MySql.

```
$ sudo apt-get install mysql-server php7.0-mysql
```

Y para acabar añadimos algunos módulos que podemos necesitar:

```
$ sudo apt-cache search php7-*
$ sudo apt-get install php7.0-curl php7.0-json
```

Para parar, iniciar o reiniciar el servicio web:

```
 sudo service apache2 stop
 sudo service apache2 start
 sudo service apache2 restart
```

# Navegador Web

Ve a la página de Google Chrome, descárgalo e instálalo.

# Sublime Text.

Versión 3. Vamos a usar este editor de texto para codificar PHP. Déscargate el paquete e instálalo desde la [página oficial](https://www.sublimetext.com/3).

Lo podemos instalar en Ubuntu desde la Terminal añadiendo un PPA con el siguiente comando:

`sudo add-apt-repository ppa:webupd8team/sublime-text-3`

Ahora tenemos que recargar los repositorios con este otro comando:

`sudo apt-get update`

Y finalmente podemos instalar Sublime Text 3 en Ubuntu con este otro comando:

`sudo apt-get install sublime-text-installer`

## Configuración

Una guía bastante completa de posibles paquetes en: 
[https:\/\/manuais.iessanclemente.net\/index.php\/Tutorial\_sobre\_editor\_Sublime\_Text\_3\#Plugin\_BracketHighlighter\_en\_Sublime\_Text\_3](https://manuais.iessanclemente.net/index.php/Tutorial_sobre_editor_Sublime_Text_3#Plugin_BracketHighlighter_en_Sublime_Text_3)

### Package Control

* Iremos a la siguiente página: [https:\/\/sublime.wbond.net\/](https://sublime.wbond.net/)
* Pulsaremos en la opción Install Now &gt;
* Seleccionamos el texto indicado para nuestra versión y lo copiamos.
* Vamos a la consola de Sublime Text y lo pegamos.

### Paquetes que debemos instalar

Para instalar paquetes, pulsamos CTRL+ Shift + P y tecleamos install y seleccionamos Package Control: Install Package.

Ahí ya podemos teclear el nombre del paquete para su instalación. Instalamos los siguientes:

* "AutoFileName"
* "BracketHighlighter"
* "Phpcs"
* "SublimeCodeIntel"
* "SublimeLinter"
* "SublimeLinter-php"
* "SyncedSideBar"

### PHPCS \(Php Conde Sniffer)

Este paquete requiere más instalaciones:

Debemos instalar algunas utilidades que usa este paquete:

* `wget https://github.com/FriendsOfPHP/PHP-CS-Fixer/releases/download/v1.12.2/php-cs-fixer.phar -O php-cs-fixer`
* `sudo mv  php-cs-fixer /usr/bin/`
* `sudo apt-get install php_codesniffer`
* `sudo apt-get install `
* `sudo apt-get install phpmd`

Debemos añadir lo siguiente al fichero de configuración de usuario de PHPCS.

```
{
 "phpcs_php_prefix_path": "/usr/bin/php",
 "phpcs_executable_path": "/usr/bin/phpcs",
 "php_cs_fixer_executable_path": "/usr/bin/php-cs-fixer",
 "phpcbf_executable_path": "/usr/bin/phpcbf",
 "phpcs_php_path": "/usr/bin/php",
 "phpmd_executable_path": "/usr/bin/phpmd"
}
```


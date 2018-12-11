# Instalar entorno de trabajo Java EE en Ubuntu o Mint

## Instalar Java (JDK)
- Instalación de JDK con Apt-Get. JDK y JRE:

https://www.digitalocean.com/community/tutorials/instalar-java-en-ubuntu-con-apt-get-es

- Instalar JDK 9 de distintas maneras:

https://howtoprogram.xyz/2016/09/29/install-oracle-java-9-on-ubuntu-16-04/


En resumen:

- Instalar Java 8 porque Netbeans no soporta Java 9.

- Añadir repositorio
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update

- Instalar JDK
```
sudo apt-get install oracle-java8-installer
```

- Comprobar versión java:
```
java -version
```

- Poner la versión 8 como versión por defecto:
```
sudo apt-get install oracle-java8-set-default
```

- Podemos ver todas las versiones disponibles para poder elegir entre ellas:
```
sudo update-alternatives  --config java
```

- Ahí vemos la ruta de instalación. El siguiente paso es necesario para algunas aplicaciones. Se trata de definir un par de variables de sistema:
```
export JAVA_HOME=/usr/lib/jvm/java-8-oracle/jre/bin/java
export PATH="$PATH:$JAVA_HOME/bin"
```

## Instalar Netbeans
- Descargar netbeans desde:

https://netbeans.org/downloads/8.1/

- O descargamos con wget:

http://download.netbeans.org/netbeans/8.1/final/bundles/netbeans-8.1-linux.sh

- Instalamos:
```
chmod +x netbeans-8.1-linux.sh 
./netbeans-8.1-linux.sh
```

- La instalación base usa sólo Glassfish, si queremos Tomcat debemos hacer instalación personalizada.
- Pregunta por la ubicación del JDK:
```
/usr/lib/jvm/java-8-oracle
```

## Instalar Eplipse

- En este curso optamos por Eclipse Neon
- Descargamos Eclipse
- Lo descomprimimos

Complementos
- Emmet
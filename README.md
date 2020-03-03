


# Despliegue DCA
## Instalación LAMP
### L - Linux
El sistema operativo que se encuentra corriendo en el DCA es CentOS 7, por lo que no es necesario realizar una instalación. 
Para asegurarnos que el ambiente se encuentra actualizado, corremos los siguientes comandos

    $ yum clean all
    $ yum update
    
### A - Apache
Vamos a usar Apache como software para el webserver, para instalarlo ejecutamos el comando

    $ yum -y install httpd
Debemos abrir los puertos en el Firewall  
  
    $ firewall-cmd --permanent --add-service=http -add-service=https
    $ firewall-cmd --reload


Iniciamos el servicio de apache y lo configuramos para que se inicie cuando el servidor arranque

    $ systemctl start httpd
    $ systemctl enable httpd
    
   ###  M - MySQL/MariaDB
Para manejar la base de datos de nuestro sitio web, se usará MySQL y MariaDB.

Instalamos MariaDB y realizamos la configuración de seguridad inicial

    $ yum -y install mariadb-server
    $ systemctl start mariadb
    $ mysql_secure_installation


Habilitamos el servicio de MariaDB 

    $ systemctl enable mariadb

### P - PHP
Para poder entregar contenido, usaremos PHP. Existen múltiples versiones de PHP disponibles en los repositorios de CentOS. En este caso instalaremos el paquete php72-php ya que este incluye unos módulos necesarios para la ejecución correcta de PHP.

Añadimos el repositorio EPEL y lo habilitamos

    $ yum -y install epel-release
    $ yum-config-manager --enable remi-php72
Instalamos el paquete de PHP

    $ yum update
    $ yum install php72-php 
  Tenemos que crear un link simbólico para el archivo binario de PHP

     $ ln -s /opt/remi/php72/root/usr/bin/php /usr/bin/php
   Instalamos el módulo de PHP para MySQL/MariaDB
   

    $ yum -y install php72-php-mysqlnd
Finalmente, reiniciamos el servicio de Apache para que funcione con PHP

    $ systemctl restart httpd
## Instalación y configuración de WordPress
### Configuración de la base de datos
Realizamos el login a mysql
 
    mysql -u root -p
Procedemos a crear la base de datos de nuestro WordPress y un usuario para ésta, además de proporcionarle permisos para el acceso. Cambiamos los campos de username y contraseña con la información necesaria.

    CREATE DATABASE wordpress-db;
    CREATE USER  <username>@localhost IDENTIFIED BY '<contraseña>';
    GRANT ALL PRIVILEGES ON wordpress-db.* TO <username>@localhost IDENTIFIED BY '<contraseña>';
Ejecutamos el siguiente comando para que MySQL acepte los cambios, y salimos de MySQL

    FLUSH PRIVILEGES;
    exit
## Instalación y configuración de WordPress
### Configuración de la base de datos
Realizamos el login a mysql
 
    mysql -u root -p
Procedemos a crear la base de datos de nuestro WordPress y un usuario para ésta, además de proporcionarle permisos para el acceso. Cambiamos los campos de username y contraseña con la información necesaria.

    CREATE DATABASE wordpress-db;
    CREATE USER  <username>@localhost IDENTIFIED BY '<contraseña>';
    GRANT ALL PRIVILEGES ON wordpress-db.* TO <username>@localhost IDENTIFIED BY '<contraseña>';
Ejecutamos el siguiente comando para que MySQL acepte los cambios, y salimos de MySQL

    FLUSH PRIVILEGES;
    exit
### Instalación de WordPress
Instalamos wget para descargar los archivos de WordPress y los descargamos

    $ yum install wget
    $ wget http://wordpress.org/latest.tar.gz
Como es un archivo comprimido, lo debemos extraer

    $ tar -xzvf latest.tar.gz
   Tenemos que mover los archivos extraidos a la carpeta /var/www/html para que el sitio muestre contenido. Además queremos conservar los permisos de los archivos, por lo que usamos el siguiente comando
   

    $ sudo rsync -avP ~/wordpress/ /var/www/html/
Para que se puedan subir archivos al sitio, se debe crear un directorio que los almacene

    $ mkdir /var/www/html/wp-content/uploads

Actualizamos los permisos de los nuevos archivos

    $ sudo chown -R apache:apache /var/www/html/*
### Configuración de WordPress
Debemos configurar los archivos de WordPress para que se conecte correctamente con la base de datos.
Estos archivos se encuentran en el siguiente directorio

    $ cd /var/www/html
Creamos una copia del archivo de configuración predeterminado para modificarlo

    $ cp wp-config-sample.php wp-config.php

Se debe editar la información que contiene el archivo

    $ nano wp-config.php
   
En los campos DB_NAME, DB_USER y DB_PASSWORD reemplazamos la información predeterminada con los datos que configuramos previamente

    // ** MySQL settings - You can get this info from your web host ** //
    /** The name of the database for WordPress */
	define( 'DB_NAME', '<nombreDB>' );

	/** MySQL database username */
	define( 'DB_USER', '<usuario>' );

	/** MySQL database password */
	define( 'DB_PASSWORD', '<contraseña>' );
Guardamos los cambios y así quedaría configurado nuestro WordPress.

Para verificar que esté funcionando correctamente, accedemos al sitio usando la IP del servidor o el dominio obtenido.
http://ip_o_dominio/wp-admin

Si nos encontramos con la pantalla de configuración inicial, la instalación fue un éxito.



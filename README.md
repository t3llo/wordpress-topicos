


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
### Instalación de WordPress

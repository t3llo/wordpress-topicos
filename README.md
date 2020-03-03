


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

`$ systemctl start httpd`  
`$ systemctl enable httpd`

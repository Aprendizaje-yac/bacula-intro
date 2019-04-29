.. Bacula documentation master file, created by
   sphinx-quickstart on Wed Apr 24 11:45:26 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Instalando Bacula en la PSI
==================================

Configuración inicial de Bacula
------
En un docker con las librerías basadas en Debian, se realizaron los siguientes pasos:

* Instalación de Apache, MySQL y PHP. Es necesario el servidor de aplicaciones web para correr Webmin en el servidor y así tener una herramienta gráfica muy potente para administrar no solo Bacula sino también todo el sistema y servicios que tengamos en el servidor, además necesitaremos el SGBD MySQL para que Bacula lo use en sus bases de datos.
* Instalación de los paquetes necesarios de cliente y servidor Bacula.
    *	bacula bacula-client bacula-common-mysql
    *	bacula-director-mysql bacula-sd-mysql bacula-server bacula-traymonitor
* Configuración de la base de datos que usará bacula-director
* Instalación de Webmin
Para instalar Webmin tenemos que descargarlo desde su web oficial http://www.webmin.com/download.html y descargar la versión para Debian. Una vez finalizada la descarga entramos al directorio donde se ha descargado e instalamos el paquete: 
dpkg -i nombre_paquete.deb. 
Es posible que aparezcan errores por la falta de dependencias, si este es el caso se deben instalar los paquetes faltantes. 
Una vez finalizada la instalación ya se tiene acceso desde la dirección https://localhost:10000 con el usuario root y la contraseña (pass) que tenga este en el sistema.



Como inicializar el servidor
------

Para poder iniciar el stack del servidor es necesario ubicarse dentro de la carpeta donde se encuentre la definición del servicio.

  .. code-block:: yaml
    :linenos:

      version: '2'                                                                                                                                                                                    
      services:                                                                                                                                                                                       
        bacula-db-test:                                                                                                                                                                               
          image: hub.psi.unc.edu.ar/dev/bacula-db:latest                                                                                                                                              
          environment:                                                                                                                                                                                
            MYSQL_ROOT_PASSWORD: root                                                                                                                                                                 
            MYSQL_DATABASE: bacula                                                                                                                                                                    
            MYSQL_USER: bacula                                                                                                                                                                        
            MYSQL_PASSWORD: bacula                                                                                                                                                                    
          stdin_open: true                                                                                                                                                                            
          tty: true                                                                                                                                                                                   
          ports:                                                                                                                                                                                      
          - 3306:3306                                                                                                                                                                                 
          volumes:                                                                                                                                                                                    
          - /srv/docker-persist/mysql:/var/lib/mysql
          links:
          - bacula-test:director
          labels:
            io.rancher.container.pull_image: always
        bacula-test:
          image: hub.psi.unc.edu.ar/dev/bacula-director:0.0.4
          stdin_open: true
          tty: true
          links:
          - bacula-db-test:database
          ports:
          - 10002:10000/tcp
          - 9101:9101
          - 9102:9102
          - 9103:9103
          volumes:
          - /srv/docker-persist/config:/etc/webmin/bacula-backup/config
          - /srv/docker-persist/director/etc-bacula:/etc/bacula
          - /backup-1:/mnt/backup-1
          - /srv/docker-persist/director/var-bacula/lib:/var/lib/bacula
          - /srv/docker-persist/director/var-bacula/run:/var/run/bacula
          - /srv/docker-persist/director/run-bacula:/run/bacula
          - /etc/hosts:/etc/hosts
      #   - /etc/resolv.conf:/etc/resolv.conf
      #    command: /bin/bash up-services.sh
          labels:
            io.rancher.container.pull_image: always

    



.. Bacula documentation master file, created by
   sphinx-quickstart on Wed Apr 24 11:45:26 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Agregar backup de base de datos a Bacula
====================================================

Intrucción
-----------

Para poder realizar backups de la base de datos, se decidió llevarlo a cabo de la siguiente manera:
En tyrion, se despliegan dockers (con el nombre de la base de datos a backupear), y este docker hara el dump remoto a la base) 

Pasos para agregar una base de datos
-----------------------------------------

1. Obtenemos la ip del servidor donde se encuentra la base de datos

::

  ssh databases-mariadb-dev.psi ipadd

2. En tyrion accedemos al /etc/hosts y agregamos el servidor nuevo:

.. code-block:: txt
  :linenos:
   
    172.19.27.112 g3-nodo-6.backup.unc.edu.ar
    172.19.27.113 g3-nodo-7.backup.unc.edu.ar
    172.19.27.114 g3-nodo-8.backup.unc.edu.ar
    172.19.27.115 g3-nodo-9.backup.unc.edu.ar
    172.23.16.66 huemul-1.backup.unc.edu.ar
    172.23.48.29 moodle-dev-memcached.backup.unc.edu.ar
    172.23.48.27 moodle-dev-nodo1.backup.unc.edu.ar
    172.23.48.28 moodle-dev-nodo2.backup.unc.edu.ar
    172.23.16.68 noc-jenkins-1.backup.unc.edu.ar
    172.23.16.80 nuxeo-dev-2.backup.unc.edu.ar
    172.23.16.79 openvas-1.backup.unc.edu.ar
    172.23.16.78 phpmyadmin.backup.unc.edu.ar
    172.23.16.77 phppgadmin.backup.unc.edu.ar
    172.23.16.83 spgi-mapuche-api-5.backup.unc.edu.ar
    172.23.16.75 spgi-mapuche-consulta.backup.unc.edu.ar

*Aclaración: por ahora se lo agrega en los falta agregar ya que se le realizará backup por medio de la red de administración hasta que tenga interfaz por la red de backup.*
   

2. Comprobar la conexión por medio haciendo: 

::

  ping databases-mariadb-dev.backup.unc.edu.ar


3. Acceder a **/root/tools/configuration/data/dbarmy/databases.yml** y agregar el nuevo servidor. Ejemplo:

.. code-block:: txt
  :linenos:

    - service_name: mariadb-slave
      database_srv: databases-mariadb-slave.backup.unc.edu.ar
      port: 9052
      type: mysql
      ipv4_address:  192.168.2.2
      ipv4_army: 172.23.15.150
    [...]
    - service_name: databases-2
      database_srv: databases-2.backup.unc.edu.ar
      type: mysql
      port: 9059
      ipv4_address:  192.168.2.3
      ipv4_army: 172.23.15.150
    - service_name: databases-mariadb-dev
      database_srv: databases-mariadb-dev.backup.unc.edu.ar
      type: mysql
      port: 9060
      ipv4_address:  192.168.2.3
      ipv4_army: 172.23.15.150

*Se completa el service_name con el nombre del servidor y database_srv con la dirección completa del servidor. A port el incrementamos uno del anterior (databases-mariadb-3 es 9057 entonce este es 9058), En type colocamos el tipo, y ipv4_address lo dejamos como esta (ya no se usa) y ipv4_army contiene la ip del servidor tyrion.*

4. Luego se ejecuta: */root/tools/configuration/actualizar_stack_bd.sh* 

Dicho script toma del stack de rancher “databases” aquellos servicios que están inactivos y los guarda en /tmp/inactivos.tmp
Luego se ejecuta el script de python que genera los clients, jobs y filesets correspondientes. Finalmente actualiza la carpeta donde bacula lee los clients, jobs y filesets y verifica que las configuraciones sean las correctas antes de reiniciar el director. 
Luego se accede a la carpeta /root/tools/stacks/bd-army/ donde se encuentra el rancher-compose.yml y allí se levanta el stack databases
Finalmente se leen cada una de las entradas de los servicios inactivos guardados en /tmp/inactivos.tmp
Por último se recarga el servicio bacula-director en el director

**Es necesario, que el servidor de la base de datos tenga el puerto habilitado, y creado los permisos necesarios para el usuario de bacula**
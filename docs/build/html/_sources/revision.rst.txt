.. Bacula documentation master file, created by
   sphinx-quickstart on Wed Apr 24 11:45:26 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Saber a que archivos se le están haciendo backups
====================================================

Es de suma importancia que verifiquen que los servidores y los archivos listados, para backup, sean los requeridos para que se pueda restaurar el sistema en caso de un desastre. Además, puede revisar si el backup configurado se realizó correctamente.

En caso de que desee agregar nuevos archivos o modificar los existentes, se solicita que se envíe un ticket especificando:

* Servidor
* Directorios con los archivos a incluir/excluir

En base a dicha solicitud se realizarán los cambios apropiados y se dispondrá de los backups a partir de la fecha que haya ingresado al sistema de backup.

Sistemas de backups
--------------------------------

Los backups se realizan a sistemas de archivos (DLE, conjunto de archivos a los que se hace backup) y base de datos.

Los sistemas de archivos están detallados en http://backups.psi.unc.edu.ar:/dle/

Procedimiento de Visualizacion de Backups
---------------------

1. Ingresar al link http://backups.psi.unc.edu.ar

.. image:: ./_images/revision1.png

2. Acceder a la sección **Reportes**: Reportes Diarios (Allí se muestran reportes diarios de los backups que se hacen) 

.. image:: ./_images/revision2.png


3. Al ingresar a alguno de ellos se mostrarán los backups realizados en esa fecha por servidor bajo el nombre de Job, cuyo formato es *<servidor>-<dle>*.

.. image:: ./_images/revision3.png

4. Usar el filtro para buscar el servidor a verificar, y se podrán visualizar los archivos que incluye el dle accediendo a http://backups.psi.unc.edu.ar:9000/dle/ y seleccionando la el dle correspondiente.

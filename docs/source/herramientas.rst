.. Bacula documentation master file, created by
   sphinx-quickstart on Wed Apr 24 11:45:26 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Herramientas Elegidas
==================================

------------

Docker
"""""""

DOCKER es una herramienta desarrollada por la empresa Docker Inc. Esta herramienta permite la creacion y el uso de contenedores de Linux.
Es decir, Docker usa el kernel de Linux y funciones de este, para segregar los procesos, de modo que puedan ejecutarse de manera independiente.

Una de las principales virtudes del uso de contenedores es la independencia, la capacidad de ejecutar varios procesos y aplicaciones por separado para hacer un mejor uso de la infraestructura, y al mismo tiempo, conservar la seguridad.

Esta herramienta ofrece un modelo de implementación basado en imagenes, donde, a través de un archivo de texto plano **Dockerfile**, se le especifican las instrucciones necesarias para automatizar la creaciòn de la imagen que será utilizada posteriormente para la ejecuciñón de las instancias especificas ( contenedores ).


En este proyecto se optó por realizar distintas imágenes de acuerdo a las funcionalidades.

*Bacula-director
*Bacula-mysql
*Bacula-restore
*Bacula-apache

Bacula-director
***************
Va a ser el contenedor encargado de realizar los backups.
En dicho docker se instalara La herramienta Bacula



Bacula-mysql
************
Tendra toda la base de datos tipo __mysql__, donde quedara registrado todo el árbol de directorios, para que Bacula-director pueda manipular los archivos y 


Modelo Teórico
===============

Planificación
-------------
(hacer diagrama de gantt)

Detalle de los objetivos y resultados esperados en las diferentes etapas.

Etapa de requerimientos
"""""""""""""""""""""""
Objetivo: Estudiar el dominio identificando los requerimientos y el alcance

Resultado: Historias de usuarios y diagramas.

Investigación
"""""""""""""""
Objetivo: Identificar y analizar las distintas tecnologías disponibles

Resultado: Selección de tecnologías.

Capacitación en las tecnologías 
""""""""""""""""""""""""""""""""
Objetivo: Aprendizaje de las herramientas y técnicas a usar.

Resultado: Equipo capacitado.


Análisis y diseño
"""""""""""""""""""
Objetivo: 

Resultado:


Implementación
"""""""""""""""
Objetivo: Implementación del sistema de backup con una cantidad limitada de servidores.

Resultado: Sistema de backup funcional.

Testing
""""""""
Objetivo:

Resultado:


Requerimientos
===============

Usuarios del sistema 
----------------------

Usuario administrador de backup
""""""""""""""""""""""""""""""""
Es aquel encargado de configurar, monitorear y administrar el sistema de backups. Puede crear y configurar los distintos tipos de backups y equipos de los que se tendrá respaldos. 

Usuario propietario de la información
""""""""""""""""""""""""""""""""""""""
Es aquel que es dueño de la información y del sistema en sí. Conoce plenamente su funcionamiento y tiene la capacidad de tomar decisiones sobre sus datos y equipos. Es el encargado de determinar a quiénes serán los administradores de la aplicación y en caso de cambios de personal, reportarlo. Además, es el encargado de determinar a qué se le realizará respaldo y la frecuencia necesaria.

Usuario administrador del servidor
"""""""""""""""""""""""""""""""""""
Es el responsable de garantizar el correcto funcionamiento de toda la plataforma tecnológica e informática a fin de mantener la operatividad del negocio. Hace uso de los backups en caso de producirse un error o para generar pruebas específicas. 

Usuario de administración de la aplicación
""""""""""""""""""""""""""""""""""""""""""""
Es el responsable de realizar las configuraciones deseadas para que el sistema funcione correctamente.  Además, corroborará que los respaldos sean los correctos. Hace uso de los backups en caso de producirse un error o para generar pruebas específicas. 

Equipos a realizar backup
--------------------------

Los recursos informáticos que se disponen en la PSI son:

* Servidor de archivos (NFS): almacenan distintos tipos de archivos y los distribuye en los diversos clientes en la red.
* Servidor de aplicaciones: aloja las aplicaciones de la UNC y de las dependencias correspondientes. 
* Servidor de correo electrónico: almacena, envía, recibe, enruta y realiza operaciones relacionales con email para los clientes. 
* Servidor de base de datos: provee servicios de base de datos a otros programas. 
* Servidor DNS:
* Servidor web:
* Switches y Routers:

Infraestructura existente 
--------------------------




Historias de usuarios
----------------------

A continuación se listan las historias de usuarios identificadas tras varias reuniones con las diferentes personas interesadas en el proyecto. Se han agrupado por temática.

Sobre respaldo a equipos
"""""""""""""""""""""""""

Como propietario de la información
''''''''''''''''''''''''''''''''''''
Como usuario quiero poder definir a qué se le debe hacer respaldo.

Como usuario quiero poder definir la frecuencia de los respaldos.

Como usuario quiero poder definir quién tendrá acceso a los respaldos. 

Como administrador de la aplicación 
'''''''''''''''''''''''''''''''''''''
Como usuario quiero poder acceder a ciertos backups realizados.

Como usuario quiero poder ver el estado de los respaldos de mis sistemas. 

Como administrador del servicio 
'''''''''''''''''''''''''''''''''''''
Como usuario quiero poder acceder a los backups en caso de ser necesario. 

Como administrador de backups
'''''''''''''''''''''''''''''''''''''
Como usuario quiero poder crear y administrar filesets (conjunto de directorios o archivos).

Como usuario quiero poder recibir un reporte de los estados de los backups.

Como usuario quiero conocer los tiempos en que tardan los backups en ejecutarse.


Investigación
==============

Las características deseables de un sistema de respaldo de información:

* Copia y recuperación consistente. 
* Automatización de tareas. 
* Seguridad y fiabilidad. 
* Simplicidad de uso (curva de aprendizaje). 
* Almacenamiento en diversos medios.
* Generación de informes. 


Herramientas open source usadas para sistemas de backups

Bacula 
------
Es un conjunto de programas open source que permiten administrar copias de seguridad, recuperar y la verificar los datos de la computadora en una red de computadoras de diferentes tipos. Bacula también se puede ejecutar completamente en una sola computadora y puede realizar copias de seguridad en varios tipos de medios, incluidas cintas y discos.

Su infraestructura está basada en cliente / servidor de red. Bacula es relativamente fácil de usar y eficiente, al tiempo que ofrece muchas funciones avanzadas de gestión de almacenamiento que facilitan la búsqueda y recuperación de archivos perdidos o dañados. Debido a su diseño modular, Bacula es escalable desde pequeños sistemas informáticos a sistemas que consisten en cientos de computadoras ubicadas en una gran red. [#BaculaQuees]_

Componentes de Bacula
"""""""""""""""""""""""""""
Director (DIR, bacula-director) es el programa servidor que supervisa todas las funciones necesarias para las operaciones de copia de seguridad y restauración. Es el eje central de Bacula y en él se declaran todos los parámetros necesarios. Se ejecuta como un “demonio” en el servidor.

Storage (SD, bacula-sd) es el programa que gestiona las unidades de almacenamiento donde se almacenarán los datos. Es el responsable de escribir y leer en los medios que utilizaremos para nuestras copias de seguridad. Se ejecuta como un “demonio” en la máquina propietaria de los medios utilizados. En muchos casos será en el propio servidor, pero también puede ser otro equipo independiente.

Catalog es la base de datos (MySQL en nuestro caso) que almacena la información necesaria para localizar donde se encuentran los datos salvaguardados de cada archivo, de cada cliente, etc. En muchos casos será en el propio servidor, pero también puede ser otro equipo independiente.

Console (bconsole) es el programa que permite la interacción con el “Director” para todas las funciones del servidor. La versión original es una aplicación en modo texto (bconsole). Existen igualmente aplicaciones GUI para Windows y Linux (Webmin, Bacula Admin Tool, Bacuview, Webacula, Reportula, Bacula-Web, etc).

File (FD) Este servicio, conocido como “cliente” o servidor de ficheros está instalado en cada máquina a salvaguardar y es específico al sistema operativo donde se ejecuta. Responsable para enviar al “Director” los datos cuando este lo requiera. [#BaculaComponentes]_

Características
"""""""""""""""""

* Tiene garantía de copia y recuperación consistente.
* Tiene garantía de seguridad y fiabilidad de información porque es capaz de usar algoritmos de cifrados. 
* Tiene garantía de almacenamiento en diversos medios.
* No presenta simplicidad de uso. 
* No presenta una forma automática generación de informes.
* Tiene automatización de tareas.
* Presenta solución de catálogo. 

.. [#BaculaQuees] ¿Qué es Bacula? https://www.bacula.org/9.4.x-manuals/en/main/What_is_Bacula.html
.. [#BaculaComponentes] Componentes o servicios de Bacula https://www.bacula.org/9.4.x-manuals/en/main/What_is_Bacula.html


BackupPC
----------
Sistema de alto rendimiento y nivel empresarial para realizar copias de seguridad de computadoras, computadoras de escritorio y portátiles Unix, Linux, WinXX y MacOSX en el disco de un servidor. BackupPC es altamente configurable y fácil de instalar y mantener.
BackupPC presenta herramientas que hacen qe minimice el almacenamiento en disco y la E/S de disco. Esto es así porque los archivos idénticos de diferentes copias de seguridad se almacenan sólo una vez (usando enlaces). No es necesario ningún cliente, ya que el propio servidor es un cliente para varios protocolos que son manejados por otros servicios nativos del sistema operativo cliente. 


Caracteristicas
"""""""""""""""""

* Tiene garantía de copia y recuperación consistente.
* Tiene garantía de seguridad y fiabilidad de información porque es capaz de usar algoritmos de cifrados. 
* Tiene garantía de almacenamiento en al menos un tipo de medio.
* Presenta simplicidad de uso. Dispone de una interfaz gráfica. 
* Presenta una forma automática generación de informes.
* Tiene automatización de tareas.


Amanda 
-------
AMANDA, el Advanced Maryland Automatic Network Disk Archiver, es una solución de respaldo que le permite al administrador de TI configurar un único servidor de respaldo maestro para hacer una copia de seguridad de múltiples hosts a través de la red en unidades de cinta / cambiadores o discos o medios ópticos. Amanda usa utilidades y formatos nativos (por ejemplo, volcado y / o tar de GNU) y puede hacer una copia de seguridad de una gran cantidad de servidores y estaciones de trabajo que ejecutan varias versiones de Linux o Unix. 
Amanda presenta una arquitectura cliente/ servidor. La mayor ventaja de Amanda sobre cualquier otro software de respaldo es que Amanda no utiliza ningún formato de datos de propiedad exclusiva. Amanda usa utilidades estándar de sistemas operativos como dump y tar , o utilidades de código abierto disponibles en muchos sistemas operativos como GNUtar , smbtar y Schily tar, y utiliza el mismo formato de archivo en el medio. 


Características
"""""""""""""""""

* Tiene garantía de copia y recuperación consistente.
* Tiene garantía de seguridad y fiabilidad de información porque es capaz de usar algoritmos de cifrados. 
* Tiene garantía de almacenamiento en al menos un tipo de medio.
* No presenta simplicidad de uso. Tampoco ofrece una interfaz gráfica intuitiva y ágil.
* No presenta una forma automática generación de informes.
* Tiene automatización de tareas.
* Realiza backups con utilidades estándar de sistemas operativos. 

Rsync
-------






Drive 
-------

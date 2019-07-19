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
* Servidor de correo electrónico: almacena, envía, recibe, enruta y realiza operaciones relacionalas con email para los clientes. 
* Servidor de base de datos: provee servicios de base de datos a otros programas. 
* Servidor DNS:
* Servidor web:
* Switches y Routers:


Historias de usuarios
----------------------

A continuación se listan las historias de usuarios identificadas tras varias reuniones con las diferentes personas interesadas en el proyecto. Se han agrupado por temática.

Sobre respaldo a equipos
"""""""""""""""""""""""""

Como usuario propietario de la información
'''''''''''''''''''''''''''''''''''''''''''
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






.. Bacula documentation master file, created by
   sphinx-quickstart on Wed Apr 24 11:45:26 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Introducción
=============

Antecedentes
-------------
¿Qué es una organización sin sus datos? ¿Cómo debe actuar frente a una pérdida de datos? 
Los recursos informáticos se han convertido en una herramienta imprescindible en todos los ámbitos empresariales, organizaciones, autónomos.  
Todos estos disponen de un equipo informáticos que ayuda en el proceso productivo y que contiene una gran cantidad de datos. 
Cualquier fallo del mismo puede suponer una gran pérdida.

Además, a fin de proporcionar o automatizar más procesos de negocio, a medida que las organizaciones crecen, van adquiriendo más recursos informáticos,
implicando, muchas veces, que algun recurso requiera la disponibilidad de otros para funcionar correctamente, produciendo que una falla o pérdida en 
alguno de estos genere grandes repercusiones.

Cada día aparecen distintos tipos de amenazas que comprometen los datos y archivos que una organización genera para el desarrollo de su negocio. Estos
tipos de amenazas varían desde errores humanos como, por ejemplo, el borrado accidental de ficheros claves como /etc en Linux, un malware que que 
restringe el acceso a determinadas partes o archivos del sistema operativo infectado, y pide un rescate a cambio de quitar esta restricción.

Son estas situaciones donde las copias de seguridad toman un caracter relevante a fin de que la organizacion pueda garantizar la continuidad de su 
negocio en caso de catástrofe. 

Se define como un recurso informático a todos aquellos componentes tanto de hardware como de software que son necesarios para el buen funcionamiento y la 
optimización del trabajo de computadoras y periféricos, tanto a nivel individual como organizativo.

Objetivo y problemática de estudio 
-----------------------------------
Una buena infraestructura de backup responderá a garantías de integridad y consistencia del dato, continuidad de servicio y negocio. 

El siguiente proyecto pretende desarrollar una propuesta para realizar backups en los diversos equipos
de la Prosecretaría Informática de la Universidad Nacional de Córdoba (PSI). 

La PSI [#PSI]_ tiene como misión contribuir con las funciones de docencia, investigación y extensión de la Universidad Nacional de Córdoba, 
coordinando el uso de los recursos relacionados con la informática. 

Para ello sus acciones se encuadran en tres ejes principales:

* Formulación y ejecución de políticas relacionadas con las Tecnologías de Información y Comunicaciones (TICs).
* Adquisición y administración de sistemas de información.
* Planificación y administración de la infraestructura de red y servicios informáticos.

A fin de lograr con esto, la PSI cuenta con una infraestructura de gran envergadura de la cual es importante resguardar los datos. 
Actualmente la UNC, cuenta con un procedimiento para realizar copias de seguridad de la información, pero dado el crecimiento tanto 
en la cantidad de información como en las tecnologías utilizadas, se planteo la necesidad de buscar una nueva solución 
a este nuevo escenario.

El backup, o copia de resguardo, se refiere al proceso de copiar y almacenar datos de diversos equipos (computadoras, switchs, etc)
en otro/otros dispositivos con el objeto de poder restaurar los datos en caso de que la información original se haya eliminado o
sus datos se encuentren corruptos. También brinda la posibilidad de acceder a datos anteriores para buscar archivos específicos de acuerdo
con las políticas de retención de datos definidas.

La forma en que se lleva a cabo el proceso de backup se consolida mediante politicas de backup precisas, claras y adaptadas a la PSI. Estas políticas
determinan:

* cómo se lleva a cabo el proceso
* qué hardware, software y factor humano intervendrá
* dónde se almacenarán los datos
* cuánto tiempo se mantendrán los datos
* la frecuencia de las copias de resguardo
* los tipos de backups que se llevarán a cabo 
* el plan de recuperación de datos

.. [#PSI] PSI http://www.psi.unc.edu.ar/

Campo de acción
----------------
La misión de este proyecto consiste en implementar una solución de backup que abarque todos los diversos recursos informáticos de la PSI.


Objetivos
----------
Objetivo general
"""""""""""""""""
Implementar una solución de backups para la Prosecretaría Informática de la Universidad Nacional de Córdoba, para asegurar la persistencia de los datos de los múltiples sistemas basado en las buenas prácticas.



Objetivos específicos
""""""""""""""""""""""
* Identificar los sistemas y equipos que necesitan backups de la PSI
* Relevar las políticas de backups definidas
* Proponer un sistema de backup que cumpla con las politicas relevadas 
* Elegir herramientas opensource que tengan las funicionalidad necesarias para poder realizar los backup con los criterios necesarios.
* Relevar los usuarios del sistema y sus necesidades de acceso a los backups.
* Desarrollar una solución para brindarle los backups en tiempo y forma a los usuarios autorizados.
* Realizar un sistema de monitoreo para ver el estado de los backups.
* Desarrollar procedimientos precisos para automatizar los despliegues de los clientes de backups.
* Documentar el proceso de respaldo de información, para evidenciar de forma clara y concreta las herramientas y procedimientos que se deben tener en cuenta para dicho proceso.

Aporte
-------
La implementación de un sistema de backups ofrece diversos beneficios tanto para la PSI como para los usuarios de los diversos sistemas, tales como:

* Capacidad de respuesta: gracias a las copias guardadas, se podrá volver a tener todos los sistemas plenamente operativos en un breve lapso de tiempo. Esto se traduce en un nivel superior de eficiencia, permitiendo restaurar la información y servicios de manera eficaz.
* Incremento de la confianza en los usuarios: al disponer de redundancia de información


Enfoque metodológico
---------------------

Proceso
""""""""
La secuencia de pasos para llevar a cabo el presente proyecto son:

#. Identificación y análisis de requerimientos.
#. Análisis y evaluación de la arquitectura a implementar en el proyecto.
#. Investigación y capacitación en las herramientas necesarias para llevar a cabo el proyecto.
#. Definición de los casos de prueba.
#. Desarrollo e implementación de pruebas iniciales con cada tipo. 
#. Verificación y Validación de los casos empleados. 
#. Generación de scripts automáticos de despliegue. 
#. Puesta en producción



Técnicas
"""""""""




Herramientas
"""""""""""""

Marco Teórico
=====================

La ISO/IEC 27002 define a la información como un recurso que tiene valor para una organización y por consiguiente debe ser debidamente protegida y a la seguridad de la información como la protección de la información de una amplia variedad de amenazas, con el objeto de asegurar la continuidad del negocio, minimizar los riesgos y maximizar el retorno de la inversión y las oportunidades de negocio. [#ISOIEC27002DEFINFO]_

Backup, copia de seguridad o copia de respaldo
-----------------------------------------------

El backup, copia de resguardo, copia de reserva, es una copia exacta de los datos originales de un sistema de información o de un conjunto de software (archivos, documentos, etc) que se  almacena en un lugar seguro, con el fin de poder volver a disponer de su información en caso de que alguna eventualidad, accidente o desastre ocurra y ocasione su pérdida del sistema. [#BCKDEF]_

Lineamientos de implementación [#ISOIEC27002LIN]_
"""""""""""""""""""""""""""""""
Los lineamiento de implementación de backup que propone son los siguientes:

Se debiera proporcionar medios de respaldo adecuados para asegurar que toda la información esencial y software se pueda recuperar después de un desastre o falla de medios:
Se debieran considerar los siguientes ítems para el respaldo de la información:

a. se debiera definir el nivel necesario de respaldo de la información;
b. se debieran producir registros exactos y completos de las copias de respaldo y procedimientos documentados de la restauración;
c. la extensión (por ejemplo, respaldo completo o diferencial) y la frecuencia de los respaldos debiera reflejar los requerimientos comerciales de la organización, los requerimientos de seguridad de la información involucrada, y el grado crítico de la información para la operación continua de la organización;
d. las copias de respaldo se debieran almacenar en un lugar apartado, a la distancia suficiente como para escapar de cualquier daño por un desastre en el local principal;
e. a la información de respaldo se le debiera dar el nivel de protección física y ambiental apropiado consistente con los estándares aplicados en el local principal; los controles aplicados a los medios en el local principal se debiera extender para cubrir la ubicación de la copia de respaldo;
f. los medios de respaldo se debieran probar regularmente para asegurar que se puedan confiar en ellos para usarlos cuando sea necesaria en caso de emergencia;
g. los procedimientos de restauración se debieran chequear y probar regularmente para asegurar que sean efectivos y que pueden ser completados dentro del tiempo asignado en los procedimientos operacionales para la recuperación;
h. en situaciones cuando la confidencialidad es de importancia, las copias de respaldo debieran ser protegidas por medios de una codificación.

Los procedimientos de respaldo para los sistemas individuales debieran ser probados regularmente para asegurar que cumplan con los requerimientos de los planes de continuidad del negocio. 

Para sistemas críticos, los procedimientos de respaldo debieran abarcar toda la información, aplicaciones y data de todos los sistemas, necesarios para
recuperar el sistema completo en caso de un desastre.

Se debiera determinar el período de retención para la información comercial esencial, y también cualquier requerimiento para que las copias de archivo se mantengan permanentemente.


Importancia de los backups
--------------------------
La ISO define que el objetivo de un backup o respaldo es mantener la integridad y disponibilidad de la información y de las instalaciones del procesamiento de la información. Además, afirma que se debieran establecer los procedimientos de rutina para implementar la política de respaldo acordada y la estrategia para tomar copias de respaldo de la data y practicar su restauración oportuna. [#ISOIEC27002OBJBACK]_

Dentro de los beneficios que encontramos en el tener un sistema de backup se encuentran:

* Capacidad de respuesta: debido a las copias de respaldo se puede volver a tener un sistema totalmente operativo en un breve lapso de tiempo. 
* Incremento de la confianza del cliente


Tipos de backups
-----------------
Full
"""""
Consiste en hacer una copia de todos los datos del sistema en otro soporte. 
La ventaja principal de este tipo de copia es que proporciona una fácil restauración de los datos. 
Desventajas:

* Mayor necesidad de espacio de almacenamiento. Puesto que se realiza la copia total del sistema cada vez que se ejecuta el respaldo, provocando que se almacene múltiples veces todos los archivos, incluso aquellos 

Diferencial
""""""""""""


Incremental
""""""""""""


Responsables de backups
------------------------


.. [#ISOIEC27002DEFINFO] ISO/IEC 27002 página 9
.. [#BCKDEF] https://concepto.de/backup/
.. [#ISOIEC27002LIN] ISO/IEC 27002 página 60
.. [#ISOIEC27002OBJBACK] ISO/IEC 27002 página 60


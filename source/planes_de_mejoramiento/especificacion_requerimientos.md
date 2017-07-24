[[
title: Documento de Especificación de requerimientos del Proceso Registro y Seguimiento Planes de Mejoramiento
author: José Javier Vargas Serrato
]]
Especificación de Requerimientos de Aplicaciones
=================================================

Registro y seguimiento Planes de Mejoramiento
=============================================

Casos de Uso
------------

A continuación se listan y describen los actores principales del sistema, estos son candidatos para ser implementados como grupos de seguridad en el aplicativo:

### Actores
* **Responsable**:  Tienen a cargo el registro de avances y observaciones de las acciones que coincidan con su dependencia.

* **Auditor**: Tiene como responsabilidad registrar los hallazgos, las acciones, la información básica del plan, la calificación de los avances mensuales y las observaciones.

* **Administrador Módulo**: Tiene como responsabilidad parametrizar el sistema.

* **Analista**: Tiene como responsabilidad:

    - Consultar los registros de avances de los usuarios responsables y auditores.
    - Registro de observaciones para solicitar a los responsables la ampliación de la información.
    - Generar reportes.

### Diagrama de casos de uso

{uml}

    left to right direction
    skinparam packageStyle rect
    actor usuario
    actor responsable
    actor jefe_dependencia
    actor ejecutor
    actor auditor
    actor analista

    usuario <|-- responsable
    responsable <|-- jefe_dependencia
    responsable <|-- ejecutor
    usuario <|-- auditor
    auditor <|-- oci
    usuario <|-- analista
    analista <|-- OAP
    analista <|-- CCCI
    analista <|-- DG

{enduml}


{uml}

    left to right direction
    skinparam packageStyle rect
    actor auditor


    rectangle "Módulo Registro de Plan de Mejoramiento" {
      auditor -- (Registrar Plan de mejoramiento)
      (Registrar hallazgo) .> (Registrar Plan de mejoramiento)  : extends

      (Registrar actividades)
      (Registrar actividades) .> (Registrar hallazgo) : extends
      (Registrar tareas)
      (Registrar tareas) .> (Registrar actividades) : extends
    }

{enduml}

{uml}

    left to right direction
    skinparam packageStyle rect
    actor jefe_dependencia

    rectangle "Módulo Aceptar Asignación" {
      jefe_dependencia -- (Aceptar/Rechaza asignación plan de mejoramiento)
      jefe_dependencia -- (Acepta/Rechaza Avance mensual)
    }

{enduml}


{uml}

    left to right direction
    skinparam packageStyle rect
    actor ejecutor

    rectangle "Módulo Registro Avances por Acciones" {
      ejecutor -- (Registrar avances Mensual por Accion)
    }

{enduml}

{uml}

    left to right direction
    skinparam packageStyle rect
    actor ejecutor

    rectangle "Módulo Asignación de Tareas por Acciones" {
      ejecutor -- (Registrar tareas por Accion)
    }

{enduml}

{uml}

    left to right direction
    skinparam packageStyle rect
    actor auditor
    actor analista

    rectangle "Módulo Registro de Observaciones" {
      auditor -- (Registrar observaciones para Acción)
      auditor -- (Registrar observaciones para tareas)
      analista -- (Registrar observaciones para Acción)
      analista -- (Registrar observaciones para tareas)
    }
{enduml}

{uml}

    left to right direction
    skinparam packageStyle rect
    actor oci

    rectangle "Módulo Cuantificación/Cualificación Mensual" {
      oci -- (Cuantificar/Cualificar avances del plan de mejoramiento)
    }

{enduml}

{uml}

    left to right direction
    skinparam packageStyle rect
    actor analista
    actor auditor

    rectangle "Módulo de Reportes" {
     analista  -- (Reporte avances planes de mejoramiento)
     analista  -- (Generar informe planes de mejoramiento)

     (Reporte avances planes de mejoramiento) -- auditor
     (Generar informe planes de mejoramiento)-- auditor
    }

{enduml}

{uml}

left to right direction
    skinparam packageStyle rect
    actor responsable

    rectangle "Módulo de Notificaciones" {
     responsable -- (Notificar asignación de plan de mejoramiento)
     responsable  -- (Notificar calificación de avances del plan de mejoramiento)
     responsable --  (Notificar aceptación de plan de mejoramiento)
     responsable -- (Notificar cuantificación/cualificación de avances del plan de mejoramiento)
    }

{enduml}

Diagrama de Actividades del sistema
-----------------------------------

{uml}

    title Diagrama de Actividades \n
    |#AntiqueWhite|auditor|
    start

    :Registrar plan de mejoramiento para el hallazgo;
    :Registrar hallazgo;
    :Registrar Acciones;
    :Asignar plan de mejoramiento a responsable;
    |Jefe Dependencia|
    while (aceptar asignación) is (no)
      |auditor|
      :Actualizar plan de mejoramiento;
    endwhile (si)
    |Jefe Dependencia|
    :Registrar Tareas;
    |responsable|
    :Registrar avances mensuales de las Acciones;
    |Jefe Dependencia|
    :Aceptar/Rechazar avances mensuales de las Acciones;
    |usuario|
    :Registrar observaciones;
    |auditor|
    :Registrar cuantificación/cualificación de avances del plan de mejoramiento;
    |#AntiqueWhite|usuario|
    :Generar reportes;
    stop

{enduml}

### Registrar Plan de Mejoramiento
En esta actividad se realiza el ingreso de los datos básicos para consolidar el registro del objeto plan. El Plan de Mejoramiento está constituido por uno o varios objetos Hallazgos relacionados. Los registros de las Acción están asociados a los objetos Hallazgos; y las Acciones tendrán registro de tareas asociados.

#### Historias de Usuario

- SPM-01: Como auditor quiero registrar un plan de mejoramiento conforme a los hallazgos encontrados para darle debido tratamiento en forma de actividades y tareas en el sistema.

    - [1] **Registrar Plan de Mejoramiento Interno**: El sistema debe permitir diligenciar los campos: Nombre, Radicado Orfeo, Dependencia, Tipo, Origen plan de mejoramiento, proceso origen plan de mejoramiento, dependencia que formula el plan.
    - [2] **Registrar Plan de Mejoramiento Externo Contraloría General de la República (CGR)**: El sistema debe permitir diligenciar los campos: Nombre, Radicado Orfeo, Tipo, Dependencia.
    - [3] **Registrar Plan de Mejoramiento Externo Contraloría de Bogotá (CB)**: El sistema debe permitir diligenciar los campos: Nombre, Radicado Orfeo, Dependencia, Tipo.
    - [4] ** Filtros y Búsquedas de planes de mejoramiento**: El sistema debe proporcionar una vista que permite realizar filtros de búsqueda por; tipo de plan de mejoramiento, estados, áreas responsables. Esto para facilitar el seguimiento y control de los planes de mejoramiento.
    - [5] **Restricción de dominio del plan de mejoramiento**: El usuario auditor podrá leer todos los registros de planes para realizar control y auditoría de estos; pero solo podrá modificar los que esté en su dominio "registro creados por él".
El usuario jefe_dependencia tendrá acceso de lectura únicamente a los planes que estén asignados a su dependencia.
El usuario analistas podrá leer todos los planes.


### Registrar Hallazgo
Durante esta actividad se realiza el ingreso de datos para el registro de un Hallozgo asosiado a un plan de mejoramiento registrado.

#### Historias de Usuario

- SPM-02: Como auditor quiero registrar un hallazgo para darle el debido tratamiento en la organización a travéz de un plan de mejoramiento.

    - [1] **Registrar Hallazgo de Plan de Mejoramiento Interno**: El sistema debe permitir diligenciar los campos: Nombre de hallazgo, Descripción, Causa.
    - [2] **Registrar Hallazgo de Plan de Mejoramiento Externo Contraloría de Bogotá**: El sistema debe permitir diligenciar los campos: Nombre de hallazgo, Descripción.
    - [3] **Registrar Hallazgo de Plan de Mejoramiento Externo Contraloría General de la República (CGR)**: El sistema debe permitir diligenciar los campos: Nombre de hallazgo, Descripción hallazgo, Causa del hallazgo, Efecto del hallazgo.
    - [4] **Filtros y Búsquedas de Hallazgos**: El sistema debe permitir consultar los hallazgos y filtrarlos por Tipo (Interno, Contraloria Bogotá, Contraloría General), Plan de Mejoramiento.
    - [5] **Restricción de dominio de Hallazgos**: El usuario auditor podrá leer todos los registro de Hallazgo para realizar control y auditoria de estos; pero solo podrá modificar los registros que esté en su dominio "registro creados por él".
El usuario jefe_dependencia tendrá acceso de lectura únicamente a los hallazgos que estén asignados a su dependencia.
El usuario analistas podrá leer todos los Hallazgos.

### Registrar Acción
Durante esta actividad se realiza el ingreso de datos de los objetos Acciones que van a permitir dar solución un Hallazgo encontrado.

#### Historias de Usuario

- SPM-03: Como auditor quiero registrar actividades que definirán la solución a los hallazgos del plan de mejoramiento.

    - [1] **Registrar Acción de Plan de Mejoramiento Interno:**: El sistema debe permitir diligenciar los campos: Acción correctiva(texto), objetivo(texto), indicador(texto), unidad de medida(texto), meta(texto), área responsable(relacionado con el plan), recursos(texto), fecha inicial(date), fecha final(date), responsable seguimiento OCI(Relacionado con el auditor del hallazgo).
    - [2] **Registrar Acción Plan de Mejoramiento Externo PM Contraloría de Bogotá**: El sistema debe permitir diligenciar los campos: Acción Correctiva(texto), indicador(texto), meta(texto), área responsable (relacionado con el plan), recursos(Texto), fecha inicial, fecha final.
    - [3] **Registrar Acción de Plan de Mejoramiento Externo Contraloría General de la República (CGR)**: El sistema debe permitir diligenciar los campos: Accion de mejoramiento(texto), propósito de la acción de mejoramiento(texto), No. actividad(Generado por el sistema), Descripción de la actividad(texto), Denominación de la unidad de medida(texto), cantidad de medida de la actividad(integer), área responsable (Relacionado al plan), Fecha iniciación de la actividad(date), Fecha terminación de la actividad(date), Plazo en semana de la actividad(calculado a partir de las fechas).
    - [4] **Filtros y Búsquedas de Acción**: El sistema debe permitir consultar las Actividades y filtrarlas por Tipo(Interno, Contraloría Bogotá, Contraloría General), Plan de mejoramiento, Estado, Dependencia, Responsable. Adicionalmente se debe permitir agrupar las actividades por Fecha de inicio, Fecha de finalización, Dependencia, Responsable.
    - [5] **Restricción de dominio de Acción**: El usuario auditor podrá crear registros de Acciones, podrá leer todos los registro de Acciones para realizar control y auditoría de estos; pero solo podrá modificar los registros creados por él.
El usuario jefe_dependencia tendrá acceso de lectura únicamente a las Acciones que estén asignadas a su dependencia.
El usuario analistas podrá leer todos los Acciones.

### Aceptar Asignación
En esta actividad los usuarios jefe_dependencia aceptan o rechazan la asignación de una acción para su respectiva área.

#### Historias de Usuario
- SPM-12: Como auditor requiero asignar acciones del plan de mejoramiento a las dependencias responsables para su aceptación o rechazo.

    - [1] **Asignar Acciones a una dependencia**: El sistema debe permitir asignar acciones del plan de mejoramiento a una dependencia para que esta acepte o rechace la asignación.
    - [2] **Notificar asignación de una acción**: El sistema debe notificar al jefe de dependencia cuando se le asigne una tarea

- SPM-13: Como usuario jefe_dependencia quiero acceder a una vista que muestre las acciones asignadas a mi dependencia y poder aceptarlas o rechazarlas para realizarles el debido tratamiento.

    - [1] **Aceptar/Rechazar Asignación de las Acciones**: El sistema debe  permitir aceptar o rechazar la asignación de la acción.
    - [2] **Notificar Aceptación y Rechazo de Acciones**: El sistema debe notificar al auditor OCI cuando se realice la aceptación y rechazo de la asignación de una o varias acciones.


### Registrar Tareas

Durante esta actividad se realiza el registro de Tareas pertenecientes a una Acción del Plan de Mejoramiento. Estas tareas se asignan a otras áreas para su debido diligenciamiento.

#### Historias de Usuario

- SPM-04: Como jefe_dependencia quiero registrar tareas que abarquen la solución de la Acción definida dentro de un hallazgo.

    - [1] **Registrar Tarea de Actividad**: El sistema debe permitir diligenciar los campos: resumen de la tarea, asignado a(ejecutor), fecha de inicio, fecha de finalización, actividad del plan a la que pertenece.
    - [2] **Filtros y Búsquedas de Tareas**: El sistema debe permitir consultar las Tareas y filtrarlas por Responsable, Estado, Asignadas a mí, Plan de Mejoramiento(proyecto), Actividad. Adicionalmente se debe permitir agrupar las tareas por Fecha de inicio, Fecha de finalización, Responsable, Plan de mejoramiento, Actividad y Estado.
    - [3] **Restricción de dominio de Tareas**: El usuario auditor podrá leer  todos los registros de Tareas.
El jefe_dependencia puede crear y editar todas las tareas relacionadas a una acción que pertenezca a su dependencia.
El usuario analistas podrá leer todas las tareas.

### Registrar Avances a las Acciones

En esta actividad el usuario ejecutor registra una descripción del avance, definiendo lo realizado en el transcurso del mes relacionado a la acción y las tareas que abarque dicha acción. El usuario OSI le corresponde realizar la cuantificación de este avance mensual.

#### Historias de Usuario

- SPM-05: como Administrador del módulo quiero abrir el sistema para permitir el registro de avances a las actividades.

    - [1] **Abrir sistema para registro de actividades**: Al estar abierto el sistema los jefes de dependencia podrán adicionar avances a las actividades de sus planes de mejoramiento. No se podrán editar avances de periodos anteriores.
    - [2] **Cerrar sistema para registro de actividades**: Al estar cerrado el sistema los jefes de dependencia NO podrán adicionar ni editar los avances de las actividades de sus planes de mejoramiento.

- SPM-06: como responsable quiero registrar avances de una actividad para soportar lo realizado durante el mes a esta actividad encomendada a realizar.

    - [1] **Registrar Avance de Plan de Mejoramiento**: El sistema debe permitir diligenciar los campos: descripción avance, fecha de corte.
    - [2] **Restricción de dominio de Avances**: La creación del avance la puede realizar el jefe de la dependencia. La edición la puede realizar solo para los avances del periodo actual, no podrá editar los avances ya registrados para otros periodos.
Los auditores OCI sólo podrá consultar los avances.
El usuario analistas sólo podrá leer los avances.

### Calificación de Avances

El auditor OCI registra un porcentaje y asigna un estado a los avances realizados por el área encargada en el transcurso de mes.

#### Historias de Usuario

- SPM-07: como usuario administrador requiero parametrizar las diferentes calificaciones disponibles en el sistema de acuerdo al tipo de plan de mejoramiento.

    - [1] **Registrar Tipos de Calificaciones de Avances**: El sistema debe permitir diligenciar los campos: Nombre de la calificación, tipo de plan de mejoramiento al que aplica, estado interno (sin iniciar, en progreso, bloqueado, terminado, terminado con retraso)
    - [2] **Restricción de dominio de Tipos de Calificaciones Avances**: La creación/Edición la puede realizar el administrador y puede ser consultada por todos los usuarios.

- SPM-08: como usuario OCI quiero registrar una calificación a los Avances de una Acción; para darle un control porcentual al trabajo de las áreas relacionadas en el transcurso del mes.

    - [1] **Registrar Calificación de Avances**: El sistema debe permitir diligenciar los campos: Cuantificación de Cumplimiento, Estado (Selección).
    - [2] **Notificar Calificación de Avances**: El sistema debe enviar un correo electrónico a los jefe de dependencia notificando dicha calificación.
    - [3] **Registrar Cambios de Avances**: Se permite la modificación de calificaciones pero el sistema debe guardar el registro de los cambios realizados en la calificación.
    - [4] **Restricciones de dominio**: Sólo puede calificar el auditor responsable del plan de mejoramiento al que pertenece la actividad. Los jefes de dependencia sólo pueden consultar la calificación.

### Registrar Observaciones

Se registra las observaciones a los avances reportados mensualmente.

#### Historias de Usuario
- SPM-09: como usuario quiero registrar una observación a un  avance de las actividades establecidas en un plan de mejoramiento, esto con el fin de darle seguimiento y cumplimiento a las acciones.

    - [1] **Registrar Observación a Avances de Plan de Mejoramiento**: El sistema debe permitir diligenciar los campos: descripción observación.


### Generar Reportes

En esta actividad los usuarios podrán acceder a los distintos tipos de reportes para el control, seguimiento y monitoreo del plan de mejoramiento.

#### Historias de Usuario Generar Reportes
- SPM-10: como usuario quiero generara un reporte de los planes de mejoramiento para entregarlos a los distintos entes de control internos como externos.

    - [1] **Generar Informe formato internos**: El sistema debe proporcionar un archivo consolidado con la información de los planes de mejoramiento en el formato requerido
    - [3] **Generar Informe formato Contraloría General**: El sistema debe proporcionar un archivo consolidado con la información de los planes de mejoramiento en el formato requerido
    - [2] **Generar Informe formato Contraloría Distrital**: El sistema debe proporcionar un archivo consolidado con la información de los planes de mejoramiento en el formato requerido

- SPM-11: como usuario quiero acceder a un semáforo que me muestre el estado de avance o retraso de las actividades de acuerdo a las fechas programadas

    - [1] **Despliegue de Semáforo**: El sistema debe desplegar en verde las actividades terminada antes de las fecha final programada, en amarillo las terminas con retraso y en rojo las vencidas y no terminadas


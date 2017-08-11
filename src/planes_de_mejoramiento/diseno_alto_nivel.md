[[
title: Documento de diseño de Alto Nivel del Proceso Registro y Seguimiento Planes de Mejoramiento
author: José Javier Vargas Serrato
]]
Diseño de Alto Nivel
====================

Registro y Seguimiento Planes de Mejoramiento
============================

[TOC]

Diagrama de Clases de Alto Nivel
--------------------------------

{uml}

    hr.department "1" --> "*" plan_mejoramiento.plan: dependencia_id
    plan_mejoramiento.plan --|> project.project: Delegation[project_id]
    plan_mejoramiento.plan "1" --> "*" plan_mejoramiento.hallazgo
    plan_mejoramiento.hallazgo "1" --> "*" plan_mejoramiento.accion
    plan_mejoramiento.accion "1" --> "*" plan_mejoramiento.avance
    plan_mejoramiento.avance "1" --> "*" plan_mejoramiento.tipo_calificacion
    plan_mejoramiento.accion "1" -- "*" project.task
    plan_mejoramiento.hallazgo --|> project.wbs: Delegation[wbs_root_id]
    plan_mejoramiento.accion --|> project.wbs: Delegation[wbs_root_id]
    project.project "*" -- "1" project.wbs

{enduml}

Modelo de Máquina de Estados
----------------------------

### Acciones

#### Diagrama de Estados

{uml}

    [*] -> Nuevo
    Nuevo -> Por_Aprobar
    Por_Aprobar -> Aprobada
    Por_Aprobar -down> Rechazado
    Rechazado -> Por_Aprobar
    Aprobada -> En_Progreso
    En_Progreso -> Terminada
    Rechazado -up-> Cancelada
    En_Progreso -up-> Cancelada
    Aprobada -up-> Cancelada
    Terminada -> [*]

{enduml}

- **Nuevo**
    - **Descripción:** Estado inicial y por defecto el que se asigna cuando se crea el registro de la acción.
    - **Acción a ejecutar en el sistema:**No aplica.

- **Por_Aprobar**
    - **Descripción:** Estado que indica que la acción ha sido asignada y esta a la espera de la aprobación por el usuario jefe_dependencia.
    - **Acción a ejecutar en el sistema:** El campo state cambia al valor por_aprobar. Se notifica al usuario jefe_dependencia que se le ha asignado una nueva acción a su dependencia por medio de correo electrónico y en el menú de mensajes del sistema.

- **Rechazada**
    - **Descripción:** Estado que indica que la acción no fue aceptada por el jefe_dependencia.
    - **Acción a ejecutar en el sistema:**El campo state cambia al valor rechazado, y se notifica en el sistema el rechazo al usuario OCI propietario de la acción.

- **Aprobada**
    - **Descripción:** Estado que indica que el usuario jefe_dependencia ha aprobado la acción.
    - **Acción a ejecutar en el sistema:** El campo state cambia al valor aprobado y se notifica en el sistema la aceptación de la acción al usuario OCI.

- **En_Progreso**
    - **Descripción:** Estado que indica que la acción se encuentra asignada y en ejecucion por un jefe_dependencia.
    - **Acción a ejecutar en el sistema:**El campo state cambia al valor en_progreso.

- **Terminada**
    - **Descripción:** Estado que indica que la acción ha sido cumplida o terminada por el jefe_dependencia correspondiente al área a la cual se asignó la acción.
    - **Acción a ejecutar en el sistema:**El campo state cambia al valor terminada.

- **Cancelada**
    - **Descripción:** Estado que indica la acción ya no está activa en el sistema.
    - **Acción a ejecutar en el sistema:**El campo state cambia al valor cancelado.

#### Transiciones

- **Nuevo a Por_Aprobar**
    - **Validación**: El campo dependencia_id debe estar diligenciado.
    - **Grupo**: OCI
    - **Acción disparadora/trigger**: Acción manual a través de boton.

- **Por_Aprobar a Aprobada**
    - **Validación**: No aplica.
    - **Grupo**: jefe_dependencia
    - **Acción disparadora/trigger**: Acción manual a través de boton.

- **Por_Aprobar a Rechazada**
    - **Validación**: No aplica.
    - **Grupo**: jefe_dependencia
    - **Acción disparadora/trigger**: Acción manual a través de boton.

- **Rechazada a Por_Aprobar**
    - **Validación**: No aplica.
    - **Grupo**: OCI
    - **Acción disparadora/trigger**: Acción manual a través de boton.

- **Aprobada a En_Progreso**
    - **Validación**: No aplica.
    - **Grupo**: jefe_dependencia
    - **Acción disparadora/trigger**: Acción manual a través de boton.

- **En_Progreso a Terminada**
    - **Validación**: No aplica.
    - **Grupo**: OCI
    - **Acción disparadora/trigger**: Acción manual a través de boton.

- **En_Progreso a Cancelada**
    - **Validación**: No aplica.
    - **Grupo**: OCI
    - **Acción disparadora/trigger**: Acción manual a través de boton.

- **Aprobada a Cancelada**
    - **Validación**: No aplica.
    - **Grupo**: OCI
    - **Acción disparadora/trigger**: Acción manual a través de boton.

Modelo de Seguridad y Control de Acceso
---------------------------------------

A continuación se listan los permisos que tendrán los grupos de usuario sobre los diferentes objetos de negocio del sistema y las restricciones de dominio.


|Nombre del Grupo|Objeto de negocio|Permisos||||Restricciones de dominio|
|--------|-------|--------|-------|--------|-------|-------|
|||**Leer**|**Actualizar**|**Crear**|**Borrar**||
|Analistas|Plan de Mejoramiento|X|-|-|-|-|
|Analistas|Hallazgo|X|-|-|-|-|
|Analistas|Acciones|X|-|-|-|-|
|Analistas|Avances|X|-|-|-|-|
|Analistas|Observaciones|X|X|X|-|-|
|jefe_dependencia|Plan de Mejoramiento|X|-|-|-|Solo puede ver los registros asociado a su dependencia|
|jefe_dependencia|Hallazgo|X|-|-|-|Solo puede ver los registros asociado a su dependencia|
|jefe_dependencia|Accion|X|-|-|-|Solo puede ver los registros asociado a su dependencia|
|jefe_dependencia|Avances|X|X|X|-|-|
|jefe_dependencia|obaservaciones|X|X|X|-|Solo puede ver los registros asociado a su dependencia|
|responsable_tarea|Plan de Mejoramiento|X|-|-|-|Solo puede ver los registros asociado a su dependencia|
|responsable_tarea|Hallazgo|X|-|-|-|Solo puede ver los registros asociado a su dependencia|
|responsable_tarea|Accion|X|-|-|-|Solo puede ver los registros asociado a su dependencia|
|responsable_tarea|Avances|X|X|X|-|-|
|responsable_tarea|obaservaciones|X|X|X|-|Solo puede ver los registros asociado a su dependencia|
|oci|Plan de Mejoramiento|X|X|X|-|-|
|oci|Hallazgo|X|X|X|-|-|
|oci|Acciones|X|X|X|-|-|
|oci|Avances|X|X|-|-|-|
|oci|Observaciones|X|X|X|X|-|
|administrador|Plan de Mejoramiento|X|X|X|X|-|
|administrador|Hallazgo|X|X|X|X|-|
|administrador|Acciones|X|X|X|X|-|
|administrador|Avances|X|X|X|-|-|
|administrador|Observaciones|X|X|X|X|-|
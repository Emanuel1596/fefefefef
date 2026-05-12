# Entidades y atributos de base de datos

## Proyecto: EduTech - Plataforma Web de Cursos

Este documento contiene las entidades principales, entidades de catalogo, explicacion breve y atributos del modelo de datos.

## Criterios usados

1. Una entidad representa informacion importante que el sistema necesita guardar.
2. Los atributos deben ser claros y atomicos.
3. Los datos repetidos y controlados se manejan mediante catalogos.
4. Si un dato solo tiene dos estados, como activo/inactivo, se puede manejar con `BOOLEAN`.
5. Si un dato tiene varios estados de negocio, como pendiente, aprobado, rechazado o cancelado, se maneja con catalogo.
6. Botones, pantallas, secciones, menus y dashboards no son entidades de base de datos.
7. Los datos calculables, como porcentaje de avance, no se guardan como atributo fijo si pueden obtenerse mediante consulta.
8. Las contrasenas no se guardan en texto plano; se guarda `password_hash`.
9. Los cambios de estado normalmente los ejecuta el backend.
10. Una orden puede incluir varios cursos; por eso existe `Orden_Detalle`.

---

# Entidades de catalogo

## Rol

Representa los roles principales del sistema.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_rol | INTEGER PK |
| nombre_rol | VARCHAR(30) |

## Estado_Solicitud_Instructor

Representa el estado de una solicitud para convertirse en instructor.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_solicitud | INTEGER PK |
| nombre_estado_solicitud | VARCHAR(30) |

## Nivel_Curso

Representa el nivel de dificultad de un curso.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_nivel_curso | INTEGER PK |
| nombre_nivel | VARCHAR(30) |

## Estado_Curso

Representa el estado general de un curso.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_curso | INTEGER PK |
| nombre_estado_curso | VARCHAR(30) |

## Estado_Revision_Curso

Representa el resultado de una revision administrativa de curso.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_revision_curso | INTEGER PK |
| nombre_estado_revision_curso | VARCHAR(30) |

## Tipo_Video

Representa el tipo de video principal de una leccion.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_tipo_video | INTEGER PK |
| nombre_tipo_video | VARCHAR(30) |

## Tipo_Recurso

Representa los tipos de recursos adicionales de una leccion.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_tipo_recurso | INTEGER PK |
| nombre_tipo_recurso | VARCHAR(30) |

## Moneda

Representa las monedas permitidas en ordenes y pagos.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_moneda | INTEGER PK |
| codigo_moneda | CHAR(3) |
| nombre_moneda | VARCHAR(30) |

## Estado_Orden

Representa los estados de una orden de compra.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_orden | INTEGER PK |
| nombre_estado_orden | VARCHAR(30) |

## Entidad_Federativa

Representa estados de Mexico usados en datos de compra o facturacion.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_entidad_federativa | INTEGER PK |
| nombre_entidad_federativa | VARCHAR(80) |

## Ciudad

Representa ciudades relacionadas con una entidad federativa.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_ciudad | INTEGER PK |
| nombre_ciudad | VARCHAR(80) |
| id_entidad_federativa | INTEGER FK |

## Proveedor_Pago

Representa las pasarelas de pago permitidas.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_proveedor_pago | INTEGER PK |
| nombre_proveedor | VARCHAR(30) |

## Estado_Pago

Representa los estados posibles de un pago.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_pago | INTEGER PK |
| nombre_estado_pago | VARCHAR(30) |

## Estado_Webhook

Representa el estado de procesamiento de una notificacion recibida desde la pasarela.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_webhook | INTEGER PK |
| nombre_estado_webhook | VARCHAR(30) |

## Estado_Inscripcion

Representa los estados posibles de una inscripcion.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_inscripcion | INTEGER PK |
| nombre_estado_inscripcion | VARCHAR(30) |

## Estado_Examen

Representa los estados posibles de un examen.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_examen | INTEGER PK |
| nombre_estado_examen | VARCHAR(30) |

## Estado_Intento

Representa el estado de un intento de examen.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_intento | INTEGER PK |
| nombre_estado_intento | VARCHAR(30) |

---

# Entidades principales

## Usuario

Representa a alumnos, instructores y administradores. El rol se controla con `id_rol`.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_usuario | INTEGER PK |
| nombre | VARCHAR(50) |
| apellido_paterno | VARCHAR(50) |
| apellido_materno | VARCHAR(50) |
| correo_electronico | VARCHAR(100) |
| password_hash | VARCHAR(255) |
| telefono | CHAR(10) |
| esta_activo | BOOLEAN |
| fecha_registro | TIMESTAMP |
| fecha_actualizacion | TIMESTAMP |
| id_rol | INTEGER FK |

## Solicitud_Instructor

Representa la solicitud de una persona para ser habilitada como instructor.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_solicitud | INTEGER PK |
| nombre_solicitante | VARCHAR(50) |
| apellido_paterno_solicitante | VARCHAR(50) |
| apellido_materno_solicitante | VARCHAR(50) |
| correo_solicitante | VARCHAR(100) |
| telefono_solicitante | CHAR(10) |
| area_experiencia | VARCHAR(100) |
| portafolio_url | VARCHAR(255) |
| motivo | TEXT |
| experiencia | TEXT |
| fecha_solicitud | TIMESTAMP |
| fecha_revision | TIMESTAMP |
| comentario_revision | TEXT |
| id_usuario_solicitante | INTEGER FK |
| id_usuario_revisor | INTEGER FK |
| id_estado_solicitud | INTEGER FK |

## Curso

Representa los cursos creados por instructores.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_curso | INTEGER PK |
| titulo | VARCHAR(150) |
| descripcion | TEXT |
| imagen_portada | VARCHAR(255) |
| precio_mxn | NUMERIC(10,2) |
| fecha_creacion | TIMESTAMP |
| fecha_actualizacion | TIMESTAMP |
| id_usuario | INTEGER FK |
| id_nivel_curso | INTEGER FK |
| id_estado_curso | INTEGER FK |

Nota: `imagen_portada` guarda una ruta o URL de la imagen, no el archivo binario dentro de la base de datos.

## Revision_Curso

Representa revisiones administrativas hechas a un curso.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_revision_curso | INTEGER PK |
| comentario | TEXT |
| fecha_revision | TIMESTAMP |
| id_curso | INTEGER FK |
| id_estado_revision_curso | INTEGER FK |
| id_usuario_revisor | INTEGER FK |

## Modulo

Representa divisiones internas de un curso.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_modulo | INTEGER PK |
| titulo | VARCHAR(150) |
| numero_orden | INTEGER |
| id_curso | INTEGER FK |

## Leccion

Representa una leccion perteneciente a un modulo.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_leccion | INTEGER PK |
| titulo | VARCHAR(150) |
| numero_orden | INTEGER |
| texto_descriptivo | TEXT |
| url_video | VARCHAR(255) |
| duracion_segundos | INTEGER |
| esta_activa | BOOLEAN |
| id_tipo_video | INTEGER FK |
| id_modulo | INTEGER FK |

## Recurso

Representa materiales adicionales de una leccion.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_recurso | INTEGER PK |
| titulo | VARCHAR(150) |
| descripcion | VARCHAR(255) |
| url_recurso | VARCHAR(255) |
| fecha_creacion | TIMESTAMP |
| id_tipo_recurso | INTEGER FK |

## Leccion_Recurso

Representa la relacion entre lecciones y recursos.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_recurso | INTEGER PK/FK |
| id_leccion | INTEGER PK/FK |
| numero_orden | INTEGER |

## Orden

Representa la compra general generada por un alumno. Una orden puede incluir varios cursos.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_orden | INTEGER PK |
| numero_orden | VARCHAR(20) |
| total | NUMERIC(10,2) |
| fecha_creacion | TIMESTAMP |
| fecha_actualizacion | TIMESTAMP |
| id_moneda | INTEGER FK |
| id_estado_orden | INTEGER FK |
| id_usuario | INTEGER FK |

## Orden_Detalle

Representa cada curso incluido dentro de una orden.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_orden_detalle | INTEGER PK |
| precio_unitario | NUMERIC(10,2) |
| id_orden | INTEGER FK |
| id_curso | INTEGER FK |

## Datos_Compra

Guarda los datos usados en una orden especifica.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_orden | INTEGER PK/FK |
| nombre_contacto | VARCHAR(50) |
| apellido_paterno_contacto | VARCHAR(50) |
| apellido_materno_contacto | VARCHAR(50) |
| correo_electronico_contacto | VARCHAR(100) |
| telefono_contacto | CHAR(10) |
| direccion | VARCHAR(180) |
| codigo_postal | CHAR(5) |
| id_ciudad | INTEGER FK |

## Pago

Representa la transaccion o intento de pago asociado a una orden. El metodo se identifica con `Proveedor_Pago`.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_pago | INTEGER PK |
| id_pago_externo | VARCHAR(120) |
| monto_pagado | NUMERIC(10,2) |
| fecha_pago | TIMESTAMP |
| id_proveedor_pago | INTEGER FK |
| id_orden | INTEGER FK |
| id_estado_pago | INTEGER FK |

## Webhook_Pago

Guarda notificaciones recibidas desde la pasarela de pago.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_webhook | INTEGER PK |
| tipo_evento | VARCHAR(100) |
| id_evento_externo | VARCHAR(150) |
| contenido_evento | JSONB |
| fecha_recibido | TIMESTAMP |
| id_estado_webhook | INTEGER FK |
| id_pago | INTEGER FK |

## Inscripcion

Representa el acceso educativo generado por cada curso comprado dentro de una orden.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_inscripcion | INTEGER PK |
| fecha_inscripcion | TIMESTAMP |
| fecha_finalizacion | TIMESTAMP |
| id_estado_inscripcion | INTEGER FK |
| id_orden_detalle | INTEGER FK |

## Progreso_Leccion

Registra lecciones completadas dentro de una inscripcion.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_inscripcion | INTEGER PK/FK |
| id_leccion | INTEGER PK/FK |
| completada | BOOLEAN |
| fecha_completada | TIMESTAMP |

## Examen

Representa el examen final de un curso.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_examen | INTEGER PK |
| titulo | VARCHAR(150) |
| descripcion | TEXT |
| tiempo_limite_minutos | INTEGER |
| max_intentos | INTEGER |
| calificacion_minima | NUMERIC(5,2) |
| cantidad_preguntas | INTEGER |
| id_curso | INTEGER FK |
| id_estado_examen | INTEGER FK |

## Pregunta

Representa una pregunta del examen.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_pregunta | INTEGER PK |
| texto_pregunta | TEXT |
| esta_activa | BOOLEAN |
| id_examen | INTEGER FK |

## Opcion_Respuesta

Representa opciones de respuesta para una pregunta.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_opcion | INTEGER PK |
| texto_opcion | TEXT |
| es_correcta | BOOLEAN |
| id_pregunta | INTEGER FK |

## Intento_Examen

Representa cada intento realizado por un alumno.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_intento | INTEGER PK |
| numero_intento | INTEGER |
| fecha_inicio | TIMESTAMP |
| fecha_fin | TIMESTAMP |
| calificacion | NUMERIC(5,2) |
| aprobado | BOOLEAN |
| id_inscripcion | INTEGER FK |
| id_estado_intento | INTEGER FK |

## Intento_Pregunta

Guarda las preguntas asignadas a un intento y la opcion seleccionada por el alumno.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_intento_pregunta | INTEGER PK |
| numero_orden | INTEGER |
| id_intento | INTEGER FK |
| id_pregunta | INTEGER FK |
| id_opcion_seleccionada | INTEGER FK |

## Certificado

Representa el certificado emitido al completar un curso.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_certificado | INTEGER PK |
| codigo_certificado | VARCHAR(20) |
| fecha_emision | TIMESTAMP |
| url_certificado | VARCHAR(255) |
| id_inscripcion | INTEGER FK |

## Mensaje_Contacto

Entidad opcional para guardar mensajes enviados desde contacto.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_mensaje_contacto | INTEGER PK |
| nombre | VARCHAR(80) |
| correo | VARCHAR(150) |
| asunto | VARCHAR(120) |
| mensaje | TEXT |
| fecha_envio | TIMESTAMP |
| fue_atendido | BOOLEAN |

---

# Reglas recomendadas para explicar fuera del diagrama

- `Examen.id_curso` debe ser UNIQUE porque cada curso tiene un solo examen final.
- `Orden_Detalle` debe tener UNIQUE(`id_orden`, `id_curso`) para evitar repetir el mismo curso en una orden.
- `Modulo` debe tener UNIQUE(`id_curso`, `numero_orden`) para evitar dos modulos con el mismo orden dentro del curso.
- `Leccion` debe tener UNIQUE(`id_modulo`, `numero_orden`) para evitar dos lecciones con el mismo orden dentro del modulo.
- `Leccion_Recurso` puede tener UNIQUE(`id_leccion`, `numero_orden`) para evitar dos recursos en la misma posicion dentro de una leccion.
- `Datos_Compra.id_orden` funciona como PK/FK porque cada orden tiene maximo un registro de datos de compra.

# Entidades y atributos de base de datos

## Proyecto: EduTech - Plataforma Web de Cursos

Este documento contiene únicamente las entidades principales, su explicación breve y sus atributos.

---

# Entidades de catálogo

---

## 1. Rol

Representa los roles principales del sistema.

Se usa para diferenciar permisos entre Alumno, Instructor y Administrador.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_rol | INTEGER PK |
| nombre_rol | VARCHAR(30) |

---

## 2. Nivel_Curso

Representa el nivel de dificultad de un curso.

Permite clasificar los cursos como principiante, intermedio o avanzado.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_nivel_curso | INTEGER PK |
| nombre_nivel | VARCHAR(30) |

---

## 3. Estado_Curso

Representa el estado de publicación de un curso.

Permite controlar si un curso está en borrador, pendiente de revisión, publicado o no publicado.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_curso | INTEGER PK |
| nombre_estado_curso | VARCHAR(30) |

---

## 4. Tipo_Video

Representa el tipo de video que puede tener una lección.

Permite saber si el video se mostrará como YouTube, Vimeo o archivo local.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_tipo_video | INTEGER PK |
| nombre_tipo_video | VARCHAR(30) |

---

## 5. Tipo_Recurso

Representa los tipos de recursos adicionales de una lección.

Permite diferenciar si un recurso es PDF, enlace, archivo o repositorio.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_tipo_recurso | INTEGER PK |
| nombre_tipo_recurso | VARCHAR(30) |

---

## 6. Moneda

Representa las monedas permitidas en órdenes y pagos.

Aunque EduTech use pesos mexicanos, la moneda se guarda de forma controlada para validar pagos.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_moneda | INTEGER PK |
| codigo_moneda | CHAR(3) |
| nombre_moneda | VARCHAR(50) |

---

## 7. Estado_Orden

Representa los estados de una orden de compra.

Permite controlar si una orden está pendiente, completada, cancelada, fallida o expirada.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_orden | INTEGER PK |
| nombre_estado_orden | VARCHAR(30) |

---

## 8. Estado_Federativo

Representa los estados de México usados en los datos de compra.

Se usa para normalizar la información de facturación o contacto.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_federativo | INTEGER PK |
| nombre_estado_federativo | VARCHAR(80) |

---

## 9. Ciudad

Representa las ciudades usadas en los datos de compra.

Cada ciudad pertenece a un estado federativo.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_ciudad | INTEGER PK |
| id_estado_federativo | INTEGER FK |
| nombre_ciudad | VARCHAR(80) |

---

## 10. Proveedor_Pago

Representa las pasarelas de pago permitidas.

Permite controlar proveedores como PayPal o Stripe.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_proveedor_pago | INTEGER PK |
| nombre_proveedor | VARCHAR(30) |

---

## 11. Estado_Pago

Representa los estados posibles de un pago.

Permite saber si un pago está pendiente, aprobado, rechazado o cancelado.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_pago | INTEGER PK |
| nombre_estado_pago | VARCHAR(30) |

---

## 12. Estado_Webhook

Representa el estado de procesamiento de una notificación recibida desde la pasarela de pago.

Permite saber si el evento fue recibido, procesado o falló.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_webhook | INTEGER PK |
| nombre_estado_webhook | VARCHAR(30) |

---

## 13. Estado_Inscripcion

Representa los estados posibles de una inscripción.

Permite saber si una inscripción está activa, completada o cancelada.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_inscripcion | INTEGER PK |
| nombre_estado_inscripcion | VARCHAR(30) |

---

## 14. Estado_Examen

Representa los estados posibles de un examen.

Permite saber si un examen está en borrador, activo o inactivo.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_examen | INTEGER PK |
| nombre_estado_examen | VARCHAR(30) |

---

## 15. Estado_Intento

Representa el estado de un intento de examen.

Permite saber si un intento está en progreso, finalizado, invalidado o abandonado.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_intento | INTEGER PK |
| nombre_estado_intento | VARCHAR(30) |

---

# Entidades principales

---

## 16. Usuario

Representa a las personas que utilizan el sistema.

Aquí se guardan alumnos, instructores y administradores. El rol se controla mediante `id_rol`.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_usuario | INTEGER PK |
| id_rol | INTEGER FK |
| nombre | VARCHAR(80) |
| apellido_paterno | VARCHAR(80) |
| apellido_materno | VARCHAR(80) |
| correo | VARCHAR(150) |
| password_hash | VARCHAR(255) |
| telefono | CHAR(10) |
| esta_activo | BOOLEAN |
| fecha_registro | TIMESTAMP |
| fecha_actualizacion | TIMESTAMP |

---

## 17. Curso

Representa los cursos creados dentro de la plataforma.

Cada curso pertenece a un instructor, tiene nivel, estado, descripción, imagen y precio.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_curso | INTEGER PK |
| id_usuario | INTEGER FK |
| id_nivel_curso | INTEGER FK |
| id_estado_curso | INTEGER FK |
| titulo | VARCHAR(150) |
| descripcion | TEXT |
| imagen_portada | VARCHAR(255) |
| precio_mxn | NUMERIC(10,2) |
| fecha_creacion | TIMESTAMP |
| fecha_actualizacion | TIMESTAMP |

---

## 18. Modulo

Representa las divisiones internas de un curso.

Cada módulo pertenece a un curso y tiene un orden dentro de él.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_modulo | INTEGER PK |
| id_curso | INTEGER FK |
| titulo | VARCHAR(150) |
| numero_orden | INTEGER |

---

## 19. Leccion

Representa una lección perteneciente a un módulo.

La lección contiene el contenido que consume el alumno: título, texto, video y orden.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_leccion | INTEGER PK |
| id_modulo | INTEGER FK |
| id_tipo_video | INTEGER FK |
| titulo | VARCHAR(150) |
| numero_orden | INTEGER |
| texto_descriptivo | TEXT |
| url_video | VARCHAR(255) |
| duracion_segundos | INTEGER |
| esta_activa | BOOLEAN |

---

## 20. Recurso

Representa materiales adicionales de una lección.

Puede ser PDF, enlace, archivo descargable o repositorio.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_recurso | INTEGER PK |
| id_tipo_recurso | INTEGER FK |
| titulo | VARCHAR(150) |
| descripcion | VARCHAR(255) |
| url_recurso | VARCHAR(255) |
| fecha_creacion | TIMESTAMP |

---

## 21. Leccion_Recurso

Representa la relación entre lecciones y recursos.

Se usa porque una lección puede tener varios recursos y un recurso puede reutilizarse en varias lecciones.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_leccion | INTEGER PK/FK |
| id_recurso | INTEGER PK/FK |
| numero_orden | INTEGER |

---

## 22. Orden

Representa el pedido generado cuando un alumno inicia la compra de un curso.

La orden guarda usuario, curso, total, moneda y estado de la compra.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_orden | INTEGER PK |
| numero_orden | CHAR(20) |
| id_usuario | INTEGER FK |
| id_curso | INTEGER FK |
| id_moneda | INTEGER FK |
| id_estado_orden | INTEGER FK |
| total | NUMERIC(10,2) |
| fecha_creacion | TIMESTAMP |
| fecha_actualizacion | TIMESTAMP |

---

## 23. Datos_Compra

Guarda los datos de contacto y facturación capturados durante una compra.

Se guarda separado del usuario porque representa los datos usados en una orden específica.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_orden | INTEGER PK/FK |
| nombre_contacto | VARCHAR(80) |
| apellido_paterno_contacto | VARCHAR(80) |
| apellido_materno_contacto | VARCHAR(80) |
| correo_contacto | VARCHAR(150) |
| telefono_contacto | CHAR(10) |
| direccion | VARCHAR(180) |
| id_ciudad | INTEGER FK |
| codigo_postal | CHAR(5) |

---

## 24. Pago

Representa el pago asociado a una orden.

Guarda el proveedor, estado del pago, monto confirmado y el identificador externo de la pasarela.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_pago | INTEGER PK |
| id_orden | INTEGER FK |
| id_proveedor_pago | INTEGER FK |
| id_estado_pago | INTEGER FK |
| id_moneda | INTEGER FK |
| id_pago_externo | VARCHAR(120) |
| monto_pagado | NUMERIC(10,2) |
| fecha_pago | TIMESTAMP |

---

## 25. Webhook_Pago

Guarda las notificaciones recibidas desde PayPal o Stripe.

Permite saber qué evento llegó, cuándo llegó y si fue procesado.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_webhook | INTEGER PK |
| id_pago | INTEGER FK |
| id_proveedor_pago | INTEGER FK |
| id_estado_webhook | INTEGER FK |
| tipo_evento | VARCHAR(100) |
| id_evento_externo | VARCHAR(150) |
| contenido_evento | JSONB |
| fecha_recibido | TIMESTAMP |

---

## 26. Inscripcion

Representa el acceso de un alumno a un curso comprado.

Se crea cuando el pago fue aprobado y la orden queda completada.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_inscripcion | INTEGER PK |
| id_usuario | INTEGER FK |
| id_curso | INTEGER FK |
| id_orden | INTEGER FK |
| id_estado_inscripcion | INTEGER FK |
| fecha_inscripcion | TIMESTAMP |
| fecha_finalizacion | TIMESTAMP |

---

## 27. Progreso_Leccion

Registra las lecciones completadas por un alumno dentro de una inscripción.

Permite calcular el porcentaje de avance sin guardar un porcentaje fijo.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_inscripcion | INTEGER PK/FK |
| id_leccion | INTEGER PK/FK |
| completada | BOOLEAN |
| fecha_completada | TIMESTAMP |

---

## 28. Examen

Representa el examen final de un curso.

Guarda tiempo límite, intentos máximos, calificación mínima y cantidad de preguntas.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_examen | INTEGER PK |
| id_curso | INTEGER FK |
| id_estado_examen | INTEGER FK |
| titulo | VARCHAR(150) |
| descripcion | TEXT |
| tiempo_limite_minutos | INTEGER |
| max_intentos | INTEGER |
| calificacion_minima | NUMERIC(5,2) |
| cantidad_preguntas | INTEGER |

---

## 29. Pregunta

Representa una pregunta del banco de preguntas.

Cada pregunta pertenece a un examen.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_pregunta | INTEGER PK |
| id_examen | INTEGER FK |
| texto_pregunta | TEXT |
| esta_activa | BOOLEAN |

---

## 30. Opcion_Respuesta

Representa las opciones de respuesta de una pregunta.

Cada pregunta puede tener varias opciones y una puede marcarse como correcta.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_opcion | INTEGER PK |
| id_pregunta | INTEGER FK |
| texto_opcion | TEXT |
| es_correcta | BOOLEAN |

---

## 31. Intento_Examen

Representa cada intento realizado por un alumno al presentar un examen.

Permite controlar intentos, calificación, fechas y aprobación.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_intento | INTEGER PK |
| id_examen | INTEGER FK |
| id_inscripcion | INTEGER FK |
| id_estado_intento | INTEGER FK |
| numero_intento | INTEGER |
| fecha_inicio | TIMESTAMP |
| fecha_fin | TIMESTAMP |
| calificacion | NUMERIC(5,2) |
| aprobado | BOOLEAN |

---

## 32. Pregunta_Intento

Guarda las preguntas asignadas a un intento de examen.

Se usa porque el examen genera preguntas aleatorias y se necesita conservar cuáles recibió el alumno.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_intento | INTEGER PK/FK |
| id_pregunta | INTEGER PK/FK |
| numero_orden | INTEGER |

---

## 33. Respuesta_Alumno

Guarda la respuesta seleccionada por el alumno en cada pregunta del intento.

También guarda si fue correcta para conservar el resultado histórico.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_intento | INTEGER PK/FK |
| id_pregunta | INTEGER PK/FK |
| id_opcion | INTEGER FK |
| es_correcta | BOOLEAN |

---

## 34. Certificado

Representa el certificado emitido al completar un curso.

Se genera cuando el alumno completa todas las lecciones y aprueba el examen final.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_certificado | INTEGER PK |
| id_inscripcion | INTEGER FK |
| codigo_certificado | CHAR(20) |
| fecha_emision | TIMESTAMP |
| url_certificado | VARCHAR(255) |

---

# Entidad opcional

---

## 35. Mensaje_Contacto

Representa los mensajes enviados desde la pantalla de contacto.

Esta entidad solo se usa si EduTech guardará los mensajes en la base de datos. Si el formulario solo envía correo y no guarda nada, se puede omitir.

| Atributo | Tipo de dato sugerido |
|---|---|
| id_mensaje_contacto | INTEGER PK |
| nombre | VARCHAR(80) |
| correo | VARCHAR(150) |
| asunto | VARCHAR(120) |
| mensaje | TEXT |
| fecha_envio | TIMESTAMP |
| fue_atendido | BOOLEAN |

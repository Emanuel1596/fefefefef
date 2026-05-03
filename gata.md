# Identificación de entidades y atributos de base de datos

## Proyecto: EduTech - Plataforma Web de Cursos

## Objetivo

Este documento presenta las entidades y atributos principales que se identificaron para la base de datos del sistema EduTech.

La identificación se realizó tomando en cuenta las funciones principales del sistema: registro de usuarios, manejo de roles, cursos, módulos, lecciones, compra de cursos, pagos, inscripciones, progreso, examen final, banco de preguntas, respuestas y certificados.

---

# Entidades y atributos

## 1. Rol

Entidad que representa el tipo de usuario dentro del sistema.

**Atributos:**

- id_rol
- nombre_rol

---

## 2. Estado_Usuario

Entidad que representa el estado de una cuenta de usuario.

**Atributos:**

- id_estado_usuario
- nombre_estado_usuario

---

## 3. Usuario

Entidad que representa a las personas que usan el sistema, ya sean alumnos, instructores o administradores.

**Atributos:**

- id_usuario
- id_rol
- id_estado_usuario
- nombre
- apellido_paterno
- apellido_materno
- correo
- password_hash
- telefono
- fecha_registro
- fecha_actualizacion

---

## 4. Nivel_Curso

Entidad que representa el nivel de dificultad de un curso.

**Atributos:**

- id_nivel_curso
- nombre_nivel

---

## 5. Estado_Curso

Entidad que representa el estado de publicación de un curso.

**Atributos:**

- id_estado_curso
- nombre_estado_curso

---

## 6. Curso

Entidad que representa los cursos disponibles dentro de la plataforma.

**Atributos:**

- id_curso
- id_instructor
- id_nivel_curso
- id_estado_curso
- titulo
- resumen
- descripcion
- imagen_portada
- precio_mxn
- fecha_creacion
- fecha_actualizacion

---

## 7. Modulo

Entidad que representa las divisiones internas de un curso.

**Atributos:**

- id_modulo
- id_curso
- titulo
- numero_orden

---

## 8. Tipo_Video

Entidad que representa el tipo de video utilizado en una lección.

**Atributos:**

- id_tipo_video
- nombre_tipo_video

---

## 9. Estado_Leccion

Entidad que representa el estado de una lección.

**Atributos:**

- id_estado_leccion
- nombre_estado_leccion

---

## 10. Leccion

Entidad que representa cada lección perteneciente a un módulo.

**Atributos:**

- id_leccion
- id_modulo
- id_tipo_video
- id_estado_leccion
- titulo
- numero_orden
- texto_descriptivo
- url_video
- duracion_segundos

---

## 11. Tipo_Recurso

Entidad que representa el tipo de recurso adicional que puede tener una lección.

**Atributos:**

- id_tipo_recurso
- nombre_tipo_recurso

---

## 12. Recurso

Entidad que representa los materiales adicionales que pueden asociarse a una lección.

**Atributos:**

- id_recurso
- id_tipo_recurso
- id_creador
- titulo
- descripcion
- url_recurso
- fecha_creacion

---

## 13. Leccion_Recurso

Entidad intermedia que permite relacionar lecciones con recursos.

**Atributos:**

- id_leccion
- id_recurso
- numero_orden

---

## 14. Moneda

Entidad que representa la moneda utilizada en órdenes y pagos.

**Atributos:**

- id_moneda
- codigo_moneda
- nombre_moneda

---

## 15. Estado_Orden

Entidad que representa el estado de una orden de compra.

**Atributos:**

- id_estado_orden
- nombre_estado_orden

---

## 16. Orden

Entidad que representa el pedido generado cuando un alumno inicia la compra de un curso.

**Atributos:**

- id_orden
- numero_orden
- id_alumno
- id_curso
- id_moneda
- id_estado_orden
- total
- fecha_creacion
- fecha_actualizacion

---

## 17. Datos_Compra

Entidad que guarda los datos de contacto y facturación capturados durante la compra.

**Atributos:**

- id_orden
- nombre_contacto
- apellido_paterno_contacto
- apellido_materno_contacto
- correo_contacto
- telefono_contacto
- direccion_linea_1
- direccion_linea_2
- ciudad
- estado_federativo
- codigo_postal

---

## 18. Proveedor_Pago

Entidad que representa las pasarelas de pago permitidas por el sistema.

**Atributos:**

- id_proveedor_pago
- nombre_proveedor

---

## 19. Estado_Pago

Entidad que representa el estado de un pago.

**Atributos:**

- id_estado_pago
- nombre_estado_pago

---

## 20. Pago

Entidad que representa el pago asociado a una orden.

**Atributos:**

- id_pago
- id_orden
- id_proveedor_pago
- id_estado_pago
- id_moneda
- id_pago_externo
- monto
- fecha_pago

---

## 21. Estado_Webhook

Entidad que representa el estado de procesamiento de una notificación recibida desde la pasarela de pago.

**Atributos:**

- id_estado_webhook
- nombre_estado_webhook

---

## 22. Webhook_Pago

Entidad que guarda las notificaciones enviadas por la pasarela de pago al sistema.

**Atributos:**

- id_webhook
- id_pago
- id_proveedor_pago
- id_estado_webhook
- tipo_evento
- id_evento_externo
- contenido_evento
- fecha_recibido

---

## 23. Estado_Inscripcion

Entidad que representa el estado de una inscripción.

**Atributos:**

- id_estado_inscripcion
- nombre_estado_inscripcion

---

## 24. Inscripcion

Entidad que representa el acceso de un alumno a un curso comprado.

**Atributos:**

- id_inscripcion
- id_alumno
- id_curso
- id_orden
- id_estado_inscripcion
- fecha_inscripcion
- fecha_finalizacion

---

## 25. Progreso_Leccion

Entidad que registra las lecciones completadas por un alumno dentro de una inscripción.

**Atributos:**

- id_inscripcion
- id_leccion
- completada
- fecha_completada

---

## 26. Estado_Examen

Entidad que representa el estado del examen final.

**Atributos:**

- id_estado_examen
- nombre_estado_examen

---

## 27. Examen

Entidad que representa el examen final de un curso.

**Atributos:**

- id_examen
- id_curso
- id_estado_examen
- titulo
- descripcion
- tiempo_limite_minutos
- max_intentos
- calificacion_minima
- cantidad_preguntas

---

## 28. Estado_Pregunta

Entidad que representa el estado de una pregunta del banco de preguntas.

**Atributos:**

- id_estado_pregunta
- nombre_estado_pregunta

---

## 29. Pregunta

Entidad que representa una pregunta perteneciente al banco de preguntas de un examen.

**Atributos:**

- id_pregunta
- id_examen
- id_estado_pregunta
- texto_pregunta

---

## 30. Opcion_Respuesta

Entidad que representa las opciones de respuesta de una pregunta.

**Atributos:**

- id_opcion
- id_pregunta
- texto_opcion
- es_correcta

---

## 31. Estado_Intento

Entidad que representa el estado de un intento de examen.

**Atributos:**

- id_estado_intento
- nombre_estado_intento

---

## 32. Intento_Examen

Entidad que representa cada intento realizado por un alumno al presentar un examen.

**Atributos:**

- id_intento
- id_examen
- id_inscripcion
- id_estado_intento
- numero_intento
- fecha_inicio
- fecha_fin
- calificacion
- aprobado

---

## 33. Pregunta_Intento

Entidad que guarda las preguntas asignadas a un intento de examen.

**Atributos:**

- id_intento
- id_pregunta
- numero_orden

---

## 34. Respuesta_Alumno

Entidad que guarda la respuesta seleccionada por el alumno en un intento de examen.

**Atributos:**

- id_intento
- id_pregunta
- id_opcion
- es_correcta

---

## 35. Certificado

Entidad que representa el certificado emitido cuando un alumno completa un curso.

**Atributos:**

- id_certificado
- id_inscripcion
- codigo_certificado
- fecha_emision
- url_certificado

---

## 36. Estado_Mensaje

Entidad que representa el estado de un mensaje enviado desde la pantalla de contacto.

**Atributos:**

- id_estado_mensaje
- nombre_estado_mensaje

---

## 37. Mensaje_Contacto

Entidad que representa los mensajes enviados desde el formulario de contacto.

**Atributos:**

- id_mensaje
- id_estado_mensaje
- nombre
- correo
- asunto
- mensaje
- fecha_envio

---

# Entidades principales del modelo

Las entidades principales del sistema son:

- Usuario
- Rol
- Curso
- Modulo
- Leccion
- Recurso
- Orden
- Pago
- Inscripcion
- Progreso_Leccion
- Examen
- Pregunta
- Opcion_Respuesta
- Intento_Examen
- Respuesta_Alumno
- Certificado

---

# Entidades de catálogo

Las entidades de catálogo son aquellas que guardan valores definidos y controlados. Sirven para evitar errores de escritura y mantener consistencia en la base de datos.

Las entidades de catálogo identificadas son:

- Estado_Usuario
- Nivel_Curso
- Estado_Curso
- Tipo_Video
- Estado_Leccion
- Tipo_Recurso
- Moneda
- Estado_Orden
- Proveedor_Pago
- Estado_Pago
- Estado_Webhook
- Estado_Inscripcion
- Estado_Examen
- Estado_Pregunta
- Estado_Intento
- Estado_Mensaje

---

# Entidades intermedias

Las entidades intermedias se usan para resolver relaciones de muchos a muchos o para guardar información adicional de una relación.

Las entidades intermedias identificadas son:

- Leccion_Recurso
- Pregunta_Intento
- Respuesta_Alumno

---

---

# Conclusión

La identificación de entidades y atributos permite definir qué información necesita almacenar EduTech para funcionar correctamente.

El modelo propuesto considera usuarios, roles, cursos, módulos, lecciones, recursos, compras, pagos, inscripciones, progreso, exámenes, respuestas y certificados. También se incluyen catálogos para manejar datos controlados como estados, niveles, tipos, monedas y proveedores.

Esta estructura ayuda a mantener una base de datos ordenada, normalizada y coherente con los requerimientos del sistema.

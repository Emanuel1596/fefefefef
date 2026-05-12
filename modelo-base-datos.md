# Identificacion de entidades y atributos de base de datos

## Proyecto: EduTech - Plataforma Web de Cursos

Este documento presenta las entidades, atributos y explicaciones principales del modelo de base de datos de EduTech.

El modelo considera que EduTech combina una parte de venta de cursos digitales y una parte e-learning. Por eso separa la compra en Orden y Orden_Detalle, y separa el acceso educativo en Inscripcion, Progreso_Leccion, Intento_Examen y Certificado.

---

## Criterios usados

1. Una entidad representa informacion importante que el sistema necesita guardar.
2. Los atributos deben ser claros y atomicos.
3. Los valores repetidos y controlados se manejan mediante catalogos.
4. Los botones, pantallas, secciones y menus no son entidades.
5. Los porcentajes calculables, como el avance, no se guardan como dato fijo si pueden obtenerse mediante consulta.
6. Las contraseñas no se guardan en texto plano; se guarda password_hash.
7. Una compra puede incluir varios cursos, por eso Orden no se conecta directamente con Curso; se usa Orden_Detalle.

---

## Nota sobre el uso de nombres sin recuadros grises

En este documento los nombres de tablas y atributos se escriben como texto normal, sin formato de codigo, para evitar los recuadros grises en GitHub.

---

# Entidades de catalogo y principales

---

## 1. Rol

Tipo de entidad: Catalogo.

Representa los roles principales del sistema.

Justificacion: Se usa para diferenciar permisos entre Alumno, Instructor y Administrador.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_rol | INTEGER PK |
| nombre_rol | VARCHAR(30) |

---

## 2. Usuario

Tipo de entidad: Principal.

Representa a las personas que utilizan el sistema.

Justificacion: Aqui se guardan alumnos, instructores y administradores. No se crean tablas separadas para Alumno, Instructor y Administrador porque comparten los mismos datos basicos y se diferencian mediante Rol.

### Atributos

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

---

## 3. Estado_Solicitud_Instructor

Tipo de entidad: Catalogo.

Representa los estados posibles de una solicitud de instructor.

Justificacion: Se usa como catalogo porque una solicitud puede pasar por varios estados, como pendiente, aceptada o rechazada.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_solicitud | INTEGER PK |
| nombre_estado_solicitud | VARCHAR(30) |

---

## 4. Solicitud_Instructor

Tipo de entidad: Principal.

Representa la solicitud para que un alumno pueda convertirse en instructor.

Justificacion: Se guarda porque no cualquier usuario debe publicar cursos. Primero debe existir una solicitud revisada por un administrador.

### Atributos

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

---

## 5. Nivel_Curso

Tipo de entidad: Catalogo.

Representa el nivel de dificultad de un curso.

Justificacion: Permite clasificar los cursos como principiante, intermedio o avanzado de forma controlada.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_nivel_curso | INTEGER PK |
| nombre_nivel | VARCHAR(30) |

---

## 6. Estado_Curso

Tipo de entidad: Catalogo.

Representa el estado de publicacion de un curso.

Justificacion: Se usa como catalogo porque un curso puede estar en borrador, pendiente de revision, publicado o no publicado.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_curso | INTEGER PK |
| nombre_estado_curso | VARCHAR(30) |

---

## 7. Curso

Tipo de entidad: Principal.

Representa los cursos creados dentro de la plataforma.

Justificacion: Cada curso pertenece a un instructor, tiene nivel, estado, descripcion, imagen y precio. La imagen no se guarda como archivo dentro de la base; se guarda una ruta o URL en imagen_portada.

### Atributos

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

---

## 8. Estado_Revision_Curso

Tipo de entidad: Catalogo.

Representa los estados de una revision de curso.

Justificacion: Permite controlar si una revision queda pendiente, aprobada o rechazada.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_revision_curso | INTEGER PK |
| nombre_estado_revision_curso | VARCHAR(30) |

---

## 9. Revision_Curso

Tipo de entidad: Principal.

Representa la revision realizada por un administrador sobre un curso.

Justificacion: Se guarda para conservar evidencia del comentario, fecha y administrador que reviso el curso.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_revision_curso | INTEGER PK |
| comentario | TEXT |
| fecha_revision | TIMESTAMP |
| id_curso | INTEGER FK |
| id_estado_revision_curso | INTEGER FK |
| id_usuario_revisor | INTEGER FK |

---

## 10. Modulo

Tipo de entidad: Principal.

Representa las divisiones internas de un curso.

Justificacion: Cada modulo pertenece a un curso y tiene numero_orden para saber en que posicion se muestra. No es para calculos, pero debe ser INTEGER porque se ordena numericamente.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_modulo | INTEGER PK |
| titulo | VARCHAR(150) |
| numero_orden | INTEGER |
| id_curso | INTEGER FK |

---

## 11. Tipo_Video

Tipo de entidad: Catalogo.

Representa el tipo de video que puede tener una leccion.

Justificacion: Se justifica porque el video principal de una leccion puede mostrarse de forma distinta si es YouTube, Vimeo o local. No es lo mismo que Tipo_Recurso, porque el video es contenido principal de la leccion y el recurso es material adicional.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_tipo_video | INTEGER PK |
| nombre_tipo_video | VARCHAR(30) |

---

## 12. Leccion

Tipo de entidad: Principal.

Representa una leccion perteneciente a un modulo.

Justificacion: La leccion contiene el contenido principal que consume el alumno: titulo, orden, texto, video y estado de disponibilidad.

### Atributos

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

---

## 13. Tipo_Recurso

Tipo de entidad: Catalogo.

Representa los tipos de recursos adicionales de una leccion.

Justificacion: Permite saber si un recurso se debe mostrar como PDF, enlace, archivo o repositorio.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_tipo_recurso | INTEGER PK |
| nombre_tipo_recurso | VARCHAR(30) |

---

## 14. Recurso

Tipo de entidad: Principal.

Representa materiales adicionales de una leccion.

Justificacion: Se separa de Leccion porque una leccion puede tener varios recursos y un recurso puede reutilizarse en varias lecciones.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_recurso | INTEGER PK |
| titulo | VARCHAR(150) |
| descripcion | VARCHAR(255) |
| url_recurso | VARCHAR(255) |
| fecha_creacion | TIMESTAMP |
| id_tipo_recurso | INTEGER FK |

---

## 15. Leccion_Recurso

Tipo de entidad: Intermedia.

Representa la relacion entre lecciones y recursos.

Justificacion: Se usa porque una leccion puede tener varios recursos y un mismo recurso puede aparecer en varias lecciones. numero_orden indica la posicion del recurso dentro de una leccion.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_recurso | INTEGER PK/FK |
| id_leccion | INTEGER PK/FK |
| numero_orden | INTEGER |

---

## 16. Moneda

Tipo de entidad: Catalogo.

Representa las monedas permitidas en ordenes y pagos.

Justificacion: Aunque EduTech use pesos mexicanos, la moneda se guarda de forma controlada para validar pagos externos.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_moneda | INTEGER PK |
| codigo_moneda | CHAR(3) |
| nombre_moneda | VARCHAR(30) |

---

## 17. Estado_Orden

Tipo de entidad: Catalogo.

Representa los estados de una orden de compra.

Justificacion: Se usa porque una orden puede estar pendiente, completada, cancelada, fallida o expirada.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_orden | INTEGER PK |
| nombre_estado_orden | VARCHAR(30) |

---

## 18. Orden

Tipo de entidad: Principal.

Representa la compra general generada por un alumno.

Justificacion: Orden ya no guarda id_curso porque una orden puede incluir varios cursos. Los cursos comprados se guardan en Orden_Detalle.

### Atributos

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

---

## 19. Orden_Detalle

Tipo de entidad: Principal.

Representa cada curso incluido dentro de una orden.

Justificacion: Se crea porque una compra puede incluir varios cursos. Cada registro de Orden_Detalle representa un curso especifico dentro de una orden y guarda el precio unitario usado al momento de la compra.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_orden_detalle | INTEGER PK |
| precio_unitario | NUMERIC(10,2) |
| id_orden | INTEGER FK |
| id_curso | INTEGER FK |

---

## 20. Entidad_Federativa

Tipo de entidad: Catalogo.

Representa las entidades federativas de Mexico usadas en datos de compra.

Justificacion: Se usa para normalizar direccion o facturacion opcional.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_entidad_federativa | INTEGER PK |
| nombre_entidad_federativa | VARCHAR(80) |

---

## 21. Ciudad

Tipo de entidad: Catalogo.

Representa las ciudades usadas en datos de compra.

Justificacion: Cada ciudad pertenece a una Entidad_Federativa.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_ciudad | INTEGER PK |
| nombre_ciudad | VARCHAR(80) |
| id_entidad_federativa | INTEGER FK |

---

## 22. Datos_Compra

Tipo de entidad: Principal.

Guarda los datos de contacto y facturacion capturados durante una compra.

Justificacion: Se guarda separado de Usuario porque representa los datos usados en una orden especifica. Si el usuario cambia su perfil despues, la orden conserva los datos con los que fue realizada.

### Atributos

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

---

## 23. Proveedor_Pago

Tipo de entidad: Catalogo.

Representa las pasarelas de pago permitidas.

Justificacion: Permite controlar proveedores como PayPal o Stripe.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_proveedor_pago | INTEGER PK |
| nombre_proveedor | VARCHAR(30) |

---

## 24. Estado_Pago

Tipo de entidad: Catalogo.

Representa los estados posibles de un pago.

Justificacion: Permite saber si el pago esta pendiente, aprobado, rechazado o cancelado.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_pago | INTEGER PK |
| nombre_estado_pago | VARCHAR(30) |

---

## 25. Pago

Tipo de entidad: Principal.

Representa la transaccion o intento de pago asociado a una orden.

Justificacion: Pago no es el metodo de pago. El proveedor se guarda en Proveedor_Pago. Pago guarda el identificador externo de la pasarela, el monto confirmado y la fecha.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_pago | INTEGER PK |
| id_pago_externo | VARCHAR(120) |
| monto_pagado | NUMERIC(10,2) |
| fecha_pago | TIMESTAMP |
| id_proveedor_pago | INTEGER FK |
| id_orden | INTEGER FK |
| id_estado_pago | INTEGER FK |

---

## 26. Estado_Webhook

Tipo de entidad: Catalogo.

Representa el estado de procesamiento de una notificacion recibida desde la pasarela de pago.

Justificacion: Permite saber si el evento fue recibido, procesado o fallo.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_webhook | INTEGER PK |
| nombre_estado_webhook | VARCHAR(30) |

---

## 27. Webhook_Pago

Tipo de entidad: Principal.

Guarda las notificaciones recibidas desde PayPal o Stripe.

Justificacion: Permite conservar evidencia tecnica de lo que envio la pasarela y evitar procesar dos veces el mismo evento.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_webhook | INTEGER PK |
| tipo_evento | VARCHAR(100) |
| id_evento_externo | VARCHAR(150) |
| contenido_evento | JSONB |
| fecha_recibido | TIMESTAMP |
| id_estado_webhook | INTEGER FK |
| id_pago | INTEGER FK |

---

## 28. Estado_Inscripcion

Tipo de entidad: Catalogo.

Representa los estados posibles de una inscripcion.

Justificacion: Permite saber si la inscripcion esta activa, completada o cancelada. Activa significa que el alumno tiene acceso; completada significa que termino el curso; cancelada significa que el acceso fue revocado.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_inscripcion | INTEGER PK |
| nombre_estado_inscripcion | VARCHAR(30) |

---

## 29. Inscripcion

Tipo de entidad: Principal.

Representa el acceso educativo de un alumno a un curso comprado.

Justificacion: Se crea a partir de Orden_Detalle cuando el pago es aprobado y la orden queda completada. Si una orden contiene tres cursos, se crean tres inscripciones, una por cada Orden_Detalle.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_inscripcion | INTEGER PK |
| fecha_inscripcion | TIMESTAMP |
| fecha_finalizacion | TIMESTAMP |
| id_estado_inscripcion | INTEGER FK |
| id_orden_detalle | INTEGER FK |

---

## 30. Progreso_Leccion

Tipo de entidad: Principal.

Registra las lecciones completadas por un alumno dentro de una inscripcion.

Justificacion: Permite calcular el porcentaje de avance sin guardar un porcentaje fijo en la base de datos.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_inscripcion | INTEGER PK/FK |
| id_leccion | INTEGER PK/FK |
| completada | BOOLEAN |
| fecha_completada | TIMESTAMP |

---

## 31. Estado_Examen

Tipo de entidad: Catalogo.

Representa los estados posibles de un examen.

Justificacion: Permite saber si el examen esta en borrador, activo o inactivo.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_examen | INTEGER PK |
| nombre_estado_examen | VARCHAR(30) |

---

## 32. Examen

Tipo de entidad: Principal.

Representa el examen final de un curso.

Justificacion: Guarda tiempo limite, intentos maximos, calificacion minima y cantidad de preguntas. Cada curso debe tener como maximo un examen final.

### Atributos

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

---

## 33. Pregunta

Tipo de entidad: Principal.

Representa una pregunta del banco de preguntas.

Justificacion: Cada pregunta pertenece a un examen y puede activarse o desactivarse.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_pregunta | INTEGER PK |
| texto_pregunta | TEXT |
| esta_activa | BOOLEAN |
| id_examen | INTEGER FK |

---

## 34. Opcion_Respuesta

Tipo de entidad: Principal.

Representa las opciones de respuesta de una pregunta.

Justificacion: Cada pregunta puede tener varias opciones y una puede marcarse como correcta.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_opcion | INTEGER PK |
| texto_opcion | TEXT |
| es_correcta | BOOLEAN |
| id_pregunta | INTEGER FK |

---

## 35. Estado_Intento

Tipo de entidad: Catalogo.

Representa el estado de un intento de examen.

Justificacion: Permite saber si un intento esta en progreso, finalizado, invalidado o abandonado.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_intento | INTEGER PK |
| nombre_estado_intento | VARCHAR(30) |

---

## 36. Intento_Examen

Tipo de entidad: Principal.

Representa cada intento realizado por un alumno al presentar un examen.

Justificacion: Permite controlar numero de intento, fechas, calificacion y si el intento fue aprobado.

### Atributos

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

---

## 37. Intento_Pregunta

Tipo de entidad: Principal.

Guarda las preguntas asignadas a un intento de examen.

Justificacion: Se usa porque el examen genera preguntas aleatorias. Tambien guarda la opcion seleccionada para conservar la respuesta historica del alumno.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_intento_pregunta | INTEGER PK |
| numero_orden | INTEGER |
| id_intento | INTEGER FK |
| id_pregunta | INTEGER FK |
| id_opcion_seleccionada | INTEGER FK |

---

## 38. Certificado

Tipo de entidad: Principal.

Representa el certificado emitido al completar un curso.

Justificacion: Se genera cuando la inscripcion queda completada. El codigo_certificado es visible para identificar o validar el certificado.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_certificado | INTEGER PK |
| codigo_certificado | VARCHAR(20) |
| fecha_emision | TIMESTAMP |
| url_certificado | VARCHAR(225) |
| id_inscripcion | INTEGER FK |


---

# Explicaciones extra para defender el modelo

## Imagen de portada

El atributo imagen_portada no guarda la imagen como archivo dentro de la base de datos. Guarda la ruta o URL donde se encuentra la imagen.

Ejemplos:

| Caso | Valor posible |
|---|---|
| Imagen dentro del proyecto | frontend/assets/img/curso-java.jpg |
| Imagen subida al servidor | uploads/cursos/curso-15.webp |
| Imagen en almacenamiento externo | https://cdn.edutech.com/cursos/java.webp |

Esto evita guardar archivos pesados directamente en la base de datos.

## Tipo TEXT

TEXT se usa para textos largos cuyo tamaño puede variar mucho, como descripcion, motivo, experiencia, comentario o contenido de una pregunta.

No se usa para datos cortos como nombre, telefono o correo. Esos datos usan VARCHAR o CHAR segun corresponda.

## numero_orden

numero_orden se usa para ordenar elementos, no para hacer calculos matematicos.

Debe ser INTEGER porque representa una posicion numerica. Si fuera texto, el orden podria quedar mal, por ejemplo 1, 10, 2. Como INTEGER se ordena correctamente: 1, 2, 10.

## Tipo_Video y Tipo_Recurso

Tipo_Video indica como se reproduce el video principal de la leccion: YouTube, Vimeo o Local.

Tipo_Recurso indica como se muestra un material adicional: PDF, enlace, archivo o repositorio.

No son lo mismo porque el video es contenido principal de la leccion, mientras que los recursos son materiales complementarios.

## Orden, Orden_Detalle, Pago e Inscripcion

Orden representa la compra completa.

Orden_Detalle representa cada curso incluido en esa compra.

Pago representa la transaccion o intento de cobro asociado a la orden.

Inscripcion representa el acceso educativo que se genera por cada curso comprado.

Ejemplo:

| Proceso | Entidad |
|---|---|
| El alumno compra tres cursos | Orden |
| Cada curso comprado queda como renglon de la compra | Orden_Detalle |
| La pasarela confirma el cobro | Pago |
| El sistema libera acceso a cada curso | Inscripcion |

## Datos_Compra como PK/FK

Datos_Compra.id_orden funciona como PK/FK porque cada orden puede tener maximo un registro de datos de compra.

Esto es valido en relaciones uno a uno. Significa que el mismo identificador de la orden identifica tambien los datos de compra asociados.

## Estados de inscripcion

Estado_Inscripcion se conserva porque activa, completada y cancelada no significan lo mismo.

| Estado | Significado |
|---|---|
| activa | El alumno tiene acceso y esta cursando. |
| completada | El alumno termino el curso y cumplio las condiciones. |
| cancelada | El acceso fue revocado por una situacion especial. |

## Reglas extra de integridad

Estas reglas se explican fuera del diagrama para que el diagrama no se vea saturado.

| Regla | Explicacion |
|---|---|
| Examen.id_curso debe ser unico | Cada curso tiene un solo examen final. |
| Orden_Detalle debe evitar repetir id_orden e id_curso | Evita que el mismo curso aparezca dos veces en la misma orden. |
| Modulo debe evitar repetir id_curso y numero_orden | Evita dos modulos en la misma posicion dentro del curso. |
| Leccion debe evitar repetir id_modulo y numero_orden | Evita dos lecciones en la misma posicion dentro del modulo. |
| Leccion_Recurso debe evitar repetir id_leccion y numero_orden | Evita dos recursos en la misma posicion dentro de una leccion. |
| Intento_Examen debe evitar repetir id_inscripcion y numero_intento | Controla el numero de intentos por inscripcion. |
| Intento_Pregunta debe evitar repetir id_intento e id_pregunta | Evita que una pregunta se repita dentro del mismo intento. |
| Intento_Pregunta debe evitar repetir id_intento y numero_orden | Evita dos preguntas en la misma posicion del examen. |
| Pago debe evitar repetir id_proveedor_pago e id_pago_externo | Evita duplicar pagos externos de PayPal o Stripe. |
| Webhook_Pago debe evitar repetir id_evento_externo | Evita procesar dos veces el mismo webhook. |

## Frase para recordar

Orden guarda la compra completa; Orden_Detalle guarda cada curso comprado; Pago confirma el dinero; Inscripcion libera el acceso educativo.

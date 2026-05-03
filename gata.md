# Identificación de entidades y atributos de base de datos

## Proyecto: EduTech - Plataforma Web de Cursos

## Objetivo

Este documento presenta la identificación de entidades y atributos principales para la base de datos del sistema EduTech.

El sistema EduTech permite que los alumnos se registren, consulten cursos, compren cursos, accedan a módulos y lecciones, registren su progreso, presenten un examen final y obtengan certificados. También contempla usuarios con rol de instructor y administrador.

---

## Criterios usados para identificar entidades

Para identificar las entidades se tomaron en cuenta los siguientes criterios:

1. Una entidad representa información importante que el sistema necesita almacenar.
2. Los atributos deben ser claros y, en lo posible, atómicos.
3. Los datos repetidos y controlados se manejan mediante catálogos.
4. Los botones, pantallas, secciones, menús o dashboards no se consideran entidades de base de datos.
5. Los datos calculables, como el porcentaje de avance, no se guardan como atributo fijo si pueden obtenerse mediante consulta.
6. Las contraseñas no se guardan en texto plano; se guarda un `password_hash`.
7. Los tipos de dato se plantean con una lógica compatible con PostgreSQL.

---

## Criterio para tipos de dato

Los tipos de dato sugeridos se plantean pensando en PostgreSQL.

| Caso | Tipo sugerido |
|---|---|
| Identificadores de catálogos | SMALLINT PK |
| Identificadores de entidades principales | BIGINT PK |
| Llaves foráneas hacia catálogos | SMALLINT FK |
| Llaves foráneas hacia entidades principales | BIGINT FK |
| Texto corto | VARCHAR(n) |
| Texto largo | TEXT |
| Dinero o calificaciones | NUMERIC(p,s) |
| Fecha y hora | TIMESTAMP |
| Verdadero / falso | BOOLEAN |
| Teléfono mexicano | CHAR(10) |
| Código postal mexicano | CHAR(5) |
| Código de moneda | CHAR(3) |
| Contenido de webhook | JSONB |

---

## Nota sobre catálogos

Un catálogo es una entidad o tabla que guarda valores controlados y definidos previamente.

Por ejemplo, en lugar de escribir muchas veces el estado de un pago como texto, se crea una entidad `Estado_Pago` y en la tabla `Pago` solo se guarda el identificador correspondiente.

Esto ayuda a evitar errores como:

- aprobado
- Aprobado
- APROBADO
- aprovado

---

# Entidades y atributos

---

## 1. Rol

Representa los roles principales del sistema.

**Justificación:**  
Se crea porque EduTech maneja diferentes tipos de usuario: Alumno, Instructor y Administrador. Separar los roles en una entidad permite controlar permisos y evita repetir texto en la tabla de usuarios.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_rol | SMALLINT PK |
| nombre_rol | VARCHAR(30) |

### Registros de ejemplo

| id_rol | nombre_rol |
|---:|---|
| 1 | Alumno |
| 2 | Instructor |

---

## 2. Estado_Usuario

Representa el estado de una cuenta de usuario.

**Justificación:**  
Se crea para controlar si una cuenta está activa o inactiva. Esto permite que el administrador desactive usuarios sin eliminar su información histórica.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_usuario | SMALLINT PK |
| nombre_estado_usuario | VARCHAR(30) |

### Registros de ejemplo

| id_estado_usuario | nombre_estado_usuario |
|---:|---|
| 1 | activo |
| 2 | inactivo |

---

## 3. Usuario

Representa a las personas que utilizan el sistema.

**Justificación:**  
Se crea porque el sistema necesita almacenar los datos de alumnos, instructores y administradores. Esta entidad permite registrar usuarios, iniciar sesión, controlar el estado de la cuenta y asignar un rol principal.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_usuario | BIGINT PK |
| id_rol | SMALLINT FK |
| id_estado_usuario | SMALLINT FK |
| nombre | VARCHAR(80) |
| apellido_paterno | VARCHAR(80) |
| apellido_materno | VARCHAR(80) |
| correo | VARCHAR(150) |
| password_hash | VARCHAR(255) |
| telefono | CHAR(10) |
| fecha_registro | TIMESTAMP |
| fecha_actualizacion | TIMESTAMP |

### Registros de ejemplo

| id_usuario | id_rol | id_estado_usuario | nombre | apellido_paterno | apellido_materno | correo | password_hash | telefono | fecha_registro | fecha_actualizacion |
|---:|---:|---:|---|---|---|---|---|---|---|---|
| 1 | 1 | 1 | Emanuel | Villanueva | García | emanuel@gmail.com | hash_ejemplo_1 | 5512345678 | 2026-05-01 10:00:00 | 2026-05-01 10:00:00 |
| 2 | 2 | 1 | Luisa | Pérez | Ramírez | luisa@gmail.com | hash_ejemplo_2 | 5598765432 | 2026-05-01 11:00:00 | 2026-05-01 11:00:00 |

---

## 4. Nivel_Curso

Representa el nivel de dificultad de un curso.

**Justificación:**  
Se crea para normalizar los niveles de dificultad, como principiante, intermedio o avanzado. Esto evita escribir el nivel como texto repetido en cada curso.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_nivel_curso | SMALLINT PK |
| nombre_nivel | VARCHAR(30) |

### Registros de ejemplo

| id_nivel_curso | nombre_nivel |
|---:|---|
| 1 | principiante |
| 2 | intermedio |

---

## 5. Estado_Curso

Representa el estado de publicación de un curso.

**Justificación:**  
Se crea para controlar si un curso está en borrador, publicado o despublicado. Esto ayuda a definir qué cursos aparecen en el catálogo público.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_curso | SMALLINT PK |
| nombre_estado_curso | VARCHAR(30) |

### Registros de ejemplo

| id_estado_curso | nombre_estado_curso |
|---:|---|
| 1 | borrador |
| 2 | publicado |

---

## 6. Curso

Representa los cursos creados dentro de la plataforma.

**Justificación:**  
Se crea porque EduTech necesita almacenar la información de cada curso: título, descripción, precio, nivel, estado e instructor propietario. Esta entidad permite mostrar cursos en el catálogo y en el detalle del curso.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_curso | BIGINT PK |
| id_instructor | BIGINT FK |
| id_nivel_curso | SMALLINT FK |
| id_estado_curso | SMALLINT FK |
| titulo | VARCHAR(150) |
| resumen | VARCHAR(255) |
| descripcion | TEXT |
| imagen_portada | VARCHAR(255) |
| precio_mxn | NUMERIC(10,2) |
| fecha_creacion | TIMESTAMP |
| fecha_actualizacion | TIMESTAMP |

### Registros de ejemplo

| id_curso | id_instructor | id_nivel_curso | id_estado_curso | titulo | resumen | descripcion | imagen_portada | precio_mxn | fecha_creacion | fecha_actualizacion |
|---:|---:|---:|---:|---|---|---|---|---:|---|---|
| 1 | 2 | 1 | 2 | Java desde cero | Aprende Java desde lo básico | Curso introductorio de Java con módulos y examen final | java.jpg | 299.00 | 2026-05-02 09:00:00 | 2026-05-02 09:00:00 |
| 2 | 2 | 2 | 1 | Bases de datos | Aprende diseño de bases de datos | Curso sobre entidades, relaciones, normalización y SQL | bd.jpg | 349.00 | 2026-05-02 10:00:00 | 2026-05-02 10:00:00 |

---

## 7. Modulo

Representa las divisiones internas de un curso.

**Justificación:**  
Se crea porque los cursos se organizan por módulos. Cada módulo pertenece a un curso y tiene un orden dentro de él.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_modulo | BIGINT PK |
| id_curso | BIGINT FK |
| titulo | VARCHAR(150) |
| numero_orden | SMALLINT |

### Registros de ejemplo

| id_modulo | id_curso | titulo | numero_orden |
|---:|---:|---|---:|
| 1 | 1 | Introducción a Java | 1 |
| 2 | 1 | Variables y tipos de datos | 2 |

---

## 8. Tipo_Video

Representa los tipos de video permitidos en una lección.

**Justificación:**  
Se crea porque las lecciones pueden usar video de YouTube, Vimeo o carga local. Al manejarlo como catálogo, se evita repetir texto en cada lección.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_tipo_video | SMALLINT PK |
| nombre_tipo_video | VARCHAR(30) |

### Registros de ejemplo

| id_tipo_video | nombre_tipo_video |
|---:|---|
| 1 | youtube |
| 2 | vimeo |

---

## 9. Estado_Leccion

Representa el estado de una lección.

**Justificación:**  
Se crea para indicar si una lección está en borrador, activa o inactiva. Esto permite controlar qué contenido puede ver el alumno.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_leccion | SMALLINT PK |
| nombre_estado_leccion | VARCHAR(30) |

### Registros de ejemplo

| id_estado_leccion | nombre_estado_leccion |
|---:|---|
| 1 | borrador |
| 2 | activa |

---

## 10. Leccion

Representa una lección perteneciente a un módulo.

**Justificación:**  
Se crea porque cada módulo contiene lecciones. La lección almacena el contenido principal que consume el alumno: título, texto descriptivo, video y orden.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_leccion | BIGINT PK |
| id_modulo | BIGINT FK |
| id_tipo_video | SMALLINT FK |
| id_estado_leccion | SMALLINT FK |
| titulo | VARCHAR(150) |
| numero_orden | SMALLINT |
| texto_descriptivo | TEXT |
| url_video | VARCHAR(255) |
| duracion_segundos | INTEGER |

### Registros de ejemplo

| id_leccion | id_modulo | id_tipo_video | id_estado_leccion | titulo | numero_orden | texto_descriptivo | url_video | duracion_segundos |
|---:|---:|---:|---:|---|---:|---|---|---:|
| 1 | 1 | 1 | 2 | Bienvenida al curso | 1 | Presentación general del curso | https://youtube.com/video1 | 300 |
| 2 | 1 | 1 | 2 | Instalación de herramientas | 2 | Instalación de Java y editor de código | https://youtube.com/video2 | 600 |

---

## 11. Tipo_Recurso

Representa los tipos de recursos adicionales.

**Justificación:**  
Se crea para clasificar los recursos de una lección, como PDF, enlace, archivo o repositorio.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_tipo_recurso | SMALLINT PK |
| nombre_tipo_recurso | VARCHAR(30) |

### Registros de ejemplo

| id_tipo_recurso | nombre_tipo_recurso |
|---:|---|
| 1 | pdf |
| 2 | enlace |

---

## 12. Recurso

Representa materiales adicionales que pueden asociarse a una o varias lecciones.

**Justificación:**  
Se crea para almacenar materiales adicionales como PDFs, enlaces o archivos. Se separa de `Leccion` porque un recurso puede reutilizarse en varias lecciones.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_recurso | BIGINT PK |
| id_tipo_recurso | SMALLINT FK |
| id_creador | BIGINT FK |
| titulo | VARCHAR(150) |
| descripcion | VARCHAR(255) |
| url_recurso | VARCHAR(255) |
| fecha_creacion | TIMESTAMP |

### Registros de ejemplo

| id_recurso | id_tipo_recurso | id_creador | titulo | descripcion | url_recurso | fecha_creacion |
|---:|---:|---:|---|---|---|---|
| 1 | 1 | 2 | Guía de instalación | PDF con pasos de instalación | recursos/guia-instalacion.pdf | 2026-05-02 12:00:00 |
| 2 | 2 | 2 | Documentación oficial Java | Enlace de apoyo | https://docs.oracle.com | 2026-05-02 12:30:00 |

---

## 13. Leccion_Recurso

Representa la relación entre lecciones y recursos.

**Justificación:**  
Se crea como entidad intermedia porque una lección puede tener varios recursos y un mismo recurso puede aparecer en varias lecciones.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_leccion | BIGINT PK/FK |
| id_recurso | BIGINT PK/FK |
| numero_orden | SMALLINT |

### Registros de ejemplo

| id_leccion | id_recurso | numero_orden |
|---:|---:|---:|
| 1 | 1 | 1 |
| 2 | 1 | 1 |

---

## 14. Moneda

Representa las monedas permitidas en órdenes y pagos.

**Justificación:**  
Se crea porque las pasarelas de pago manejan la moneda de forma explícita. Aunque el sistema use pesos mexicanos, conviene registrar la moneda en órdenes y pagos.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_moneda | SMALLINT PK |
| codigo_moneda | CHAR(3) |
| nombre_moneda | VARCHAR(50) |

### Registros de ejemplo

| id_moneda | codigo_moneda | nombre_moneda |
|---:|---|---|
| 1 | MXN | Peso mexicano |
| 2 | USD | Dólar estadounidense |

---

## 15. Estado_Orden

Representa los estados de una orden de compra.

**Justificación:**  
Se crea para controlar si una orden está pendiente, completada, cancelada o fallida.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_orden | SMALLINT PK |
| nombre_estado_orden | VARCHAR(30) |

### Registros de ejemplo

| id_estado_orden | nombre_estado_orden |
|---:|---|
| 1 | pendiente |
| 2 | completada |

---

## 16. Orden

Representa el pedido generado cuando un alumno inicia la compra de un curso.

**Justificación:**  
Se crea porque el sistema necesita registrar cada intento de compra. La orden guarda el alumno, curso, total, moneda y estado de la compra.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_orden | BIGINT PK |
| numero_orden | CHAR(20) |
| id_alumno | BIGINT FK |
| id_curso | BIGINT FK |
| id_moneda | SMALLINT FK |
| id_estado_orden | SMALLINT FK |
| total | NUMERIC(10,2) |
| fecha_creacion | TIMESTAMP |
| fecha_actualizacion | TIMESTAMP |

### Registros de ejemplo

| id_orden | numero_orden | id_alumno | id_curso | id_moneda | id_estado_orden | total | fecha_creacion | fecha_actualizacion |
|---:|---|---:|---:|---:|---:|---:|---|---|
| 1 | ORD-2026-000001 | 1 | 1 | 1 | 2 | 299.00 | 2026-05-03 09:00:00 | 2026-05-03 09:05:00 |
| 2 | ORD-2026-000002 | 1 | 2 | 1 | 1 | 349.00 | 2026-05-03 10:00:00 | 2026-05-03 10:00:00 |

---

## 17. Datos_Compra

Guarda los datos de contacto y facturación capturados durante la compra.

**Justificación:**  
Se crea para conservar una copia de los datos usados al momento de comprar. Estos datos pueden coincidir con los datos del usuario, pero se guardan aparte porque representan la información capturada en esa orden específica.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_orden | BIGINT PK/FK |
| nombre_contacto | VARCHAR(80) |
| apellido_paterno_contacto | VARCHAR(80) |
| apellido_materno_contacto | VARCHAR(80) |
| correo_contacto | VARCHAR(150) |
| telefono_contacto | CHAR(10) |
| direccion_linea_1 | VARCHAR(150) |
| direccion_linea_2 | VARCHAR(150) |
| ciudad | VARCHAR(80) |
| estado_federativo | VARCHAR(80) |
| codigo_postal | CHAR(5) |

### Registros de ejemplo

| id_orden | nombre_contacto | apellido_paterno_contacto | apellido_materno_contacto | correo_contacto | telefono_contacto | direccion_linea_1 | direccion_linea_2 | ciudad | estado_federativo | codigo_postal |
|---:|---|---|---|---|---|---|---|---|---|---|
| 1 | Emanuel | Villanueva | García | emanuel@gmail.com | 5512345678 | Av. Universidad 3000 | Depto 2 | CDMX | Ciudad de México | 04510 |
| 2 | Emanuel | Villanueva | García | emanuel@gmail.com | 5512345678 | Av. Universidad 3000 | Depto 2 | CDMX | Ciudad de México | 04510 |

---

## 18. Proveedor_Pago

Representa las pasarelas de pago permitidas.

**Justificación:**  
Se crea para controlar qué proveedores puede usar EduTech para procesar pagos, como PayPal o Stripe.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_proveedor_pago | SMALLINT PK |
| nombre_proveedor | VARCHAR(30) |

### Registros de ejemplo

| id_proveedor_pago | nombre_proveedor |
|---:|---|
| 1 | PayPal |
| 2 | Stripe |

---

## 19. Estado_Pago

Representa los estados posibles de un pago.

**Justificación:**  
Se crea para controlar si un pago está pendiente, aprobado, rechazado o cancelado. Esto permite decidir si el curso debe liberarse o no.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_pago | SMALLINT PK |
| nombre_estado_pago | VARCHAR(30) |

### Registros de ejemplo

| id_estado_pago | nombre_estado_pago |
|---:|---|
| 1 | pendiente |
| 2 | aprobado |

---

## 20. Pago

Representa el pago asociado a una orden.

**Justificación:**  
Se crea porque cada orden necesita registrar información del pago realizado o intentado. No guarda datos bancarios sensibles, solo datos necesarios para relacionar la orden con la pasarela externa.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_pago | BIGINT PK |
| id_orden | BIGINT FK |
| id_proveedor_pago | SMALLINT FK |
| id_estado_pago | SMALLINT FK |
| id_moneda | SMALLINT FK |
| id_pago_externo | VARCHAR(120) |
| monto | NUMERIC(10,2) |
| fecha_pago | TIMESTAMP |

### Registros de ejemplo

| id_pago | id_orden | id_proveedor_pago | id_estado_pago | id_moneda | id_pago_externo | monto | fecha_pago |
|---:|---:|---:|---:|---:|---|---:|---|
| 1 | 1 | 1 | 2 | 1 | PAYPAL-ABC123 | 299.00 | 2026-05-03 09:05:00 |
| 2 | 2 | 1 | 1 | 1 | PAYPAL-DEF456 | 349.00 | NULL |

---

## 21. Estado_Webhook

Representa el estado de procesamiento de una notificación recibida desde una pasarela de pago.

**Justificación:**  
Se crea para saber si un webhook fue recibido, procesado o falló. Esto ayuda a controlar la liberación automática del curso.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_webhook | SMALLINT PK |
| nombre_estado_webhook | VARCHAR(30) |

### Registros de ejemplo

| id_estado_webhook | nombre_estado_webhook |
|---:|---|
| 1 | recibido |
| 2 | procesado |

---

## 22. Webhook_Pago

Guarda las notificaciones recibidas desde PayPal o Stripe.

**Justificación:**  
Se crea porque la pasarela de pago notifica automáticamente el resultado del pago. Esta entidad permite registrar qué evento llegó, cuándo llegó y si fue procesado.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_webhook | BIGINT PK |
| id_pago | BIGINT FK |
| id_proveedor_pago | SMALLINT FK |
| id_estado_webhook | SMALLINT FK |
| tipo_evento | VARCHAR(100) |
| id_evento_externo | VARCHAR(150) |
| contenido_evento | JSONB |
| fecha_recibido | TIMESTAMP |

### Registros de ejemplo

| id_webhook | id_pago | id_proveedor_pago | id_estado_webhook | tipo_evento | id_evento_externo | contenido_evento | fecha_recibido |
|---:|---:|---:|---:|---|---|---|---|
| 1 | 1 | 1 | 2 | PAYMENT.CAPTURE.COMPLETED | WH-001 | {"status":"COMPLETED"} | 2026-05-03 09:05:10 |
| 2 | 2 | 1 | 1 | CHECKOUT.ORDER.CREATED | WH-002 | {"status":"CREATED"} | 2026-05-03 10:00:10 |

---

## 23. Estado_Inscripcion

Representa los estados posibles de una inscripción.

**Justificación:**  
Se crea para controlar si una inscripción está activa, completada o cancelada.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_inscripcion | SMALLINT PK |
| nombre_estado_inscripcion | VARCHAR(30) |

### Registros de ejemplo

| id_estado_inscripcion | nombre_estado_inscripcion |
|---:|---|
| 1 | activa |
| 2 | completada |

---

## 24. Inscripcion

Representa el acceso de un alumno a un curso comprado.

**Justificación:**  
Se crea porque el sistema necesita saber qué alumno tiene acceso a qué curso. La inscripción se genera cuando el pago fue aprobado.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_inscripcion | BIGINT PK |
| id_alumno | BIGINT FK |
| id_curso | BIGINT FK |
| id_orden | BIGINT FK |
| id_estado_inscripcion | SMALLINT FK |
| fecha_inscripcion | TIMESTAMP |
| fecha_finalizacion | TIMESTAMP |

### Registros de ejemplo

| id_inscripcion | id_alumno | id_curso | id_orden | id_estado_inscripcion | fecha_inscripcion | fecha_finalizacion |
|---:|---:|---:|---:|---:|---|---|
| 1 | 1 | 1 | 1 | 1 | 2026-05-03 09:06:00 | NULL |
| 2 | 1 | 2 | 2 | 1 | 2026-05-03 10:06:00 | NULL |

---

## 25. Progreso_Leccion

Registra las lecciones completadas por un alumno dentro de una inscripción.

**Justificación:**  
Se crea para guardar el avance real del alumno. Con esta entidad se puede calcular el porcentaje de avance sin guardarlo directamente.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_inscripcion | BIGINT PK/FK |
| id_leccion | BIGINT PK/FK |
| completada | BOOLEAN |
| fecha_completada | TIMESTAMP |

### Registros de ejemplo

| id_inscripcion | id_leccion | completada | fecha_completada |
|---:|---:|---|---|
| 1 | 1 | true | 2026-05-03 11:00:00 |
| 1 | 2 | false | NULL |

---

## 26. Estado_Examen

Representa los estados posibles del examen.

**Justificación:**  
Se crea para controlar si el examen está en borrador, activo o inactivo.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_examen | SMALLINT PK |
| nombre_estado_examen | VARCHAR(30) |

### Registros de ejemplo

| id_estado_examen | nombre_estado_examen |
|---:|---|
| 1 | borrador |
| 2 | activo |

---

## 27. Examen

Representa el examen final de un curso.

**Justificación:**  
Se crea porque cada curso puede tener un examen final con tiempo límite, número de intentos, calificación mínima y cantidad de preguntas.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_examen | BIGINT PK |
| id_curso | BIGINT FK |
| id_estado_examen | SMALLINT FK |
| titulo | VARCHAR(150) |
| descripcion | TEXT |
| tiempo_limite_minutos | SMALLINT |
| max_intentos | SMALLINT |
| calificacion_minima | NUMERIC(5,2) |
| cantidad_preguntas | SMALLINT |

### Registros de ejemplo

| id_examen | id_curso | id_estado_examen | titulo | descripcion | tiempo_limite_minutos | max_intentos | calificacion_minima | cantidad_preguntas |
|---:|---:|---:|---|---|---:|---:|---:|---:|
| 1 | 1 | 2 | Examen final de Java | Evaluación final del curso | 30 | 2 | 70.00 | 10 |
| 2 | 2 | 1 | Examen final de Bases de Datos | Evaluación sobre modelado y SQL | 40 | 2 | 70.00 | 15 |

---

## 28. Estado_Pregunta

Representa el estado de una pregunta.

**Justificación:**  
Se crea para controlar si una pregunta del banco está activa o inactiva.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_pregunta | SMALLINT PK |
| nombre_estado_pregunta | VARCHAR(30) |

### Registros de ejemplo

| id_estado_pregunta | nombre_estado_pregunta |
|---:|---|
| 1 | activa |
| 2 | inactiva |

---

## 29. Pregunta

Representa una pregunta del banco de preguntas.

**Justificación:**  
Se crea para almacenar las preguntas que el instructor usará en el examen final.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_pregunta | BIGINT PK |
| id_examen | BIGINT FK |
| id_estado_pregunta | SMALLINT FK |
| texto_pregunta | TEXT |

### Registros de ejemplo

| id_pregunta | id_examen | id_estado_pregunta | texto_pregunta |
|---:|---:|---:|---|
| 1 | 1 | 1 | ¿Qué es una clase en Java? |
| 2 | 1 | 1 | ¿Qué palabra reservada se usa para crear un objeto? |

---

## 30. Opcion_Respuesta

Representa las opciones de respuesta de una pregunta.

**Justificación:**  
Se crea porque cada pregunta de opción múltiple necesita varias opciones y una de ellas debe identificarse como correcta.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_opcion | BIGINT PK |
| id_pregunta | BIGINT FK |
| texto_opcion | TEXT |
| es_correcta | BOOLEAN |

### Registros de ejemplo

| id_opcion | id_pregunta | texto_opcion | es_correcta |
|---:|---:|---|---|
| 1 | 1 | Una plantilla para crear objetos | true |
| 2 | 1 | Una base de datos | false |

---

## 31. Estado_Intento

Representa el estado de un intento de examen.

**Justificación:**  
Se crea para controlar si un intento está en progreso, finalizado o invalidado.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_intento | SMALLINT PK |
| nombre_estado_intento | VARCHAR(30) |

### Registros de ejemplo

| id_estado_intento | nombre_estado_intento |
|---:|---|
| 1 | en_progreso |
| 2 | finalizado |

---

## 32. Intento_Examen

Representa cada intento realizado por un alumno al presentar un examen.

**Justificación:**  
Se crea para guardar cada presentación del examen. Permite controlar intentos, calificación, fecha de inicio, fecha de finalización y aprobación.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_intento | BIGINT PK |
| id_examen | BIGINT FK |
| id_inscripcion | BIGINT FK |
| id_estado_intento | SMALLINT FK |
| numero_intento | SMALLINT |
| fecha_inicio | TIMESTAMP |
| fecha_fin | TIMESTAMP |
| calificacion | NUMERIC(5,2) |
| aprobado | BOOLEAN |

### Registros de ejemplo

| id_intento | id_examen | id_inscripcion | id_estado_intento | numero_intento | fecha_inicio | fecha_fin | calificacion | aprobado |
|---:|---:|---:|---:|---:|---|---|---:|---|
| 1 | 1 | 1 | 2 | 1 | 2026-05-04 12:00:00 | 2026-05-04 12:25:00 | 80.00 | true |
| 2 | 1 | 1 | 1 | 2 | 2026-05-05 12:00:00 | NULL | NULL | NULL |

---

## 33. Pregunta_Intento

Guarda las preguntas asignadas a un intento de examen.

**Justificación:**  
Se crea porque el examen usa preguntas aleatorias. Esta entidad conserva qué preguntas le tocaron a cada alumno en cada intento.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_intento | BIGINT PK/FK |
| id_pregunta | BIGINT PK/FK |
| numero_orden | SMALLINT |

### Registros de ejemplo

| id_intento | id_pregunta | numero_orden |
|---:|---:|---:|
| 1 | 1 | 1 |
| 1 | 2 | 2 |

---

## 34. Respuesta_Alumno

Guarda la respuesta seleccionada por el alumno.

**Justificación:**  
Se crea para guardar qué respondió el alumno en cada pregunta del intento. También se guarda si fue correcta para conservar el resultado histórico, aunque después el instructor edite la pregunta.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_intento | BIGINT PK/FK |
| id_pregunta | BIGINT PK/FK |
| id_opcion | BIGINT FK |
| es_correcta | BOOLEAN |

### Registros de ejemplo

| id_intento | id_pregunta | id_opcion | es_correcta |
|---:|---:|---:|---|
| 1 | 1 | 1 | true |
| 1 | 2 | 2 | false |

---

## 35. Certificado

Representa el certificado emitido al completar un curso.

**Justificación:**  
Se crea para registrar el certificado que obtiene el alumno cuando completa todas las lecciones y aprueba el examen final.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_certificado | BIGINT PK |
| id_inscripcion | BIGINT FK |
| codigo_certificado | CHAR(20) |
| fecha_emision | TIMESTAMP |
| url_certificado | VARCHAR(255) |

### Registros de ejemplo

| id_certificado | id_inscripcion | codigo_certificado | fecha_emision | url_certificado |
|---:|---:|---|---|---|
| 1 | 1 | EDU-2026-000001 | 2026-05-04 13:00:00 | certificados/EDU-2026-000001.pdf |
| 2 | 2 | EDU-2026-000002 | 2026-05-06 13:00:00 | certificados/EDU-2026-000002.pdf |

---

## 36. Estado_Mensaje

Representa el estado de un mensaje de contacto.

**Justificación:**  
Se crea para controlar si un mensaje enviado desde contacto está nuevo, leído o respondido.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_mensaje | SMALLINT PK |
| nombre_estado_mensaje | VARCHAR(30) |

### Registros de ejemplo

| id_estado_mensaje | nombre_estado_mensaje |
|---:|---|
| 1 | nuevo |
| 2 | leido |

---

## 37. Mensaje_Contacto

Representa los mensajes enviados desde el formulario de contacto.

**Justificación:**  
Se crea solo si el sistema guardará los mensajes enviados desde la pantalla de contacto. Si el formulario únicamente envía un correo y no almacena mensajes, esta entidad puede omitirse.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_mensaje | BIGINT PK |
| id_estado_mensaje | SMALLINT FK |
| nombre | VARCHAR(80) |
| correo | VARCHAR(150) |
| asunto | VARCHAR(120) |
| mensaje | TEXT |
| fecha_envio | TIMESTAMP |

### Registros de ejemplo

| id_mensaje | id_estado_mensaje | nombre | correo | asunto | mensaje | fecha_envio |
|---:|---:|---|---|---|---|---|
| 1 | 1 | Diego | diego@gmail.com | Duda sobre curso | Quiero saber cuándo inicia el curso de Java | 2026-05-03 15:00:00 |
| 2 | 2 | Valeria | valeria@gmail.com | Problema de acceso | No puedo entrar a mi curso comprado | 2026-05-03 16:00:00 |

---

# Resumen de entidades

## Entidades principales

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
- Pregunta_Intento
- Respuesta_Alumno
- Certificado

## Entidades de catálogo

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

## Entidades intermedias

- Leccion_Recurso
- Pregunta_Intento
- Respuesta_Alumno

## Entidad opcional

- Mensaje_Contacto

---

# Entidades que no se consideran parte de la base de datos

No se consideran entidades de base de datos:

- Pantalla
- Botón
- Sección
- Dashboard
- Aula virtual
- Checkout
- Navbar
- Footer
- Menú

Estos elementos pertenecen al diseño de la interfaz, no a los datos que el sistema necesita almacenar.

---

# Conclusión

La identificación de entidades y atributos permite definir qué información necesita almacenar EduTech para funcionar correctamente.

El modelo propuesto considera usuarios, roles, cursos, módulos, lecciones, recursos, compras, pagos, inscripciones, progreso, exámenes, respuestas y certificados. También se incluyen catálogos para manejar datos controlados como estados, niveles, tipos, monedas y proveedores.

Esta estructura ayuda a mantener una base de datos ordenada, normalizada y coherente con los requerimientos principales del sistema.

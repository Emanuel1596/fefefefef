# Identificación de entidades y atributos de base de datos

## Proyecto: EduTech - Plataforma Web de Cursos

## Objetivo

Este documento presenta la identificación de entidades y atributos principales para la base de datos del sistema EduTech.

EduTech permite que los alumnos se registren, compren cursos, accedan a módulos y lecciones, registren su progreso, presenten un examen final y obtengan certificados. También contempla usuarios con rol de instructor y administrador.

---

## Criterios usados para identificar entidades

Para identificar las entidades se usaron estos criterios:

1. Una entidad representa información importante que el sistema necesita guardar.
2. Los atributos deben ser claros y atómicos.
3. Los datos repetidos y controlados se manejan mediante catálogos.
4. Si un dato solo tiene dos estados, como activo/inactivo, se puede manejar con `BOOLEAN`.
5. Si un dato tiene varios estados de negocio, como pendiente, aprobado, rechazado o cancelado, se maneja con catálogo.
6. Los botones, pantallas, secciones, menús o dashboards no son entidades de base de datos.
7. Los datos calculables, como el porcentaje de avance, no se guardan como atributo fijo si pueden obtenerse mediante consulta.
8. Las contraseñas no se guardan en texto plano; se guarda un `password_hash`.

---

## Criterio para tipos de dato

Los tipos de dato se plantean pensando en PostgreSQL.

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

## ¿Por qué TIMESTAMP y no DATE?

Se usa `TIMESTAMP` cuando importa guardar fecha y hora.

En EduTech casi todos los eventos necesitan hora exacta:

- fecha de registro;
- fecha de creación del curso;
- fecha de compra;
- fecha de pago;
- fecha de inscripción;
- fecha de inicio del examen;
- fecha de finalización del examen;
- fecha de emisión del certificado.

Ejemplo:

```text
fecha_pago = 2026-05-03 09:05:00
```

No sería suficiente guardar solo:

```text
fecha_pago = 2026-05-03
```

porque se perdería la hora exacta del evento.

---

## Nota sobre catálogos

Un catálogo es una tabla que guarda valores controlados y definidos previamente.

Por ejemplo, en lugar de escribir muchas veces el estado de un pago como texto, se crea una entidad `Estado_Pago` y en la tabla `Pago` solo se guarda el identificador correspondiente.

Esto evita errores como:

```text
aprobado
Aprobado
APROBADO
aprovado
```

Regla general:

```text
Si solo hay dos opciones, puede ser BOOLEAN.
Si hay varios estados con significado de negocio, conviene usar catálogo.
```

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

### Registros de catálogo

| id_rol | nombre_rol |
|---:|---|
| 1 | Alumno |
| 2 | Instructor |
| 3 | Administrador |

---

## 2. Usuario

Representa a las personas que utilizan el sistema.

**Justificación:**  
Se crea porque el sistema necesita almacenar los datos de alumnos, instructores y administradores. Esta entidad permite registrar usuarios, iniciar sesión y controlar su rol dentro del sistema.

Se usa `esta_activo` como `BOOLEAN` en lugar de crear una tabla `Estado_Usuario`, porque para este proyecto el usuario solo necesita estar activo o inactivo.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_usuario | BIGINT PK |
| id_rol | SMALLINT FK |
| nombre | VARCHAR(80) |
| apellido_paterno | VARCHAR(80) |
| apellido_materno | VARCHAR(80) |
| correo | VARCHAR(150) |
| password_hash | VARCHAR(255) |
| telefono | CHAR(10) |
| esta_activo | BOOLEAN |
| fecha_registro | TIMESTAMP |
| fecha_actualizacion | TIMESTAMP |

### Registros de ejemplo

| id_usuario | id_rol | nombre | apellido_paterno | apellido_materno | correo | password_hash | telefono | esta_activo | fecha_registro | fecha_actualizacion |
|---:|---:|---|---|---|---|---|---|---|---|---|
| 1 | 1 | Emanuel | Villanueva | García | emanuel@gmail.com | hash_ejemplo_1 | 5512345678 | true | 2026-05-01 10:00:00 | 2026-05-01 10:00:00 |
| 2 | 2 | Luisa | Pérez | Ramírez | luisa@gmail.com | hash_ejemplo_2 | 5598765432 | true | 2026-05-01 11:00:00 | 2026-05-01 11:00:00 |

---

## 3. Nivel_Curso

Representa el nivel de dificultad de un curso.

**Justificación:**  
Se crea para normalizar los niveles de dificultad. Es mejor usar catálogo porque el nivel se repite en muchos cursos y debe tener valores controlados.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_nivel_curso | SMALLINT PK |
| nombre_nivel | VARCHAR(30) |

### Registros de catálogo

| id_nivel_curso | nombre_nivel |
|---:|---|
| 1 | principiante |
| 2 | intermedio |
| 3 | avanzado |

---

## 4. Estado_Curso

Representa el estado de publicación de un curso.

**Justificación:**  
Se crea como catálogo porque el curso puede tener más de dos estados. No basta con un `BOOLEAN`, ya que no solo existe publicado/no publicado.

### Estados propuestos

| Estado | Explicación |
|---|---|
| borrador | El instructor todavía está creando el curso. |
| pendiente_revision | El curso ya fue terminado por el instructor, pero falta revisión del administrador. |
| publicado | El curso aparece en el catálogo y puede comprarse. |
| no_publicado | El curso no aparece en el catálogo. |

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_curso | SMALLINT PK |
| nombre_estado_curso | VARCHAR(30) |

### Registros de catálogo

| id_estado_curso | nombre_estado_curso |
|---:|---|
| 1 | borrador |
| 2 | pendiente_revision |
| 3 | publicado |
| 4 | no_publicado |

---

## 5. Curso

Representa los cursos creados dentro de la plataforma.

**Justificación:**  
Se crea porque EduTech necesita almacenar la información de cada curso: instructor, nivel, estado, título, descripción, imagen y precio.

No se usa `resumen` porque no está como requerimiento obligatorio. La descripción completa puede recortarse visualmente en el frontend cuando se muestre en el catálogo.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_curso | BIGINT PK |
| id_instructor | BIGINT FK |
| id_nivel_curso | SMALLINT FK |
| id_estado_curso | SMALLINT FK |
| titulo | VARCHAR(150) |
| descripcion | TEXT |
| imagen_portada | VARCHAR(255) |
| precio_mxn | NUMERIC(10,2) |
| fecha_creacion | TIMESTAMP |
| fecha_actualizacion | TIMESTAMP |

### Registros de ejemplo

| id_curso | id_instructor | id_nivel_curso | id_estado_curso | titulo | descripcion | imagen_portada | precio_mxn | fecha_creacion | fecha_actualizacion |
|---:|---:|---:|---:|---|---|---|---:|---|---|
| 1 | 2 | 1 | 3 | Java desde cero | Curso introductorio de Java con módulos, lecciones y examen final. | java.jpg | 299.00 | 2026-05-02 09:00:00 | 2026-05-02 09:00:00 |
| 2 | 2 | 2 | 1 | Bases de datos | Curso sobre entidades, relaciones, normalización y SQL. | bd.jpg | 349.00 | 2026-05-02 10:00:00 | 2026-05-02 10:00:00 |

---

## 6. Modulo

Representa las divisiones internas de un curso.

**Justificación:**  
Se crea porque los cursos se organizan por módulos. Cada módulo pertenece a un curso y tiene un orden dentro de él.

No se agrega descripción al módulo porque el requerimiento indica que el módulo tiene nombre y número. La descripción detallada vive en las lecciones.

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

## 7. Tipo_Video

Representa el tipo de video que tendrá una lección.

**Justificación:**  
Se crea porque el requerimiento no funcional indica que el reproductor debe soportar videos externos como YouTube/Vimeo o carga local optimizada.

El tipo de video ayuda al sistema a saber cómo mostrar el video.

### Ejemplo visual

En el formulario del instructor podría verse así:

```text
Crear lección

Título:
[ Introducción a Java ]

Tipo de video:
( ) YouTube
( ) Vimeo
( ) Video local

URL o ruta del video:
[ https://youtube.com/watch?v=abc123 ]

[Guardar lección]
```

Si el instructor elige `youtube`, el sistema sabe que debe mostrarlo como video embebido:

```html
<iframe src="https://www.youtube.com/embed/abc123"></iframe>
```

Si el instructor elige `vimeo`, el sistema usa un embed de Vimeo:

```html
<iframe src="https://player.vimeo.com/video/123456"></iframe>
```

Si el instructor elige `local`, el sistema usa una etiqueta de video:

```html
<video controls>
  <source src="/videos/leccion1.mp4" type="video/mp4">
</video>
```

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_tipo_video | SMALLINT PK |
| nombre_tipo_video | VARCHAR(30) |

### Registros de catálogo

| id_tipo_video | nombre_tipo_video |
|---:|---|
| 1 | youtube |
| 2 | vimeo |
| 3 | local |

---

## 8. Leccion

Representa una lección perteneciente a un módulo.

**Justificación:**  
Se crea porque cada módulo contiene lecciones. La lección almacena el contenido principal que consume el alumno: título, texto descriptivo, video y orden.

Se usa `esta_activa` como `BOOLEAN` en lugar de crear `Estado_Leccion`, porque para este proyecto basta con saber si la lección está disponible o no.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_leccion | BIGINT PK |
| id_modulo | BIGINT FK |
| id_tipo_video | SMALLINT FK |
| titulo | VARCHAR(150) |
| numero_orden | SMALLINT |
| texto_descriptivo | TEXT |
| url_video | VARCHAR(255) |
| duracion_segundos | INTEGER |
| esta_activa | BOOLEAN |

### Registros de ejemplo

| id_leccion | id_modulo | id_tipo_video | titulo | numero_orden | texto_descriptivo | url_video | duracion_segundos | esta_activa |
|---:|---:|---:|---|---:|---|---|---:|---|
| 1 | 1 | 1 | Bienvenida al curso | 1 | Presentación general del curso. | https://youtube.com/watch?v=abc123 | 300 | true |
| 2 | 1 | 1 | Instalación de herramientas | 2 | Instalación de Java y editor de código. | https://youtube.com/watch?v=def456 | 600 | true |

---

## 9. Tipo_Recurso

Representa los tipos de recursos adicionales que puede tener una lección.

**Justificación:**  
Se crea porque los recursos no siempre se muestran igual. Un PDF puede descargarse, un enlace puede abrirse en otra pestaña, un archivo puede descargarse y un repositorio puede abrir GitHub o GitLab.

### Ejemplo visual

En la vista de la lección, el sistema podría mostrar los recursos de forma diferente según su tipo:

```text
Recursos adicionales

📄 Guía de instalación
Tipo: PDF
Botón: Descargar PDF

🔗 Documentación oficial Java
Tipo: Enlace
Botón: Abrir enlace

📦 Proyecto base
Tipo: Archivo
Botón: Descargar archivo

💻 Código fuente
Tipo: Repositorio
Botón: Ver repositorio
```

Sin `Tipo_Recurso`, el sistema solo tendría una URL y no sabría claramente si debe mostrar botón de descarga, icono de PDF, enlace externo o acceso a repositorio.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_tipo_recurso | SMALLINT PK |
| nombre_tipo_recurso | VARCHAR(30) |

### Registros de catálogo

| id_tipo_recurso | nombre_tipo_recurso |
|---:|---|
| 1 | pdf |
| 2 | enlace |
| 3 | archivo |
| 4 | repositorio |

---

## 10. Recurso

Representa materiales adicionales que pueden asociarse a una o varias lecciones.

**Justificación:**  
Se crea para almacenar materiales adicionales como PDFs, enlaces, archivos o repositorios. Se separa de `Leccion` porque una lección puede tener varios recursos y un mismo recurso puede reutilizarse en más de una lección.

Se elimina `id_creador` porque no está explícitamente en los requerimientos y puede deducirse desde el curso/instructor si fuera necesario.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_recurso | BIGINT PK |
| id_tipo_recurso | SMALLINT FK |
| titulo | VARCHAR(150) |
| descripcion | VARCHAR(255) |
| url_recurso | VARCHAR(255) |
| fecha_creacion | TIMESTAMP |

### Registros de ejemplo

| id_recurso | id_tipo_recurso | titulo | descripcion | url_recurso | fecha_creacion |
|---:|---:|---|---|---|---|
| 1 | 1 | Guía de instalación | PDF con pasos de instalación. | recursos/guia-instalacion.pdf | 2026-05-02 12:00:00 |
| 2 | 2 | Documentación oficial Java | Enlace de apoyo. | https://docs.oracle.com | 2026-05-02 12:30:00 |

---

## 11. Leccion_Recurso

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

## 12. Moneda

Representa las monedas permitidas en órdenes y pagos.

**Justificación:**  
Se crea porque las pasarelas de pago manejan la moneda de forma explícita. Aunque el sistema use pesos mexicanos, conviene registrar la moneda en órdenes y pagos.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_moneda | SMALLINT PK |
| codigo_moneda | CHAR(3) |
| nombre_moneda | VARCHAR(50) |

### Registros de catálogo

| id_moneda | codigo_moneda | nombre_moneda |
|---:|---|---|
| 1 | MXN | Peso mexicano |
| 2 | USD | Dólar estadounidense |

---

## 13. Estado_Orden

Representa los estados de una orden de compra.

**Justificación:**  
Se crea como catálogo porque una orden puede tener varios estados de negocio.

### Estados propuestos

| Estado | Explicación |
|---|---|
| pendiente | La orden se creó, pero el pago todavía no se confirma. |
| completada | El pago fue aprobado y el curso puede liberarse. |
| cancelada | El alumno canceló el proceso o abandonó la compra. |
| fallida | Hubo error o rechazo en el proceso de pago. |

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_orden | SMALLINT PK |
| nombre_estado_orden | VARCHAR(30) |

### Registros de catálogo

| id_estado_orden | nombre_estado_orden |
|---:|---|
| 1 | pendiente |
| 2 | completada |
| 3 | cancelada |
| 4 | fallida |

---

## 14. Orden

Representa el pedido generado cuando un alumno inicia la compra de un curso.

**Justificación:**  
Se crea porque el sistema necesita registrar cada intento de compra. La orden guarda el alumno, curso, total, moneda y estado de la compra.

Se conservan `id_orden` y `numero_orden` porque no cumplen la misma función:

- `id_orden`: identificador interno de la base de datos.
- `numero_orden`: número visible para el usuario en comprobantes, historial de pedidos y correos.

El `numero_orden` no se captura a mano. Lo genera el backend cuando se crea la orden.

Ejemplo de generación:

```text
id_orden = 15
fecha = 2026
numero_orden = ORD-2026-000015
```

Si después cambia el estado de la orden, el `numero_orden` no cambia. Lo que cambia es el estado.

Ejemplo:

```text
Orden #ORD-2026-000015
Estado inicial: pendiente

Después del webhook:
Orden #ORD-2026-000015
Estado actualizado: completada
```

En el comprobante se sigue mostrando el mismo número de orden, pero con estado actualizado.

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

## 15. Diferencia entre precio_mxn y total

`precio_mxn` y `total` no son lo mismo.

| Campo | Significado |
|---|---|
| Curso.precio_mxn | Precio actual del curso. |
| Orden.total | Total cobrado en una orden específica. |

Ejemplo:

```text
Hoy el curso cuesta:
Curso.precio_mxn = 299.00

Emanuel compra hoy:
Orden.total = 299.00

Después el instructor cambia el precio:
Curso.precio_mxn = 399.00

La orden de Emanuel debe seguir diciendo:
Orden.total = 299.00
```

Por eso se guardan ambos. `Curso.precio_mxn` puede cambiar, pero `Orden.total` conserva la foto del precio al momento de la compra.

---

## 16. Estado_Federativo

Representa los estados de México usados en los datos de compra.

**Justificación:**  
Se crea porque el sistema será usado en México y el estado federativo es un dato repetido. Normalizarlo evita escribir muchas veces valores como “CDMX”, “Ciudad de México” o “Cdmx”.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_federativo | SMALLINT PK |
| nombre_estado_federativo | VARCHAR(80) |

### Registros de catálogo

| id_estado_federativo | nombre_estado_federativo |
|---:|---|
| 1 | Ciudad de México |
| 2 | Estado de México |
| 3 | Jalisco |
| 4 | Nuevo León |

---

## 17. Ciudad

Representa las ciudades disponibles para los datos de compra.

**Justificación:**  
Se crea porque la ciudad también puede repetirse en muchos registros de compra. Además, cada ciudad pertenece a un estado federativo.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_ciudad | BIGINT PK |
| id_estado_federativo | SMALLINT FK |
| nombre_ciudad | VARCHAR(80) |

### Registros de ejemplo

| id_ciudad | id_estado_federativo | nombre_ciudad |
|---:|---:|---|
| 1 | 1 | Ciudad de México |
| 2 | 2 | Toluca |

---

## 18. Datos_Compra

Guarda los datos de contacto y facturación capturados durante la compra.

**Justificación:**  
Se crea para conservar una copia de los datos usados al momento de comprar. Estos datos pueden coincidir con los datos del usuario, pero se guardan aparte porque representan la información capturada en esa orden específica.

Se usa una sola `direccion` porque para este proyecto no se requiere separar dirección en línea 1 y línea 2.

Se normalizan ciudad y estado federativo mediante catálogos.

El código postal se deja como atributo porque convertirlo en catálogo sería demasiado para el alcance del proyecto.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_orden | BIGINT PK/FK |
| nombre_contacto | VARCHAR(80) |
| apellido_paterno_contacto | VARCHAR(80) |
| apellido_materno_contacto | VARCHAR(80) |
| correo_contacto | VARCHAR(150) |
| telefono_contacto | CHAR(10) |
| direccion | VARCHAR(180) |
| id_ciudad | BIGINT FK |
| codigo_postal | CHAR(5) |

### Registros de ejemplo

| id_orden | nombre_contacto | apellido_paterno_contacto | apellido_materno_contacto | correo_contacto | telefono_contacto | direccion | id_ciudad | codigo_postal |
|---:|---|---|---|---|---|---|---:|---|
| 1 | Emanuel | Villanueva | García | emanuel@gmail.com | 5512345678 | Av. Universidad 3000 Depto 2 | 1 | 04510 |
| 2 | Emanuel | Villanueva | García | emanuel@gmail.com | 5512345678 | Av. Universidad 3000 Depto 2 | 1 | 04510 |

---

## 19. Proveedor_Pago

Representa las pasarelas de pago permitidas.

**Justificación:**  
Se crea para controlar qué proveedores puede usar EduTech para procesar pagos, como PayPal o Stripe.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_proveedor_pago | SMALLINT PK |
| nombre_proveedor | VARCHAR(30) |

### Registros de catálogo

| id_proveedor_pago | nombre_proveedor |
|---:|---|
| 1 | PayPal |
| 2 | Stripe |

---

## 20. Estado_Pago

Representa los estados posibles de un pago.

**Justificación:**  
Se crea como catálogo porque un pago puede tener varios estados de negocio. El estado del pago determina si el curso se libera o no.

### Estados propuestos

| Estado | Explicación |
|---|---|
| pendiente | El pago fue iniciado, pero todavía no se confirma. |
| aprobado | La pasarela confirmó que el pago fue exitoso. |
| rechazado | La pasarela rechazó el pago. |
| cancelado | El usuario canceló el proceso de pago. |

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_pago | SMALLINT PK |
| nombre_estado_pago | VARCHAR(30) |

### Registros de catálogo

| id_estado_pago | nombre_estado_pago |
|---:|---|
| 1 | pendiente |
| 2 | aprobado |
| 3 | rechazado |
| 4 | cancelado |

---

## 21. Pago

Representa el pago asociado a una orden.

**Justificación:**  
Se crea porque cada orden necesita registrar información del pago realizado o intentado. No guarda datos bancarios sensibles, solo datos necesarios para relacionar la orden con la pasarela externa.

Se usa `monto_pagado` para distinguirlo de `Orden.total`.

| Campo | Significado |
|---|---|
| Orden.total | Total que EduTech espera cobrar. |
| Pago.monto_pagado | Monto confirmado por la pasarela. |

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_pago | BIGINT PK |
| id_orden | BIGINT FK |
| id_proveedor_pago | SMALLINT FK |
| id_estado_pago | SMALLINT FK |
| id_moneda | SMALLINT FK |
| id_pago_externo | VARCHAR(120) |
| monto_pagado | NUMERIC(10,2) |
| fecha_pago | TIMESTAMP |

### Registros de ejemplo

| id_pago | id_orden | id_proveedor_pago | id_estado_pago | id_moneda | id_pago_externo | monto_pagado | fecha_pago |
|---:|---:|---:|---:|---:|---|---:|---|
| 1 | 1 | 1 | 2 | 1 | PAYPAL-ABC123 | 299.00 | 2026-05-03 09:05:00 |
| 2 | 2 | 1 | 1 | 1 | PAYPAL-DEF456 | 349.00 | NULL |

---

## 22. Estado_Webhook

Representa el estado de procesamiento de una notificación recibida desde una pasarela de pago.

**Justificación:**  
Se crea para saber si un webhook fue recibido, procesado o falló. Esto ayuda a controlar la liberación automática del curso.

### Estados propuestos

| Estado | Explicación |
|---|---|
| recibido | EduTech recibió el evento, pero aún no lo procesa completamente. |
| procesado | El evento fue validado y aplicado correctamente. |
| fallido | El evento no pudo procesarse por error o inconsistencia. |

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_webhook | SMALLINT PK |
| nombre_estado_webhook | VARCHAR(30) |

### Registros de catálogo

| id_estado_webhook | nombre_estado_webhook |
|---:|---|
| 1 | recibido |
| 2 | procesado |
| 3 | fallido |

---

## 23. Webhook_Pago

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

## 24. Estado_Inscripcion

Representa los estados posibles de una inscripción.

**Justificación:**  
Se crea como catálogo porque la inscripción puede tener varios estados de negocio.

### Estados propuestos

| Estado | Explicación |
|---|---|
| activa | El alumno tiene acceso al curso. |
| completada | El alumno terminó el curso. |
| cancelada | La inscripción fue cancelada o revocada. |

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_inscripcion | SMALLINT PK |
| nombre_estado_inscripcion | VARCHAR(30) |

### Registros de catálogo

| id_estado_inscripcion | nombre_estado_inscripcion |
|---:|---|
| 1 | activa |
| 2 | completada |
| 3 | cancelada |

---

## 25. Inscripcion

Representa el acceso de un alumno a un curso comprado.

**Justificación:**  
Se crea porque el sistema necesita saber qué alumno tiene acceso a qué curso. La inscripción se genera cuando el pago fue aprobado.

La fecha de finalización se guarda aquí porque la finalización pertenece a la relación entre alumno y curso.

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

## 26. Progreso_Leccion

Registra las lecciones completadas por un alumno dentro de una inscripción.

**Justificación:**  
Se crea para guardar el avance real del alumno. Con esta entidad se puede calcular el porcentaje de avance sin guardarlo directamente.

Ejemplo de cálculo:

```text
lecciones_completadas / total_lecciones * 100

8 / 20 * 100 = 40%
```

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

## 27. Estado_Examen

Representa los estados posibles del examen.

**Justificación:**  
Se crea como catálogo porque el examen puede tener varios estados.

### Estados propuestos

| Estado | Explicación |
|---|---|
| borrador | El examen todavía se está configurando. |
| activo | El examen puede ser presentado por los alumnos. |
| inactivo | El examen ya no está disponible. |

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_examen | SMALLINT PK |
| nombre_estado_examen | VARCHAR(30) |

### Registros de catálogo

| id_estado_examen | nombre_estado_examen |
|---:|---|
| 1 | borrador |
| 2 | activo |
| 3 | inactivo |

---

## 28. Examen

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
| 1 | 1 | 2 | Examen final de Java | Evaluación final del curso. | 30 | 2 | 70.00 | 10 |
| 2 | 2 | 1 | Examen final de Bases de Datos | Evaluación sobre modelado y SQL. | 40 | 2 | 70.00 | 15 |

---

## 29. Pregunta

Representa una pregunta del banco de preguntas.

**Justificación:**  
Se crea para almacenar las preguntas que el instructor usará en el examen final.

Se usa `esta_activa` como `BOOLEAN` en lugar de crear `Estado_Pregunta`, porque para este proyecto basta con saber si la pregunta está activa o no.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_pregunta | BIGINT PK |
| id_examen | BIGINT FK |
| texto_pregunta | TEXT |
| esta_activa | BOOLEAN |

### Registros de ejemplo

| id_pregunta | id_examen | texto_pregunta | esta_activa |
|---:|---:|---|---|
| 1 | 1 | ¿Qué es una clase en Java? | true |
| 2 | 1 | ¿Qué palabra reservada se usa para crear un objeto? | true |

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
Se crea como catálogo porque un intento puede tener más de dos estados importantes. No basta con un `BOOLEAN`.

### Estados propuestos

| Estado | Explicación |
|---|---|
| en_progreso | El alumno inició el examen, pero todavía no lo envía. |
| finalizado | El alumno envió respuestas y ya tiene calificación. |
| invalidado | El sistema anuló el intento por una regla, error o salida indebida. |
| abandonado | El alumno salió o perdió conexión antes de terminar. |

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_intento | SMALLINT PK |
| nombre_estado_intento | VARCHAR(30) |

### Registros de catálogo

| id_estado_intento | nombre_estado_intento |
|---:|---|
| 1 | en_progreso |
| 2 | finalizado |
| 3 | invalidado |
| 4 | abandonado |

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

Se conservan `id_certificado` y `codigo_certificado` porque no cumplen la misma función:

- `id_certificado`: identificador interno de la base de datos.
- `codigo_certificado`: código visible para el alumno, útil para mostrar, descargar o validar el certificado.

El `codigo_certificado` no se captura a mano. Lo genera el backend cuando se emite el certificado.

Ejemplo de generación:

```text
id_certificado = 8
año = 2026
codigo_certificado = EDU-2026-000008
```

El alumno no necesita ver `id_certificado`. En el certificado se muestra algo como:

```text
Certificado: EDU-2026-000008
```

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

# Entidades opcionales

## Mensaje_Contacto

Esta entidad solo se usaría si EduTech guardará mensajes enviados desde la pantalla de contacto.

Si el formulario de contacto solo envía un correo y no guarda mensajes en la base de datos, esta entidad puede omitirse.

---

# Resumen de entidades finales

## Entidades principales

- Usuario
- Curso
- Modulo
- Leccion
- Recurso
- Leccion_Recurso
- Orden
- Datos_Compra
- Pago
- Webhook_Pago
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

- Rol
- Nivel_Curso
- Estado_Curso
- Tipo_Video
- Tipo_Recurso
- Moneda
- Estado_Orden
- Estado_Federativo
- Ciudad
- Proveedor_Pago
- Estado_Pago
- Estado_Webhook
- Estado_Inscripcion
- Estado_Examen
- Estado_Intento

## Entidades que se quitaron o simplificaron

| Entidad o atributo anterior | Decisión |
|---|---|
| Estado_Usuario | Se reemplaza por `Usuario.esta_activo`. |
| Estado_Leccion | Se reemplaza por `Leccion.esta_activa`. |
| Estado_Pregunta | Se reemplaza por `Pregunta.esta_activa`. |
| Estado_Mensaje | Se deja opcional junto con `Mensaje_Contacto`. |
| Mensaje_Contacto | Opcional, solo si se guardan mensajes. |
| Curso.resumen | Se elimina porque no es requerimiento obligatorio. |
| Recurso.id_creador | Se elimina porque no está justificado por requerimiento. |
| Orden.numero_orden | Se conserva, pero como dato visible generado automáticamente. |
| Certificado.codigo_certificado | Se conserva, pero como código visible generado automáticamente. |

---

# Entidades que no se consideran base de datos

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

El modelo corregido considera usuarios, roles, cursos, módulos, lecciones, recursos, compras, pagos, inscripciones, progreso, exámenes, respuestas y certificados. También se normalizan los datos que realmente necesitan catálogos, como estados con varios valores, niveles, tipos, monedas, proveedores, ciudades y estados federativos.

Esta estructura mantiene la base de datos ordenada, más clara y coherente con los requerimientos del sistema.

---

# Frase para recordar

Si solo hay dos opciones, puede ser booleano.  
Si hay varios estados importantes del negocio, conviene usar catálogo.  
Si el dato se muestra al usuario como folio o código, puede existir junto al ID interno.

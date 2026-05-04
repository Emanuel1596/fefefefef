## Criterio para tipos de dato

Los tipos de dato se plantean pensando en PostgreSQL y en el tamaño real del proyecto.

| Caso | Tipo sugerido |
|---|---|
| Identificadores principales | INTEGER PK |
| Llaves foráneas | INTEGER FK |
| Texto corto | VARCHAR(n) |
| Texto largo | TEXT |
| Dinero o calificaciones | NUMERIC(p,s) |
| Fecha y hora | TIMESTAMP |
| Verdadero / falso | BOOLEAN |
| Teléfono mexicano | CHAR(10) |
| Código postal mexicano | CHAR(5) |
| Código de moneda | CHAR(3) |
| Contenido de webhook | JSONB |

Se usa `INTEGER` para los identificadores porque es suficiente para el tamaño esperado del proyecto. Un `INTEGER` permite manejar una cantidad muy grande de registros, por lo que no es necesario usar `BIGINT` en esta etapa.

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

| Campo | Valor |
|---|---|
| fecha_pago | 2026-05-03 09:05:00 |

No sería suficiente guardar solo la fecha `2026-05-03`, porque se perdería la hora exacta del evento.

---

## Nota sobre catálogos

Un catálogo es una tabla que guarda valores controlados y definidos previamente.

Por ejemplo, en lugar de escribir muchas veces el estado de un pago como texto, se crea una entidad `Estado_Pago` y en la tabla `Pago` solo se guarda el identificador correspondiente.

| id_estado_pago | nombre_estado_pago |
|---:|---|
| 1 | pendiente |
| 2 | aprobado |
| 3 | rechazado |
| 4 | cancelado |

Esto evita errores de escritura y mantiene consistencia en la base de datos.

Regla general:

- Si solo hay dos opciones, puede usarse `BOOLEAN`.
- Si hay varios estados con significado dentro del proceso, conviene usar catálogo.

---

## 2. Usuario

Representa a las personas que utilizan el sistema.

**Justificación:**  
Se crea porque el sistema necesita almacenar los datos de alumnos, instructores y administradores. Esta entidad permite registrar usuarios, iniciar sesión y controlar su rol dentro del sistema.

### Atributos

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

### Registros de ejemplo

| id_usuario | id_rol | nombre | apellido_paterno | apellido_materno | correo | password_hash | telefono | esta_activo | fecha_registro | fecha_actualizacion |
|---:|---:|---|---|---|---|---|---|---|---|---|
| 1 | 1 | Emanuel | Villanueva | García | emanuel@gmail.com | hash_ejemplo_1 | 5512345678 | true | 2026-05-01 10:00:00 | 2026-05-01 10:00:00 |
| 2 | 2 | Luisa | Pérez | Ramírez | luisa@gmail.com | hash_ejemplo_2 | 5598765432 | true | 2026-05-01 11:00:00 | 2026-05-01 11:00:00 |

---

## 5. Curso

Representa los cursos creados dentro de la plataforma.

**Justificación:**  
Se crea porque EduTech necesita almacenar la información de cada curso: instructor, nivel, estado, título, descripción, imagen y precio.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_curso | INTEGER PK |
| id_instructor | INTEGER FK |
| id_nivel_curso | INTEGER FK |
| id_estado_curso | INTEGER FK |
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

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_modulo | INTEGER PK |
| id_curso | INTEGER FK |
| titulo | VARCHAR(150) |
| numero_orden | INTEGER |

### Registros de ejemplo

| id_modulo | id_curso | titulo | numero_orden |
|---:|---:|---|---:|
| 1 | 1 | Introducción a Java | 1 |
| 2 | 1 | Variables y tipos de datos | 2 |

---

## 7. Tipo_Video

Representa el tipo de video que tendrá una lección.

**Justificación:**  
Se crea porque el reproductor de EduTech debe poder manejar diferentes formas de mostrar un video. No es lo mismo mostrar un video de YouTube, uno de Vimeo o uno cargado localmente en el servidor.

| Tipo de video | Cómo lo interpreta el sistema |
|---|---|
| youtube | El sistema muestra el video como inserción externa de YouTube. |
| vimeo | El sistema muestra el video como inserción externa de Vimeo. |
| local | El sistema reproduce un archivo guardado en el servidor. |

### Ejemplo de formulario para el instructor

| Campo | Ejemplo |
|---|---|
| Título de la lección | Introducción a Java |
| Tipo de video | YouTube |
| URL o ruta del video | https://youtube.com/watch?v=abc123 |

Si el instructor selecciona `youtube`, el sistema sabe que debe mostrar el video como contenido embebido de YouTube.

Si selecciona `vimeo`, el sistema debe usar el reproductor de Vimeo.

Si selecciona `local`, el sistema debe reproducir un archivo guardado dentro del servidor o almacenamiento del sistema.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_tipo_video | INTEGER PK |
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

### Atributos

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

### Registros de ejemplo

| id_leccion | id_modulo | id_tipo_video | titulo | numero_orden | texto_descriptivo | url_video | duracion_segundos | esta_activa |
|---:|---:|---:|---|---:|---|---|---:|---|
| 1 | 1 | 1 | Bienvenida al curso | 1 | Presentación general del curso. | https://youtube.com/watch?v=abc123 | 300 | true |
| 2 | 1 | 1 | Instalación de herramientas | 2 | Instalación de Java y editor de código. | https://youtube.com/watch?v=def456 | 600 | true |

---

## 9. Tipo_Recurso

Representa los tipos de recursos adicionales que puede tener una lección.

**Justificación:**  
Se crea porque los recursos no siempre se muestran igual. Un PDF puede descargarse, un enlace puede abrirse en otra pestaña, un archivo puede descargarse y un repositorio puede abrirse como sitio externo.

| Tipo de recurso | Cómo se muestra en la lección |
|---|---|
| pdf | Se muestra como material descargable. |
| enlace | Se muestra como enlace externo. |
| archivo | Se muestra como archivo descargable. |
| repositorio | Se muestra como enlace a repositorio de código. |

### Ejemplo de visualización para el alumno

| Recurso | Tipo | Acción mostrada |
|---|---|---|
| Guía de instalación | pdf | Descargar PDF |
| Documentación oficial Java | enlace | Abrir enlace |
| Proyecto base | archivo | Descargar archivo |
| Código fuente | repositorio | Ver repositorio |

Sin `Tipo_Recurso`, el sistema solo tendría una URL y no sabría claramente cómo presentar cada material.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_tipo_recurso | INTEGER PK |
| nombre_tipo_recurso | VARCHAR(30) |

### Registros de catálogo

| id_tipo_recurso | nombre_tipo_recurso |
|---:|---|
| 1 | pdf |
| 2 | enlace |
| 3 | archivo |
| 4 | repositorio |

---

## 11. Leccion_Recurso

Representa la relación entre lecciones y recursos.

**Justificación:**  
Se crea como entidad intermedia porque una lección puede tener varios recursos y un mismo recurso puede aparecer en varias lecciones.

El atributo `numero_orden` no ordena las lecciones ni el video principal. Solo indica el orden en que se muestran los recursos adicionales dentro de una misma lección.

Ejemplo:

| id_leccion | id_recurso | numero_orden |
|---:|---:|---:|
| 1 | 1 | 1 |
| 1 | 2 | 2 |
| 2 | 1 | 1 |

Interpretación:

- En la lección 1, el recurso 1 aparece primero.
- En la lección 1, el recurso 2 aparece segundo.
- En la lección 2, el recurso 1 también puede aparecer primero porque es otra lección.

Para evitar errores, no debe repetirse el mismo `numero_orden` dentro de la misma lección.

Regla recomendada:

| Regla | Explicación |
|---|---|
| No repetir `numero_orden` para la misma lección | Evita que dos recursos aparezcan en la misma posición. |
| Permitir repetir `numero_orden` en lecciones diferentes | Cada lección tiene su propio orden de recursos. |

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_leccion | INTEGER PK/FK |
| id_recurso | INTEGER PK/FK |
| numero_orden | INTEGER |

### Registros de ejemplo

| id_leccion | id_recurso | numero_orden |
|---:|---:|---:|
| 1 | 1 | 1 |
| 1 | 2 | 2 |

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
| cancelada | El alumno canceló explícitamente la compra. |
| fallida | Hubo error o rechazo en el proceso de pago. |
| expirada | La orden quedó sin pago durante demasiado tiempo. |

Si el alumno solo abandona la página sin cancelar, la orden puede permanecer como `pendiente` hasta que el sistema la marque como `expirada`.

### Atributos

| Atributo | Tipo de dato sugerido |
|---|---|
| id_estado_orden | INTEGER PK |
| nombre_estado_orden | VARCHAR(30) |

### Registros de catálogo

| id_estado_orden | nombre_estado_orden |
|---:|---|
| 1 | pendiente |
| 2 | completada |
| 3 | cancelada |
| 4 | fallida |
| 5 | expirada |

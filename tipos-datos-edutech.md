# Tipos de datos para diagrama relacional - EduTech

Version para diagrama: solo PK, FK y tipo de dato. No incluye NOT NULL ni reglas SQL avanzadas dentro de las tablas.

## Correcciones aplicadas

- `Orden` ya no tiene `id_curso`.
- Se agrego `Orden_Detalle` para permitir que una orden incluya varios cursos.
- `Inscripcion` apunta a `id_orden_detalle`, porque cada curso comprado genera su propia inscripcion.
- `Pago` representa la transaccion o intento de pago de una orden.
- `Webhook_Pago` apunta a `Pago`; no repite `id_proveedor_pago`.
- `Intento_Pregunta` guarda la pregunta asignada y la opcion seleccionada.
- `Examen.id_curso` debe marcarse como UNIQUE en la explicacion o script.

## Tablas

### Rol
| Atributo | Tipo | Clave |
|---|---|---|
| id_rol | INTEGER | PK |
| nombre_rol | VARCHAR(30) |  |

### Usuario
| Atributo | Tipo | Clave |
|---|---|---|
| id_usuario | INTEGER | PK |
| nombre | VARCHAR(50) |  |
| apellido_paterno | VARCHAR(50) |  |
| apellido_materno | VARCHAR(50) |  |
| correo_electronico | VARCHAR(100) |  |
| password_hash | VARCHAR(255) |  |
| telefono | CHAR(10) |  |
| esta_activo | BOOLEAN |  |
| fecha_registro | TIMESTAMP |  |
| fecha_actualizacion | TIMESTAMP |  |
| id_rol | INTEGER | FK |

### Estado_Solicitud_Instructor
| Atributo | Tipo | Clave |
|---|---|---|
| id_estado_solicitud | INTEGER | PK |
| nombre_estado_solicitud | VARCHAR(30) |  |

### Solicitud_Instructor
| Atributo | Tipo | Clave |
|---|---|---|
| id_solicitud | INTEGER | PK |
| nombre_solicitante | VARCHAR(50) |  |
| apellido_paterno_solicitante | VARCHAR(50) |  |
| apellido_materno_solicitante | VARCHAR(50) |  |
| correo_solicitante | VARCHAR(100) |  |
| telefono_solicitante | CHAR(10) |  |
| area_experiencia | VARCHAR(100) |  |
| portafolio_url | VARCHAR(255) |  |
| motivo | TEXT |  |
| experiencia | TEXT |  |
| fecha_solicitud | TIMESTAMP |  |
| fecha_revision | TIMESTAMP |  |
| comentario_revision | TEXT |  |
| id_usuario_solicitante | INTEGER | FK |
| id_usuario_revisor | INTEGER | FK |
| id_estado_solicitud | INTEGER | FK |

### Nivel_Curso
| Atributo | Tipo | Clave |
|---|---|---|
| id_nivel_curso | INTEGER | PK |
| nombre_nivel | VARCHAR(30) |  |

### Estado_Curso
| Atributo | Tipo | Clave |
|---|---|---|
| id_estado_curso | INTEGER | PK |
| nombre_estado_curso | VARCHAR(30) |  |

### Curso
| Atributo | Tipo | Clave |
|---|---|---|
| id_curso | INTEGER | PK |
| titulo | VARCHAR(150) |  |
| descripcion | TEXT |  |
| imagen_portada | VARCHAR(255) |  |
| precio_mxn | NUMERIC(10,2) |  |
| fecha_creacion | TIMESTAMP |  |
| fecha_actualizacion | TIMESTAMP |  |
| id_usuario | INTEGER | FK |
| id_nivel_curso | INTEGER | FK |
| id_estado_curso | INTEGER | FK |

### Estado_Revision_Curso
| Atributo | Tipo | Clave |
|---|---|---|
| id_estado_revision_curso | INTEGER | PK |
| nombre_estado_revision_curso | VARCHAR(30) |  |

### Revision_Curso
| Atributo | Tipo | Clave |
|---|---|---|
| id_revision_curso | INTEGER | PK |
| comentario | TEXT |  |
| fecha_revision | TIMESTAMP |  |
| id_curso | INTEGER | FK |
| id_estado_revision_curso | INTEGER | FK |
| id_usuario_revisor | INTEGER | FK |

### Modulo
| Atributo | Tipo | Clave |
|---|---|---|
| id_modulo | INTEGER | PK |
| titulo | VARCHAR(150) |  |
| numero_orden | INTEGER |  |
| id_curso | INTEGER | FK |

### Tipo_Video
| Atributo | Tipo | Clave |
|---|---|---|
| id_tipo_video | INTEGER | PK |
| nombre_tipo_video | VARCHAR(30) |  |

### Leccion
| Atributo | Tipo | Clave |
|---|---|---|
| id_leccion | INTEGER | PK |
| titulo | VARCHAR(150) |  |
| numero_orden | INTEGER |  |
| texto_descriptivo | TEXT |  |
| url_video | VARCHAR(255) |  |
| duracion_segundos | INTEGER |  |
| esta_activa | BOOLEAN |  |
| id_tipo_video | INTEGER | FK |
| id_modulo | INTEGER | FK |

### Tipo_Recurso
| Atributo | Tipo | Clave |
|---|---|---|
| id_tipo_recurso | INTEGER | PK |
| nombre_tipo_recurso | VARCHAR(30) |  |

### Recurso
| Atributo | Tipo | Clave |
|---|---|---|
| id_recurso | INTEGER | PK |
| titulo | VARCHAR(150) |  |
| descripcion | VARCHAR(255) |  |
| url_recurso | VARCHAR(255) |  |
| fecha_creacion | TIMESTAMP |  |
| id_tipo_recurso | INTEGER | FK |

### Leccion_Recurso
| Atributo | Tipo | Clave |
|---|---|---|
| id_recurso | INTEGER | PK/FK |
| id_leccion | INTEGER | PK/FK |
| numero_orden | INTEGER |  |

### Moneda
| Atributo | Tipo | Clave |
|---|---|---|
| id_moneda | INTEGER | PK |
| codigo_moneda | CHAR(3) |  |
| nombre_moneda | VARCHAR(30) |  |

### Estado_Orden
| Atributo | Tipo | Clave |
|---|---|---|
| id_estado_orden | INTEGER | PK |
| nombre_estado_orden | VARCHAR(30) |  |

### Orden
| Atributo | Tipo | Clave |
|---|---|---|
| id_orden | INTEGER | PK |
| numero_orden | VARCHAR(20) |  |
| total | NUMERIC(10,2) |  |
| fecha_creacion | TIMESTAMP |  |
| fecha_actualizacion | TIMESTAMP |  |
| id_moneda | INTEGER | FK |
| id_estado_orden | INTEGER | FK |
| id_usuario | INTEGER | FK |

### Orden_Detalle
| Atributo | Tipo | Clave |
|---|---|---|
| id_orden_detalle | INTEGER | PK |
| precio_unitario | NUMERIC(10,2) |  |
| id_orden | INTEGER | FK |
| id_curso | INTEGER | FK |

### Entidad_Federativa
| Atributo | Tipo | Clave |
|---|---|---|
| id_entidad_federativa | INTEGER | PK |
| nombre_entidad_federativa | VARCHAR(80) |  |

### Ciudad
| Atributo | Tipo | Clave |
|---|---|---|
| id_ciudad | INTEGER | PK |
| nombre_ciudad | VARCHAR(80) |  |
| id_entidad_federativa | INTEGER | FK |

### Datos_Compra
| Atributo | Tipo | Clave |
|---|---|---|
| id_orden | INTEGER | PK/FK |
| nombre_contacto | VARCHAR(50) |  |
| apellido_paterno_contacto | VARCHAR(50) |  |
| apellido_materno_contacto | VARCHAR(50) |  |
| correo_electronico_contacto | VARCHAR(100) |  |
| telefono_contacto | CHAR(10) |  |
| direccion | VARCHAR(180) |  |
| codigo_postal | CHAR(5) |  |
| id_ciudad | INTEGER | FK |

### Proveedor_Pago
| Atributo | Tipo | Clave |
|---|---|---|
| id_proveedor_pago | INTEGER | PK |
| nombre_proveedor | VARCHAR(30) |  |

### Estado_Pago
| Atributo | Tipo | Clave |
|---|---|---|
| id_estado_pago | INTEGER | PK |
| nombre_estado_pago | VARCHAR(30) |  |

### Pago
| Atributo | Tipo | Clave |
|---|---|---|
| id_pago | INTEGER | PK |
| id_pago_externo | VARCHAR(120) |  |
| monto_pagado | NUMERIC(10,2) |  |
| fecha_pago | TIMESTAMP |  |
| id_proveedor_pago | INTEGER | FK |
| id_orden | INTEGER | FK |
| id_estado_pago | INTEGER | FK |

### Estado_Webhook
| Atributo | Tipo | Clave |
|---|---|---|
| id_estado_webhook | INTEGER | PK |
| nombre_estado_webhook | VARCHAR(30) |  |

### Webhook_Pago
| Atributo | Tipo | Clave |
|---|---|---|
| id_webhook | INTEGER | PK |
| tipo_evento | VARCHAR(100) |  |
| id_evento_externo | VARCHAR(150) |  |
| contenido_evento | JSONB |  |
| fecha_recibido | TIMESTAMP |  |
| id_estado_webhook | INTEGER | FK |
| id_pago | INTEGER | FK |

### Estado_Inscripcion
| Atributo | Tipo | Clave |
|---|---|---|
| id_estado_inscripcion | INTEGER | PK |
| nombre_estado_inscripcion | VARCHAR(30) |  |

### Inscripcion
| Atributo | Tipo | Clave |
|---|---|---|
| id_inscripcion | INTEGER | PK |
| fecha_inscripcion | TIMESTAMP |  |
| fecha_finalizacion | TIMESTAMP |  |
| id_estado_inscripcion | INTEGER | FK |
| id_orden_detalle | INTEGER | FK |

### Progreso_Leccion
| Atributo | Tipo | Clave |
|---|---|---|
| id_inscripcion | INTEGER | PK/FK |
| id_leccion | INTEGER | PK/FK |
| completada | BOOLEAN |  |
| fecha_completada | TIMESTAMP |  |

### Estado_Examen
| Atributo | Tipo | Clave |
|---|---|---|
| id_estado_examen | INTEGER | PK |
| nombre_estado_examen | VARCHAR(30) |  |

### Examen
| Atributo | Tipo | Clave |
|---|---|---|
| id_examen | INTEGER | PK |
| titulo | VARCHAR(150) |  |
| descripcion | TEXT |  |
| tiempo_limite_minutos | INTEGER |  |
| max_intentos | INTEGER |  |
| calificacion_minima | NUMERIC(5,2) |  |
| cantidad_preguntas | INTEGER |  |
| id_curso | INTEGER | FK |
| id_estado_examen | INTEGER | FK |

### Pregunta
| Atributo | Tipo | Clave |
|---|---|---|
| id_pregunta | INTEGER | PK |
| texto_pregunta | TEXT |  |
| esta_activa | BOOLEAN |  |
| id_examen | INTEGER | FK |

### Opcion_Respuesta
| Atributo | Tipo | Clave |
|---|---|---|
| id_opcion | INTEGER | PK |
| texto_opcion | TEXT |  |
| es_correcta | BOOLEAN |  |
| id_pregunta | INTEGER | FK |

### Estado_Intento
| Atributo | Tipo | Clave |
|---|---|---|
| id_estado_intento | INTEGER | PK |
| nombre_estado_intento | VARCHAR(30) |  |

### Intento_Examen
| Atributo | Tipo | Clave |
|---|---|---|
| id_intento | INTEGER | PK |
| numero_intento | INTEGER |  |
| fecha_inicio | TIMESTAMP |  |
| fecha_fin | TIMESTAMP |  |
| calificacion | NUMERIC(5,2) |  |
| aprobado | BOOLEAN |  |
| id_inscripcion | INTEGER | FK |
| id_estado_intento | INTEGER | FK |

### Intento_Pregunta
| Atributo | Tipo | Clave |
|---|---|---|
| id_intento_pregunta | INTEGER | PK |
| numero_orden | INTEGER |  |
| id_intento | INTEGER | FK |
| id_pregunta | INTEGER | FK |
| id_opcion_seleccionada | INTEGER | FK |

### Certificado
| Atributo | Tipo | Clave |
|---|---|---|
| id_certificado | INTEGER | PK |
| codigo_certificado | VARCHAR(20) |  |
| fecha_emision | TIMESTAMP |  |
| url_certificado | VARCHAR(255) |  |
| id_inscripcion | INTEGER | FK |

## Reglas recomendadas para explicar fuera del diagrama

- `Examen.id_curso` UNIQUE: un curso solo tiene un examen final.
- `Orden_Detalle` UNIQUE(`id_orden`, `id_curso`): evita repetir el mismo curso en una misma orden.
- `Modulo` UNIQUE(`id_curso`, `numero_orden`): evita dos modulos con el mismo orden dentro del curso.
- `Leccion` UNIQUE(`id_modulo`, `numero_orden`): evita dos lecciones con el mismo orden dentro del modulo.
- `Leccion_Recurso` UNIQUE(`id_leccion`, `numero_orden`): evita dos recursos en la misma posicion dentro de la misma leccion.

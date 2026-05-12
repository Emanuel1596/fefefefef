# Cambios realizados en los archivos corregidos

## Cambios generales

- Se actualizo el modelo para permitir que una orden tenga varios cursos.
- Se agrego `Orden_Detalle` como entidad intermedia entre `Orden` y `Curso`.
- Se elimino la relacion directa `Orden -> Curso`.
- Se ajusto `Inscripcion` para que dependa de `id_orden_detalle`.
- Se mantuvo `Pago` como nombre de entidad, pero se explico que representa la transaccion o intento de pago de una orden.
- Se elimino la redundancia de `id_proveedor_pago` en `Webhook_Pago`.
- Se uso `Entidad_Federativa` como nombre consistente para estados de Mexico.
- Se sustituyo el modelo anterior de `Pregunta_Intento` + `Respuesta_Alumno` por `Intento_Pregunta`, de acuerdo con el ultimo documento de tipos de datos.
- Se agregaron `Estado_Revision_Curso` y `Revision_Curso`.
- Se agregaron y normalizaron `Estado_Solicitud_Instructor` y `Solicitud_Instructor`.

## Archivos modificados

| Archivo | Cambio principal |
|---|---|
| actores.md | Se mantuvieron actores; se aclaro que la compra multiple no crea actor nuevo. |
| acciones_actores.md | Se actualizo Alumno, Administrador y Pasarela de pago para compra de uno o varios cursos por orden. |
| requerimientos.md | Se corrigio compra, resumen del pedido, detalle de orden, validacion de pago, liberacion de cursos e inscripciones por curso. |
| mapa-navegacion.md | Se actualizo el flujo para lista de cursos seleccionados, Orden_Detalle e inscripcion por cada curso comprado. |
| pantallas.md | Se ajusto compra-curso.html para mostrar lista de cursos, precio unitario y total; se corrigio resultado-examen.html. |
| modelo-base-datos.md | Se rehizo el modelo con Orden_Detalle, Inscripcion por detalle de orden, Revision_Curso e Intento_Pregunta. |
| tipos-datos-edutech.md | Se paso el PDF de tipos a formato Markdown corregido y consistente. |
| estructura-carpetas.md | Se limpio la estructura y se organizo por docs, frontend y recursos. |
| diagrama-relacional.drawio | Se genero un diagrama editable actualizado con las relaciones nuevas. |

## Decision sobre carrito

No se agrego `Carrito` ni `Carrito_Detalle` como parte del modelo actual, porque el ultimo documento de tipos de datos no los incluye. La compra de varios cursos queda resuelta con `Orden` y `Orden_Detalle`.

Si despues el profesor pide un carrito persistente antes de crear la orden, se puede agregar en una siguiente version.

## Regla final del modelo

`Orden` guarda la compra completa.

`Orden_Detalle` guarda cada curso incluido en esa compra.

`Pago` confirma la transaccion de la orden.

`Inscripcion` libera el acceso educativo de cada curso comprado.

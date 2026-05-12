# Definicion de pantallas a disenar - EduTech

## Decision general

Para el proyecto completo se recomienda planear 25 pantallas HTML principales. Para el primer sprint se recomienda disenar 08-10 pantallas HTML principales.

## Totales

| Alcance | Recomendacion | Motivo |
|---|---:|---|
| Proyecto completo | 25 HTML | Cubre zona publica, alumno, instructor, administrador, compra, examen y solicitudes. |
| Primer sprint | 14 HTML | Permite entregar una version defendible sin crear pantallas innecesarias. |

## Pantallas totales del proyecto completo

| No. | Archivo HTML | Pantalla |
|---:|---|---|
| 1 | index.html | Inicio |
| 2 | cursos.html | Catalogo de cursos |
| 3 | detalle-curso.html | Detalle del curso |
| 4 | registro.html | Registro |
| 5 | login.html | Inicio de sesion |
| 6 | quienes-somos.html | Quienes somos |
| 7 | contacto.html | Contacto |
| 8 | compra-curso.html | Compra del curso |
| 9 | confirmacion-compra.html | Confirmacion de compra |
| 10 | historial-pedidos.html | Historial de pedidos |
| 11 | detalle-pedido.html | Detalle del pedido |
| 12 | escritorio-alumno.html | Escritorio del alumno |
| 13 | mis-cursos.html | Mis cursos |
| 14 | curso-comprado.html | Vista del curso comprado |
| 15 | leccion.html | Leccion |
| 16 | examen-final.html | Examen final |
| 17 | presentar-examen.html | Presentar examen |
| 18 | resultado-examen.html | Resultado del examen |
| 19 | mis-certificados.html | Mis certificados |
| 20 | mi-cuenta.html | Mi cuenta |
| 21 | solicitud-instructor.html | Solicitud de instructor |
| 22 | escritorio-instructor.html | Escritorio del instructor |
| 23 | cursos-instructor.html | Cursos del instructor |
| 24 | administrar-curso.html | Administrar curso |
| 25 | escritorio-admin.html | Escritorio del administrador / panel administrativo |

## Pantallas para el primer sprint

| No. | Archivo HTML | Pantalla | Explicacion breve | Requerimientos que cubre |
|---:|---|---|---|---|
| 1 | index.html | Inicio | Presentar EduTech, mostrar cursos destacados y dirigir a cursos, registro o login. | RF-08, RF-09 |
| 2 | cursos.html | Catalogo de cursos | Mostrar cursos publicados disponibles para compra. | RF-08, RF-09, RF-12 |
| 3 | detalle-curso.html | Detalle del curso | Mostrar descripcion, modulos, lecciones, instructor, nivel, precio y accion de compra. | RF-10, RF-11, RF-14 |
| 4 | registro.html | Registro | Crear cuenta de Alumno y validar datos de registro. | RF-01, RF-02 |
| 5 | login.html | Inicio de sesion | Permitir acceso y redireccionar segun rol. | RF-03, RF-05, RF-06 |
| 6 | compra-curso.html | Compra del curso | Mostrar datos de contacto, facturacion opcional, lista de cursos seleccionados, precio unitario, total de la orden y boton de pago externo. | RF-14, RF-15, RF-16, RF-17, RF-18, RF-19, RF-20 |
| 7 | confirmacion-compra.html | Confirmacion de compra | Mostrar resultado del pago y explicar si se liberan o no los cursos. | RF-21, RF-22, RF-23, RF-24, RF-25, RF-26, RF-28 |
| 8 | escritorio-alumno.html | Escritorio del alumno | Mostrar resumen de cursos, progreso, pedidos y certificados. | RF-27, RF-30, RF-41, RF-52 |
| 9 | mis-cursos.html | Mis cursos | Listar cursos con pago aprobado e inscripcion activa. | RF-25, RF-28, RF-29, RF-30 |
| 10 | curso-comprado.html | Vista del curso comprado | Mostrar aula virtual con modulos, lecciones y progreso. | RF-31, RF-32, RF-33, RF-39, RF-40, RF-41 |
| 11 | leccion.html | Leccion | Mostrar video, texto, recursos y accion de completar leccion. | RF-34, RF-35, RF-36, RF-37, RF-38, RF-39, RF-40 |
| 12 | examen-final.html | Examen final | Mostrar instrucciones, tiempo limite, intentos y condiciones para presentar. | RF-42, RF-46, RF-47, RF-48 |
| 13 | mi-cuenta.html | Mi cuenta | Permitir ver y editar informacion personal. | RF-07 |
| 14 | solicitud-instructor.html | Solicitud de instructor | Permitir que un alumno solicite ser instructor. | RF-65, RF-66, RF-67 |

## Pantalla opcional futura

| Archivo HTML | Motivo |
|---|---|
| carrito.html | Solo se agrega si el equipo decide manejar un carrito persistente antes de crear la orden. Para el modelo actual no es obligatorio, porque varios cursos se resuelven con `Orden_Detalle`. |

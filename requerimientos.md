# Requerimientos del Sistema EduTech

Este documento contiene los requerimientos funcionales y no funcionales del sistema EduTech, una plataforma web para la venta de cursos en linea.

## Indice

- A. Usuarios, registro y acceso
- B. Cursos
- C. Compra, orden y pago
- D. Inscripcion y acceso
- E. Modulos y lecciones
- F. Progreso del curso
- G. Examen final
- H. Finalizacion y certificado
- I. Instructor
- J. Administrador
- K. Cuenta y solicitudes
- L. Requerimientos no funcionales

---

# A. Usuarios, registro y acceso

## RF-01. Registro de alumno

El sistema debe permitir que un visitante cree una cuenta con rol de Alumno proporcionando nombre, apellidos, correo electronico, confirmacion de correo, contrasena y confirmacion de contrasena.

## RF-02. Validacion del registro

El sistema debe validar formato de correo, coincidencia de correo, nombres y apellidos con caracteres validos, requisitos minimos de contrasena y confirmacion de contrasena.

## RF-03. Inicio de sesion

El sistema debe permitir que los usuarios inicien sesion usando correo electronico y contrasena.

## RF-04. Cierre de sesion

El sistema debe permitir que los usuarios cierren sesion de forma segura.

## RF-05. Manejo de roles

El sistema debe manejar tres roles principales: Alumno, Instructor y Administrador.

## RF-06. Control de acceso por rol

El sistema debe permitir que cada usuario acceda unicamente a las funciones correspondientes a su rol.

## RF-07. Edicion de informacion de cuenta

El sistema debe permitir que los usuarios autenticados editen informacion personal permitida, como nombre, apellidos, telefono o contrasena.

---

# B. Cursos

## RF-08. Catalogo de cursos

El sistema debe permitir que visitantes y alumnos consulten cursos publicados disponibles para compra.

## RF-09. Informacion visible del curso

El sistema debe mostrar por cada curso imagen de portada, nombre, descripcion, instructor, nivel, numero de lecciones y precio en pesos mexicanos.

## RF-10. Detalle del curso

El sistema debe permitir consultar descripcion, modulos, lecciones, instructor, nivel y precio de un curso.

## RF-11. Cursos unicamente de pago

El sistema debe manejar cursos de pago. Un alumno no debe acceder al contenido sin una compra aprobada.

## RF-12. Precio en pesos mexicanos

El sistema debe registrar y mostrar el precio de los cursos en pesos mexicanos.

## RF-13. Revision de cursos

El sistema debe permitir que el instructor envie cursos a revision y que el administrador los apruebe o rechace antes de publicarlos.

---

# C. Compra, orden y pago

## RF-14. Inicio de compra

El sistema debe permitir que un alumno autenticado inicie la compra de uno o varios cursos publicados. Si un visitante intenta comprar, el sistema debe solicitarle iniciar sesion o registrarse.

## RF-15. Datos de contacto para compra

El sistema debe solicitar datos de contacto para la compra, como nombre, apellidos, correo electronico y telefono. Si el alumno ya tiene datos registrados, el sistema puede precargarlos.

Los datos de facturacion, como direccion, ciudad, entidad federativa y codigo postal, pueden manejarse como informacion opcional.

## RF-16. Resumen del pedido

El sistema debe mostrar un resumen del pedido antes de pagar, incluyendo lista de cursos seleccionados, precio unitario por curso, total de la orden, moneda y metodo de pago.

## RF-17. Creacion de orden pendiente

El sistema debe crear una orden con estado pendiente cuando el alumno autenticado inicia el proceso de compra. La orden debe tener un numero visible generado por el sistema.

## RF-18. Detalle de orden

El sistema debe crear un registro de detalle de orden por cada curso incluido en la compra. Cada detalle debe guardar el curso comprado y el precio unitario usado al momento de la compra.

## RF-19. Boton de pago externo

El sistema debe mostrar un boton de pago con PayPal Sandbox o Stripe Sandbox.

## RF-20. Procesamiento del pago en Sandbox

El sistema debe enviar la solicitud de pago a la pasarela externa en ambiente Sandbox.

## RF-21. Webhook de confirmacion de pago

El sistema debe recibir una notificacion automatica mediante webhook cuando PayPal o Stripe confirme el resultado del pago.

## RF-22. Validacion del pago

El sistema debe validar que el pago recibido corresponda al alumno, la orden, los cursos incluidos en el detalle de orden, el monto total y la moneda correcta.

## RF-23. Estados de pago

El sistema debe manejar estados de pago como pendiente, aprobado, rechazado o cancelado.

## RF-24. Estados de orden

El sistema debe manejar estados de orden como pendiente, completada, cancelada, fallida o expirada.

## RF-25. Liberacion automatica de cursos

El sistema debe liberar el acceso a cada curso incluido en la orden unicamente cuando el pago sea confirmado como aprobado y la orden quede completada.

## RF-26. Bloqueo por pago no aprobado

El sistema no debe permitir que el alumno acceda a cursos de una orden con pago pendiente, rechazado o cancelado, o con orden pendiente, fallida, cancelada o expirada.

## RF-27. Historial de pedidos

El sistema debe permitir que el alumno consulte su historial de pedidos, incluyendo numero de orden, cursos incluidos, fecha, total, estado de orden y estado del pago cuando exista.

---

# D. Inscripcion y acceso

## RF-28. Inscripcion automatica por curso comprado

El sistema debe crear automaticamente una inscripcion por cada curso incluido en el detalle de una orden cuando el pago sea aprobado y la orden quede completada.

## RF-29. Acceso solo a cursos comprados

El sistema debe permitir que el alumno acceda unicamente a los cursos que haya comprado correctamente y que tengan inscripcion activa.

## RF-30. Mis cursos

El sistema debe mostrar en Mis cursos unicamente los cursos cuya compra fue aprobada y cuya inscripcion esta activa o completada.

---

# E. Modulos y lecciones

## RF-31. Organizacion por modulos

El sistema debe permitir que cada curso este organizado en modulos.

## RF-32. Datos del modulo

Cada modulo debe tener titulo y numero de orden dentro del curso.

## RF-33. Organizacion por lecciones

El sistema debe permitir que cada modulo tenga varias lecciones.

## RF-34. Datos de la leccion

Cada leccion debe tener titulo, numero de orden, texto descriptivo, video y recursos adicionales cuando correspondan.

## RF-35. Visualizacion de lecciones

El sistema debe permitir que el alumno visualice lecciones de los cursos a los que tiene acceso.

## RF-36. Videos de leccion

El sistema debe permitir videos embebidos desde YouTube o Vimeo, o videos cargados localmente de forma optimizada.

## RF-37. Recursos adicionales

El sistema debe permitir agregar recursos adicionales a las lecciones, como enlaces, archivos descargables o repositorios.

---

# F. Progreso del curso

## RF-38. Marcar leccion como completada

El sistema debe permitir que el alumno marque una leccion como completada.

## RF-39. Avance secuencial obligatorio

El sistema debe bloquear la siguiente leccion hasta que el alumno complete la anterior.

## RF-40. Registro de progreso

El sistema debe guardar el progreso del alumno por leccion dentro de una inscripcion.

## RF-41. Consulta de progreso

El sistema debe mostrar cuantas lecciones ha completado el alumno y el porcentaje de avance calculado del curso.

---

# G. Examen final

## RF-42. Examen final por curso

El sistema debe permitir que cada curso tenga un examen final.

## RF-43. Banco de preguntas

El sistema debe permitir que el instructor cree preguntas para el examen final.

## RF-44. Preguntas de opcion multiple

El sistema debe permitir preguntas de opcion multiple.

## RF-45. Opciones de respuesta

Cada pregunta debe tener varias opciones de respuesta y debe poder identificarse la correcta.

## RF-46. Examen con preguntas aleatorias

El sistema debe seleccionar preguntas del banco para cada intento y conservar cuales preguntas recibio el alumno.

## RF-47. Intentos de examen

El sistema debe registrar cada intento de examen con numero de intento, fechas, estado, calificacion y aprobacion.

## RF-48. Tiempo limite

El sistema debe manejar tiempo limite para presentar el examen.

## RF-49. Calificacion automatica

El sistema debe calcular la calificacion a partir de las respuestas seleccionadas.

---

# H. Finalizacion y certificado

## RF-50. Finalizacion del curso

El sistema debe registrar la finalizacion cuando el alumno complete las lecciones requeridas y apruebe el examen final.

## RF-51. Certificado digital

El sistema debe generar un certificado digital con codigo visible cuando el alumno complete el curso.

## RF-52. Consulta de certificados

El sistema debe permitir que el alumno consulte o descargue sus certificados.

---

# I. Instructor

## RF-53. Panel del instructor

El sistema debe permitir que el instructor acceda a un panel para gestionar sus cursos.

## RF-54. Gestion de cursos propios

El instructor solo debe crear, editar y administrar sus propios cursos.

## RF-55. Gestion de contenido

El instructor debe poder crear modulos, lecciones, videos y recursos dentro de sus cursos.

## RF-56. Gestion de examen

El instructor debe poder configurar el examen final y crear preguntas con opciones de respuesta.

## RF-57. Consulta de alumnos inscritos

El instructor debe poder consultar alumnos inscritos en sus cursos, su progreso y sus resultados de examen.

---

# J. Administrador

## RF-58. Panel del administrador

El sistema debe permitir que el administrador acceda a un panel general de supervision.

## RF-59. Gestion de usuarios

El administrador debe poder consultar, activar, desactivar y asignar roles a usuarios.

## RF-60. Solicitudes de instructor

El administrador debe poder revisar, aceptar o rechazar solicitudes de instructor.

## RF-61. Revision de cursos

El administrador debe poder revisar cursos enviados por instructores y registrar comentarios de revision.

## RF-62. Publicacion y despublicacion

El administrador debe poder publicar cursos aprobados o despublicar cursos del catalogo sin eliminar historiales.

## RF-63. Consulta de ordenes y pagos

El administrador debe poder consultar ordenes, detalles de orden, pagos y webhooks.

## RF-64. Consulta de inscripciones

El administrador debe poder consultar las inscripciones generadas por compras aprobadas.

---

# K. Cuenta y solicitudes

## RF-65. Solicitud de instructor

El sistema debe permitir que un alumno envie una solicitud para convertirse en instructor.

## RF-66. Datos de solicitud

La solicitud debe permitir capturar area de experiencia, experiencia, portafolio o evidencia, motivo y datos de contacto.

## RF-67. Estados de solicitud

El sistema debe manejar estados de solicitud como pendiente, aceptada o rechazada.

## RF-68. Recuperacion de contrasena

El sistema debe permitir iniciar un proceso de recuperacion de contrasena mediante un token seguro o mecanismo equivalente.

---

# L. Requerimientos no funcionales

## RNF-01. Seguridad de contrasenas

Las contrasenas no deben guardarse en texto plano. Debe almacenarse un `password_hash`.

## RNF-02. Control de acceso por URL

El sistema no debe permitir acceso directo por URL a cursos, lecciones, examenes o archivos restringidos si el usuario no tiene permisos.

## RNF-03. Disponibilidad de video

El reproductor de video debe soportar YouTube, Vimeo o carga local optimizada.

## RNF-04. Integridad de pagos

El sistema debe validar monto, moneda, orden y estado antes de crear inscripciones.

## RNF-05. Trazabilidad

El sistema debe conservar registros importantes como ordenes, pagos, webhooks, inscripciones, progreso, intentos de examen y certificados.

## RNF-06. Normalizacion de base de datos

Los estados, tipos y valores controlados deben manejarse mediante catalogos cuando tengan mas de dos valores de negocio.

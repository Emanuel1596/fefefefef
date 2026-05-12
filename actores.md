# Lista de actores del sistema

## Proyecto: EduTech - Plataforma Web de Cursos

Un actor representa a una persona, rol o sistema externo que interactua con la plataforma para realizar una accion.

## Actores del sistema

| Actor | Tipo de actor | Descripcion |
|---|---|---|
| Visitante | Persona externa | Usuario que entra a la plataforma sin iniciar sesion. Puede consultar la pagina de inicio, ver el catalogo de cursos, revisar el detalle de un curso, registrarse o iniciar sesion. |
| Alumno | Usuario registrado | Usuario que puede comprar uno o varios cursos, pagar una orden, acceder a los cursos adquiridos, ver modulos y lecciones, marcar lecciones como completadas, consultar su progreso, presentar el examen final y obtener certificados. |
| Instructor | Usuario registrado | Usuario encargado de crear y administrar sus propios cursos. Puede crear modulos, lecciones, recursos, configurar el examen final, administrar preguntas y consultar el progreso o resultados de sus alumnos. |
| Administrador | Usuario registrado | Usuario encargado de supervisar el sistema. Puede gestionar usuarios, asignar roles, revisar solicitudes de instructor, revisar cursos, publicar o despublicar cursos, consultar ordenes, pagos, inscripciones y reglas de acceso. |
| Pasarela de pago | Sistema externo | Sistema externo, como PayPal o Stripe, que procesa el pago de una orden. Despues de procesar la transaccion, notifica a EduTech mediante webhook para confirmar si el pago fue aprobado, rechazado, cancelado o quedo pendiente. |

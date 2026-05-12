# Lista de actores del sistema

## Proyecto: EduTech - Plataforma Web de Cursos

Un actor puede ser una persona, un rol de usuario o un sistema externo que participa en alguna acción importante dentro de la plataforma.

---

## Actores del sistema

| Actor | Tipo de actor | Descripción |
|---|---|---|
| Visitante | Persona externa | Persona que entra a EduTech sin iniciar sesión. Puede consultar la página de inicio, ver el catálogo de cursos, revisar el detalle de un curso, registrarse, iniciar sesión o consultar información pública. |
| Alumno | Usuario registrado | Usuario que compra uno o varios cursos, consulta sus órdenes, accede a cursos aprobados, ve módulos y lecciones, registra progreso, presenta examen final y obtiene certificados. |
| Instructor | Usuario registrado | Usuario encargado de crear y administrar sus propios cursos. Puede crear cursos, módulos, lecciones, recursos, configurar examen final, administrar preguntas y consultar el avance de alumnos inscritos en sus cursos. |
| Administrador | Usuario registrado | Usuario encargado de supervisar el sistema. Puede gestionar usuarios, revisar solicitudes de instructor, revisar cursos, publicar o despublicar cursos, consultar órdenes, pagos, inscripciones y validar el funcionamiento general de la plataforma. |
| Pasarela de pago | Sistema externo | Sistema externo, como PayPal o Stripe, que procesa la transacción de pago de una orden. Después notifica a EduTech mediante webhook si el pago fue aprobado, rechazado, cancelado o quedó pendiente. |


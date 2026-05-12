# Lista de actores del sistema

## Proyecto: EduTech - Plataforma Web de Cursos

Este documento identifica los actores que interactúan con EduTech.

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

---

## Explicación de cada actor

### Visitante

El visitante todavía no tiene cuenta o no ha iniciado sesión.

Puede navegar por las partes públicas del sistema, como inicio, catálogo, detalle del curso, contacto, registro e inicio de sesión.

No puede comprar directamente ni acceder al contenido interno de los cursos. Si intenta comprar, el sistema debe pedirle que inicie sesión o se registre.

### Alumno

El alumno es un usuario registrado con rol de Alumno.

Puede comprar cursos como productos digitales. Una misma orden puede incluir uno o varios cursos. Cuando el pago de la orden es aprobado, el sistema genera una inscripción por cada curso comprado y el alumno puede acceder a esos cursos desde Mis cursos.

El alumno también puede consultar su progreso, completar lecciones, presentar exámenes y obtener certificados.

### Instructor

El instructor es un usuario registrado habilitado para crear contenido académico.

No cualquier usuario debe poder publicar cursos. Para evitar publicaciones sin control, el instructor debe ser habilitado por el administrador o mediante una solicitud de instructor aceptada.

El instructor administra sus cursos, módulos, lecciones, recursos y exámenes, pero no debe gestionar cursos de otros instructores ni información administrativa global.

### Administrador

El administrador supervisa la plataforma.

Puede revisar usuarios, solicitudes de instructor, cursos enviados a revisión, órdenes, pagos, inscripciones y reglas de acceso. También puede publicar o despublicar cursos del catálogo.

El administrador no representa al alumno ni al instructor, sino que tiene funciones de control y supervisión.

### Pasarela de pago

La pasarela de pago es un sistema externo, no una persona.

EduTech no procesa directamente los datos sensibles de pago. La pasarela externa recibe la solicitud de pago, procesa la transacción y notifica el resultado a EduTech mediante webhook.

---

## Relación general entre actores

| Actor | Relación principal con el sistema |
|---|---|
| Visitante | Consulta información pública y puede crear cuenta. |
| Alumno | Compra cursos y consume contenido educativo. |
| Instructor | Crea y administra contenido académico. |
| Administrador | Supervisa usuarios, cursos, pagos y solicitudes. |
| Pasarela de pago | Procesa pagos y notifica resultados. |

---

## Nota importante

El cambio de modelo para permitir una orden con varios cursos no crea nuevos actores.

Los actores siguen siendo los mismos. Lo que cambia es el proceso interno de compra: ahora una orden puede contener varios cursos mediante el detalle de la orden.

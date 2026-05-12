# Definicion de acciones por actor

## Proyecto: EduTech - Plataforma Web de Cursos

EduTech funciona como una plataforma de venta de cursos digitales. El alumno compra cursos como productos digitales y conserva el acceso al contenido adquirido. No se maneja como suscripcion.

## Actores considerados

- Visitante
- Alumno
- Instructor
- Administrador
- Pasarela de pago

## Tabla general de acciones por actor

| Actor | Acciones que puede realizar |
|---|---|
| Visitante | Consultar la pagina de inicio, ver el catalogo de cursos, consultar el detalle de un curso, registrarse como alumno, iniciar sesion, consultar paginas informativas como contacto o quienes somos y solicitar cuenta de instructor. |
| Alumno | Iniciar sesion, editar su informacion de cuenta, consultar cursos disponibles, ver detalle de curso, seleccionar uno o varios cursos para compra, generar una orden, pagar mediante pasarela externa, consultar historial de pedidos, acceder a cursos comprados, ver modulos, ver lecciones, reproducir videos, consultar recursos adicionales, marcar lecciones como completadas, consultar progreso, presentar examen final, consultar resultados, ver certificados, solicitar cuenta de instructor y cerrar sesion. |
| Instructor | Iniciar sesion, editar su informacion de cuenta, acceder a su panel, crear cursos, editar sus cursos, enviar cursos a revision, crear modulos, ordenar modulos, crear lecciones, editar lecciones, agregar videos, agregar recursos adicionales, configurar examen final, crear preguntas, crear opciones de respuesta, definir respuestas correctas, consultar alumnos inscritos en sus cursos, revisar progreso de alumnos, revisar resultados de examenes y cerrar sesion. |
| Administrador | Iniciar sesion, editar su informacion de cuenta, acceder al panel de administracion, consultar usuarios, activar usuarios, desactivar usuarios, asignar roles, revisar solicitudes de instructor, aceptar solicitudes de instructor, rechazar solicitudes de instructor, crear o habilitar cuentas de instructor, revisar cursos enviados por instructores, publicar cursos, despublicar cursos del catalogo, consultar ordenes, consultar detalles de orden, consultar pagos, revisar estados de transacciones, consultar inscripciones, supervisar reglas de acceso, supervisar seguridad del sistema y cerrar sesion. |
| Pasarela de pago | Recibir solicitud de pago, procesar el pago total de una orden, confirmar si el pago fue aprobado, rechazado, cancelado o quedo pendiente, generar identificador externo de pago y notificar a EduTech mediante webhook. |

---

# Acciones detalladas por actor

## 1. Visitante

| Accion | Descripcion |
|---|---|
| Consultar pagina de inicio | Puede ver informacion general de la plataforma. |
| Ver catalogo de cursos | Puede consultar los cursos publicados disponibles para compra. |
| Ver detalle de curso | Puede revisar titulo, descripcion, precio, modulos generales, lecciones generales e instructor. |
| Registrarse como alumno | Puede crear una cuenta inicial con rol Alumno. |
| Iniciar sesion | Puede entrar al sistema si ya tiene una cuenta. |
| Consultar paginas informativas | Puede ver secciones como contacto o quienes somos. |
| Solicitar cuenta de instructor | Puede llenar un formulario para pedir que el administrador revise si puede ser habilitado como Instructor. |

## 2. Alumno

| Accion | Descripcion |
|---|---|
| Iniciar sesion | Puede acceder a su cuenta con correo electronico y contrasena. |
| Editar informacion de cuenta | Puede actualizar datos personales permitidos, como nombre, apellidos, telefono o contrasena. |
| Consultar cursos disponibles | Puede ver cursos publicados en el catalogo. |
| Ver detalle de curso | Puede consultar informacion completa del curso antes de comprar. |
| Seleccionar cursos para compra | Puede elegir uno o varios cursos publicados para incluirlos en una misma orden. |
| Generar orden | Puede iniciar una orden pendiente que agrupa los cursos seleccionados. |
| Pagar orden | Puede pagar el total de la orden mediante PayPal o Stripe en ambiente Sandbox. |
| Consultar historial de pedidos | Puede revisar sus ordenes, cursos incluidos, total, estado de orden y estado de pago. |
| Acceder a cursos comprados | Puede entrar a los cursos cuya orden fue completada y cuya inscripcion fue creada. |
| Ver modulos y lecciones | Puede consultar el contenido del curso comprado. |
| Reproducir video | Puede ver el video principal de cada leccion. |
| Consultar recursos adicionales | Puede abrir o descargar materiales asociados a la leccion. |
| Marcar leccion como completada | Puede registrar que termino una leccion. |
| Consultar progreso | Puede ver lecciones completadas y porcentaje calculado. |
| Presentar examen final | Puede realizar el examen cuando cumpla las condiciones del curso. |
| Consultar resultado del examen | Puede ver calificacion y aprobacion. |
| Ver certificado | Puede consultar o descargar su certificado si completo el curso. |
| Solicitar cuenta de instructor | Puede solicitar que su cuenta sea evaluada para convertirse en Instructor. |
| Cerrar sesion | Puede salir de su cuenta de forma segura. |

## 3. Instructor

| Accion | Descripcion |
|---|---|
| Iniciar sesion | Puede acceder al sistema con sus credenciales. |
| Editar informacion de cuenta | Puede actualizar sus datos personales permitidos. |
| Crear curso | Puede registrar un curso en estado borrador. |
| Editar curso | Puede modificar informacion de sus propios cursos mientras las reglas lo permitan. |
| Enviar curso a revision | Puede solicitar que el administrador revise un curso antes de publicarlo. |
| Crear modulos | Puede organizar el curso en modulos. |
| Ordenar modulos | Puede definir el orden en que apareceran los modulos. |
| Crear lecciones | Puede agregar lecciones dentro de los modulos. |
| Editar lecciones | Puede modificar titulo, texto, video o estado de una leccion. |
| Agregar video | Puede agregar video de YouTube, Vimeo o carga local. |
| Agregar recursos adicionales | Puede asociar PDF, enlaces, archivos o repositorios a una leccion. |
| Configurar examen final | Puede definir titulo, descripcion, tiempo limite, intentos, calificacion minima y cantidad de preguntas. |
| Crear preguntas | Puede crear preguntas para el banco del examen. |
| Crear opciones de respuesta | Puede agregar opciones a cada pregunta. |
| Definir respuesta correcta | Puede indicar la opcion correcta. |
| Consultar alumnos inscritos | Puede ver alumnos con inscripcion en sus cursos. |
| Revisar progreso | Puede consultar avance de alumnos en sus cursos. |
| Revisar resultados | Puede consultar intentos y calificaciones. |
| Cerrar sesion | Puede salir del sistema. |

## 4. Administrador

| Accion | Descripcion |
|---|---|
| Iniciar sesion | Puede acceder al panel administrativo. |
| Gestionar usuarios | Puede consultar, activar o desactivar usuarios. |
| Asignar roles | Puede cambiar el rol cuando exista una razon valida, como aceptar una solicitud de instructor. |
| Revisar solicitudes de instructor | Puede aceptar o rechazar solicitudes. |
| Revisar cursos | Puede revisar cursos enviados por instructores. |
| Publicar cursos | Puede permitir que un curso aprobado aparezca en el catalogo. |
| Despublicar cursos | Puede retirar un curso del catalogo sin eliminar historiales. |
| Consultar ordenes | Puede revisar ordenes creadas por alumnos. |
| Consultar detalles de orden | Puede ver que cursos incluye cada orden. |
| Consultar pagos | Puede revisar transacciones procesadas por la pasarela. |
| Consultar inscripciones | Puede verificar que cursos se liberaron a cada alumno. |
| Supervisar reglas de acceso | Puede revisar que solo alumnos con inscripcion activa entren a cursos. |
| Cerrar sesion | Puede salir del sistema. |

## 5. Pasarela de pago

| Accion | Descripcion |
|---|---|
| Recibir solicitud de pago | Recibe el total de la orden y datos necesarios para procesar la transaccion. |
| Procesar pago | Determina si el pago fue aprobado, rechazado, cancelado o queda pendiente. |
| Generar identificador externo | Devuelve un identificador propio de PayPal o Stripe. |
| Notificar por webhook | Envia a EduTech el resultado del pago para actualizar orden, pago e inscripciones. |

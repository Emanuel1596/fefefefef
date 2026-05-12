# Mapa de navegacion de EduTech

Version corregida con flujo condicional y compra de varios cursos por orden.

## 1. Zona publica

EduTech
├── Inicio
│   ├── Presentacion de EduTech
│   ├── Cursos destacados
│   │   └── Ver detalle -> Detalle del curso
│   ├── Ver cursos -> Catalogo de cursos
│   ├── Crear cuenta -> Registro
│   ├── Iniciar sesion -> Inicio de sesion
│   └── Solicitar cuenta de instructor -> Informacion para solicitar cuenta de instructor
├── Catalogo de cursos
│   ├── Lista de cursos publicados
│   ├── Ver detalle -> Detalle del curso
│   └── Seleccionar curso para compra -> Compra del curso
├── Detalle del curso
│   ├── Informacion general del curso
│   ├── Instructor
│   ├── Nivel
│   ├── Modulos y lecciones incluidas
│   ├── Precio en MXN
│   └── Comprar curso
│       ├── Si no ha iniciado sesion -> Inicio de sesion / Registro
│       └── Si inicio sesion como Alumno -> Compra del curso
├── Registro
│   ├── Formulario de registro
│   ├── Registrarse
│   │   ├── Si los datos son correctos -> Crear cuenta con rol Alumno -> Escritorio del alumno
│   │   └── Si los datos son incorrectos -> Permanecer en Registro y mostrar errores
│   └── Ya tengo cuenta -> Inicio de sesion
├── Inicio de sesion
│   ├── Formulario de acceso
│   ├── Iniciar sesion
│   │   ├── Si el rol es Alumno -> Escritorio del alumno
│   │   ├── Si el rol es Instructor -> Escritorio del instructor
│   │   ├── Si el rol es Administrador -> Escritorio del administrador
│   │   ├── Si el usuario esta desactivado -> Permanecer en Inicio de sesion y mostrar aviso
│   │   └── Si los datos son incorrectos -> Permanecer en Inicio de sesion y mostrar error
│   └── Crear cuenta -> Registro
├── Quienes somos
│   ├── Que es EduTech
│   ├── Mision
│   ├── Vision
│   └── Objetivo
├── Contacto
│   ├── Formulario de contacto
│   ├── Enviar mensaje
│   └── Solicitar cuenta de instructor
│       ├── Si no ha iniciado sesion -> Inicio de sesion / Registro
│       └── Si inicio sesion como Alumno -> Solicitud de instructor
└── Mi cuenta
    ├── Si no ha iniciado sesion -> Inicio de sesion
    └── Si ya inicio sesion -> Mi cuenta segun rol

---

## 2. Flujo de compra, orden y pago

Detalle del curso / Catalogo de cursos
└── Comprar o seleccionar curso
    ├── Si no ha iniciado sesion -> Inicio de sesion / Registro
    └── Si inicio sesion como Alumno -> Compra del curso

Compra del curso
├── Datos de contacto
│   ├── Nombre
│   ├── Apellidos
│   ├── Correo electronico
│   └── Telefono
├── Datos opcionales de facturacion
│   ├── Direccion
│   ├── Ciudad
│   ├── Entidad federativa
│   └── Codigo postal
├── Resumen del pedido
│   ├── Lista de cursos seleccionados
│   ├── Precio unitario por curso
│   ├── Total de la orden
│   └── Metodo de pago
├── Crear orden pendiente
│   ├── Generar numero de orden visible
│   ├── Guardar total de la orden
│   └── Crear un Orden_Detalle por cada curso incluido
└── Pagar con PayPal / Stripe Sandbox
    └── Pasarela externa
        └── Webhook hacia EduTech
            └── Confirmacion de compra
                ├── Si el pago es aprobado
                │   ├── Actualizar pago a aprobado
                │   ├── Cambiar orden a completada
                │   ├── Crear una inscripcion por cada Orden_Detalle
                │   ├── Liberar acceso a cada curso comprado
                │   └── Ir a Mis cursos
                ├── Si el pago queda pendiente
                │   ├── Mantener orden pendiente
                │   └── Ver historial de pedidos
                ├── Si el pago es rechazado
                │   ├── Cambiar pago a rechazado
                │   ├── Cambiar orden a fallida
                │   └── Intentar de nuevo -> Compra del curso
                ├── Si el pago es cancelado
                │   ├── Cambiar pago a cancelado
                │   ├── Cambiar orden a cancelada
                │   └── Volver a compra
                └── Si la orden queda sin pago durante demasiado tiempo
                    ├── Cambiar orden a expirada
                    └── Ver historial de pedidos

---

## 3. Zona del alumno

Escritorio del alumno
├── Resumen del alumno
│   ├── Cursos comprados
│   ├── Cursos en progreso
│   ├── Pedidos pendientes
│   └── Certificados obtenidos
├── Mis cursos -> Vista del curso comprado
├── Historial de pedidos -> Detalle del pedido
├── Mis calificaciones
├── Mis certificados -> Ver certificado
├── Solicitar cuenta de instructor -> Solicitud de instructor
├── Mi cuenta
└── Cerrar sesion -> Inicio

Historial de pedidos
├── Lista de ordenes
├── Numero de orden
├── Total
├── Estado de orden
├── Estado de pago
└── Detalle del pedido
    ├── Cursos incluidos en la orden
    ├── Precio unitario por curso
    └── Inscripcion generada por cada curso aprobado

Vista del curso comprado
├── Informacion del curso
├── Progreso del curso
│   ├── Lecciones completadas
│   └── Porcentaje de avance calculado
├── Modulos
│   └── Lecciones
│       ├── Leccion completada
│       ├── Leccion desbloqueada
│       └── Leccion bloqueada
├── Abrir leccion
│   ├── Si la leccion esta desbloqueada -> Leccion
│   └── Si la leccion esta bloqueada -> Permanecer en Vista del curso y mostrar aviso
└── Ir al examen final
    ├── Si completo las lecciones requeridas -> Examen final
    └── Si no completo las lecciones -> Permanecer en Vista del curso y mostrar aviso

Leccion
├── Titulo
├── Video
│   ├── Si tipo_video = YouTube -> Mostrar video embebido
│   ├── Si tipo_video = Vimeo -> Mostrar video embebido
│   └── Si tipo_video = Local -> Reproducir video local optimizado
├── Texto descriptivo
├── Recursos adicionales
│   ├── Si tipo_recurso = PDF -> Descargar PDF
│   ├── Si tipo_recurso = Enlace -> Abrir enlace
│   ├── Si tipo_recurso = Archivo -> Descargar archivo
│   └── Si tipo_recurso = Repositorio -> Ver repositorio
├── Marcar como completada
│   ├── Guardar progreso de leccion
│   ├── Recalcular porcentaje de avance
│   └── Desbloquear siguiente leccion, si corresponde
└── Siguiente leccion
    ├── Si esta desbloqueada -> Leccion siguiente
    └── Si esta bloqueada -> Mostrar aviso

Examen final
├── Instrucciones
├── Tiempo limite
├── Intentos disponibles
├── Calificacion minima
├── Cantidad de preguntas
└── Iniciar examen
    ├── Si tiene intentos disponibles -> Presentar examen
    └── Si no tiene intentos disponibles -> Resultado del examen / Aviso

Presentar examen
├── Crear intento en estado en_progreso
├── Seleccionar preguntas aleatorias
├── Guardar preguntas asignadas en Intento_Pregunta
├── Mostrar opciones de respuesta
├── Temporizador
└── Enviar respuestas
    ├── Guardar opcion seleccionada por pregunta
    ├── Calcular calificacion
    ├── Cambiar intento a finalizado
    └── Resultado del examen

Resultado del examen
├── Si aprueba
│   ├── Verificar finalizacion del curso
│   ├── Registrar fecha de finalizacion
│   ├── Generar certificado con codigo visible
│   └── Ver certificado -> Mis certificados
├── Si reprueba y tiene intentos disponibles -> Reintentar examen
└── Si reprueba y no tiene intentos disponibles -> Permanecer en Resultado del examen

---

## 4. Zona del instructor

Escritorio del instructor
├── Resumen del instructor
├── Mis cursos creados
│   ├── Crear curso
│   ├── Editar curso
│   ├── Administrar curso
│   └── Enviar curso a revision
├── Gestionar modulos
├── Gestionar lecciones
├── Gestionar recursos
├── Configurar examen final
├── Crear preguntas
├── Consultar alumnos inscritos
├── Revisar progreso de alumnos
├── Revisar resultados de examenes
├── Mi cuenta
└── Cerrar sesion -> Inicio

---

## 5. Zona del administrador

Escritorio del administrador
├── Resumen general
├── Usuarios
│   ├── Consultar usuarios
│   ├── Activar usuario
│   ├── Desactivar usuario
│   └── Asignar rol
├── Solicitudes de instructor
│   ├── Ver solicitudes pendientes
│   ├── Ver detalle de solicitud
│   ├── Aceptar solicitud
│   │   ├── Cambiar solicitud a aceptada
│   │   └── Cambiar rol del usuario a Instructor
│   └── Rechazar solicitud
├── Revision de cursos
│   ├── Ver cursos pendientes de revision
│   ├── Aprobar curso
│   ├── Rechazar curso
│   └── Registrar comentario de revision
├── Ordenes y pagos
│   ├── Consultar ordenes
│   ├── Ver detalle de orden
│   ├── Ver cursos incluidos en Orden_Detalle
│   ├── Consultar pagos
│   └── Consultar webhooks
├── Inscripciones
│   └── Consultar inscripciones generadas por ordenes aprobadas
├── Mi cuenta
└── Cerrar sesion -> Inicio

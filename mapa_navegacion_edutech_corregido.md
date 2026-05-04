# EduTech - Mapa de navegación con flujo condicional

## Mapa de navegación con flujo condicional - EduTech

Versión corregida en base a requerimientos, actores, entidades y reglas de negocio del sistema.

---

# 1. Cómo leer este mapa

Este documento no es solo un mapa de navegación puro. Para EduTech conviene usar un **mapa de navegación con flujo condicional**, porque el sistema tiene:

- registro;
- inicio de sesión;
- roles;
- compra;
- orden;
- pago externo;
- webhook;
- inscripción automática;
- bloqueo de acceso;
- avance secuencial;
- examen con intentos;
- certificado;
- solicitud de cuenta de instructor;
- administración de usuarios, cursos, pagos e inscripciones.

| Concepto | Significado |
|---|---|
| Pantalla | Vista principal que el usuario ve. Ejemplo: Registro, Catálogo de cursos, Lección. |
| Sección | Parte interna de una pantalla. Ejemplo: Datos de contacto dentro de Compra del curso. |
| Botón / acción | Elemento que el usuario presiona o acción que ejecuta el sistema. Ejemplo: Pagar con PayPal, Marcar como completada. |
| Flujo condicional | Camino que depende de una condición. Ejemplo: si el pago fue aprobado va a Mis cursos; si fue rechazado vuelve a Compra del curso. |

**Frase guía:**  
El mapa definitivo no solo dice a qué pantalla vas; también muestra qué botón inicia el cambio y qué pasa cuando el usuario no cumple las condiciones.

---

# 2. Zona pública

```text
EduTech
│
├── Inicio
│   ├── Sección: Presentación de EduTech
│   ├── Sección: Cursos destacados
│   │   └── → Botón: Ver detalle → Detalle del curso
│   │
│   ├── → Botón: Ver cursos → Catálogo de cursos
│   ├── → Botón: Crear cuenta → Registro
│   ├── → Botón: Iniciar sesión → Inicio de sesión
│   └── → Botón: ¿Quieres ser instructor? → Información para solicitar cuenta de instructor
│
├── Catálogo de cursos
│   ├── Sección: Lista de cursos publicados
│   └── → Botón: Ver detalle → Detalle del curso
│
├── Detalle del curso
│   ├── Sección: Información del curso
│   ├── Sección: Instructor
│   ├── Sección: Nivel del curso
│   ├── Sección: Módulos y lecciones incluidas
│   ├── Sección: Precio en MXN
│   └── → Botón: Comprar curso
│       ├── → Si no ha iniciado sesión → Inicio de sesión / Registro
│       └── → Si inició sesión como Alumno → Compra del curso
│
├── Registro
│   ├── Sección: Formulario de registro
│   │   ├── Nombre
│   │   ├── Apellidos
│   │   ├── Correo
│   │   ├── Confirmación de correo
│   │   ├── Contraseña
│   │   └── Confirmación de contraseña
│   │
│   ├── Botón: Registrarse
│   │   ├── → Si los datos son correctos → Crear cuenta con rol Alumno
│   │   │   └── → Iniciar sesión automáticamente → Escritorio del alumno
│   │   └── → Si los datos son incorrectos → Permanecer en Registro y mostrar errores
│   │
│   └── → Botón: Ya tengo cuenta → Inicio de sesión
│
├── Inicio de sesión
│   ├── Sección: Formulario de acceso
│   │   ├── Correo
│   │   └── Contraseña
│   │
│   ├── Botón: Iniciar sesión
│   │   ├── → Si los datos son correctos y rol Alumno → Escritorio del alumno
│   │   ├── → Si los datos son correctos y rol Instructor → Escritorio del instructor
│   │   ├── → Si los datos son correctos y rol Administrador → Escritorio del administrador
│   │   ├── → Si el usuario está desactivado → Permanecer en Inicio de sesión y mostrar aviso
│   │   └── → Si los datos son incorrectos → Permanecer en Inicio de sesión y mostrar error
│   │
│   └── → Botón: Crear cuenta → Registro
│
├── ¿Quiénes somos?
│   ├── Sección: Qué es EduTech
│   ├── Sección: Misión
│   ├── Sección: Visión
│   └── Sección: Objetivo
│
├── Contacto
│   ├── Sección: Formulario de contacto
│   ├── Botón: Enviar mensaje
│   └── Botón: Solicitar cuenta de instructor
│       ├── → Si no ha iniciado sesión → Inicio de sesión / Registro
│       └── → Si inició sesión como Alumno → Solicitud de instructor
│
├── Información para solicitar cuenta de instructor
│   ├── Sección: Requisitos para solicitar cuenta de instructor
│   ├── Sección: Explicación del proceso de revisión
│   └── Botón: Solicitar cuenta de instructor
│       ├── → Si no ha iniciado sesión → Inicio de sesión / Registro
│       └── → Si inició sesión como Alumno → Solicitud de instructor
│
└── Mi cuenta
    ├── → Si no ha iniciado sesión → Inicio de sesión
    └── → Si ya inició sesión → Mi cuenta según rol
```

---

# 3. Flujo de compra, orden y pago

```text
Detalle del curso
│
└── Botón: Comprar curso
    ├── → Si no ha iniciado sesión → Inicio de sesión / Registro
    └── → Si inició sesión como Alumno → Compra del curso
```

```text
Compra del curso
│
├── Sección: Datos de contacto
│   ├── Nombre
│   ├── Apellidos
│   ├── Correo
│   └── Teléfono
│
├── Sección opcional: Datos de facturación
│   ├── Dirección
│   ├── Ciudad
│   ├── Estado
│   └── Código postal
│
├── Sección: Resumen del pedido
│   ├── Curso seleccionado
│   ├── Precio actual del curso
│   ├── Total de la orden
│   └── Método de pago
│
├── Acción automática: Crear orden pendiente
│   ├── Crear número de orden visible
│   └── Guardar total de la orden
│
└── Botón: Pagar con PayPal / Stripe Sandbox
    ↓
Pasarela externa
    ↓
Webhook hacia EduTech
    ↓
EduTech valida pago
    ├── Validar alumno
    ├── Validar curso
    ├── Validar orden
    ├── Validar monto
    └── Validar moneda
    ↓
Confirmación de compra
```

```text
Confirmación de compra
│
├── Si pago aprobado
│   ├── Mensaje: Compra aprobada
│   ├── Acción automática: Actualizar pago a aprobado
│   ├── Acción automática: Cambiar orden a completada
│   ├── Acción automática: Crear inscripción activa
│   ├── Acción automática: Liberar acceso al curso
│   └── → Botón: Ir a Mis cursos → Mis cursos
│
├── Si pago pendiente
│   ├── Mensaje: Pago pendiente
│   ├── Orden permanece pendiente
│   └── → Botón: Ver historial de pedidos → Historial de pedidos
│
├── Si pago rechazado
│   ├── Mensaje: Pago rechazado
│   ├── Acción automática: Cambiar pago a rechazado
│   ├── Acción automática: Cambiar orden a fallida
│   └── → Botón: Intentar de nuevo → Compra del curso
│
├── Si pago cancelado
│   ├── Mensaje: Pago cancelado
│   ├── Acción automática: Cambiar pago a cancelado
│   ├── Acción automática: Cambiar orden a cancelada
│   └── → Botón: Volver a compra → Compra del curso
│
└── Si la orden queda sin pago durante demasiado tiempo
    ├── Acción automática: Cambiar orden a expirada
    └── → Botón: Ver historial de pedidos → Historial de pedidos
```

---

# 4. Zona del alumno

```text
Escritorio del alumno
│
├── Sección: Resumen del alumno
│   ├── Cursos comprados
│   ├── Cursos en progreso
│   ├── Pedidos pendientes
│   └── Certificados obtenidos
│
├── Botón / menú: Mis cursos
│   ↓
│   Mis cursos
│   ├── Sección: Cursos con pago aprobado e inscripción activa
│   ├── → Botón: Continuar curso → Vista del curso comprado
│   └── → Si intenta ver curso sin compra aprobada → Acceso denegado / Historial de pedidos
│
├── Botón / menú: Historial de pedidos
│   ↓
│   Historial de pedidos
│   ├── Sección: Lista de órdenes
│   │   ├── Número de orden
│   │   ├── Curso
│   │   ├── Fecha
│   │   ├── Total
│   │   ├── Estado de orden
│   │   └── Estado de pago, si existe
│   │
│   └── → Botón: Ver detalle → Detalle del pedido
│
├── Botón / menú: Mis calificaciones
│   ↓
│   Mis calificaciones
│   └── Sección: Resultados de exámenes
│
├── Botón / menú: Mis certificados
│   ↓
│   Mis certificados
│   ├── Sección: Lista de certificados
│   └── → Botón: Ver certificado → Ver certificado
│
├── Botón / menú: Solicitar cuenta de instructor
│   ↓
│   Solicitud de instructor
│
├── Botón / menú: Mi cuenta
│   ↓
│   Mi cuenta
│
└── Botón: Cerrar sesión
    └── → Inicio
```

---

## 4.1 Vista del curso comprado

```text
Vista del curso comprado / Aula virtual
│
├── Sección: Información del curso
│   ├── Nombre del curso
│   ├── Instructor
│   ├── Nivel
│   └── Descripción
│
├── Sección: Progreso del curso
│   ├── Lecciones completadas
│   └── Porcentaje de avance
│
├── Sección: Módulos
│   └── Sección: Lecciones
│       ├── Lección completada
│       ├── Lección desbloqueada
│       └── Lección bloqueada
│
├── Botón: Abrir lección
│   ├── → Si la lección está desbloqueada → Lección
│   └── → Si la lección está bloqueada → Permanecer en Vista del curso y mostrar aviso
│
└── Botón: Ir al examen final
    ├── → Si completó las lecciones requeridas → Examen final
    └── → Si no completó las lecciones → Permanecer en Vista del curso y mostrar aviso
```

---

## 4.2 Lección

```text
Lección
│
├── Sección: Título
├── Sección: Video
│   ├── Si tipo_video = YouTube → Mostrar video embebido de YouTube
│   ├── Si tipo_video = Vimeo → Mostrar video embebido de Vimeo
│   └── Si tipo_video = Local → Reproducir video local optimizado
│
├── Sección: Texto descriptivo
│
├── Sección: Recursos adicionales
│   ├── Si tipo_recurso = PDF → Botón: Descargar PDF
│   ├── Si tipo_recurso = Enlace → Botón: Abrir enlace
│   ├── Si tipo_recurso = Archivo → Botón: Descargar archivo
│   └── Si tipo_recurso = Repositorio → Botón: Ver repositorio
│
├── Botón: Marcar como completada
│   ├── Acción: Guardar progreso de lección
│   ├── Acción: Recalcular porcentaje de avance
│   └── Acción: Desbloquear siguiente lección, si corresponde
│
└── Botón: Siguiente lección
    ├── → Si está desbloqueada → Lección siguiente
    └── → Si está bloqueada → Mostrar aviso
```

---

## 4.3 Examen final

```text
Examen final
│
├── Sección: Instrucciones
│   ├── Tiempo límite
│   ├── Intentos disponibles
│   ├── Calificación mínima
│   └── Cantidad de preguntas
│
└── Botón: Iniciar examen
    ├── → Si tiene intentos disponibles → Presentar examen
    └── → Si no tiene intentos disponibles → Resultado del examen / Aviso de intentos agotados
```

```text
Presentar examen
│
├── Acción automática: Crear intento en estado en_progreso
│
├── Sección: Preguntas aleatorias
├── Sección: Opciones de respuesta
├── Sección: Temporizador
│
├── Botón: Siguiente pregunta
│
└── Botón: Enviar respuestas
    ├── Acción: Guardar respuestas del alumno
    ├── Acción: Calcular calificación
    ├── Acción: Cambiar intento a finalizado
    └── → Resultado del examen
```

```text
Resultado del examen
│
├── Si aprueba
│   ├── Mensaje: Examen aprobado
│   ├── Acción: Verificar finalización del curso
│   ├── Acción: Registrar fecha de finalización
│   ├── Acción: Generar certificado con código visible
│   └── → Botón: Ver certificado → Mis certificados
│
├── Si reprueba y tiene intentos disponibles
│   ├── Mensaje: Examen no aprobado
│   └── → Botón: Reintentar examen → Examen final
│
└── Si reprueba y no tiene intentos disponibles
    ├── Mensaje: Intentos agotados
    └── Permanecer en Resultado del examen
```

---

## 4.4 Historial de pedidos

```text
Historial de pedidos
│
├── Sección: Lista de órdenes
│   ├── Número de orden
│   ├── Curso
│   ├── Fecha
│   ├── Total
│   ├── Estado de orden
│   └── Estado de pago
│
└── Botón: Ver detalle
    ↓
Detalle del pedido
│
├── Sección: Número de orden
├── Sección: Curso comprado
├── Sección: Fecha de creación
├── Sección: Total
├── Sección: Moneda
├── Sección: Estado de orden
├── Sección: Estado de pago, si existe
└── Sección: Proveedor de pago
```

---

## 4.5 Certificado

```text
Mis certificados
│
├── Sección: Lista de certificados
│   ├── Curso
│   ├── Fecha de emisión
│   └── Código de certificado
│
└── Botón: Ver certificado
    ↓
Ver certificado
│
├── Sección: Nombre del alumno
├── Sección: Curso
├── Sección: Fecha de finalización
├── Sección: Fecha de emisión
├── Sección: Código de certificado
└── Botón: Descargar / visualizar certificado
```

---

## 4.6 Mi cuenta

```text
Mi cuenta
│
├── Sección: Ver datos personales
│   ├── Nombre
│   ├── Apellidos
│   ├── Correo
│   └── Teléfono
│
├── Botón: Editar datos
├── Botón: Cambiar contraseña
│
└── Botón: Guardar cambios
    ├── → Si los datos son correctos → Permanecer en Mi cuenta y mostrar “datos actualizados”
    └── → Si los datos son incorrectos → Permanecer en Mi cuenta y mostrar errores
```

**Regla:**  
El usuario puede editar datos personales permitidos, pero no puede cambiar directamente su rol.

---

## 4.7 Solicitud de instructor

```text
Solicitud de instructor
│
├── Sección: Formulario de solicitud
│   ├── Área de experiencia
│   ├── Experiencia
│   ├── Enlace a portafolio, CV, LinkedIn, GitHub u otra evidencia
│   └── Motivo de solicitud
│
└── Botón: Enviar solicitud
    ├── → Si los datos son correctos
    │   ├── Acción automática: Crear solicitud en estado pendiente
    │   └── → Mensaje: Solicitud enviada para revisión
    │
    └── → Si los datos son incorrectos
        └── Permanecer en Solicitud de instructor y mostrar errores
```

---

# 5. Zona del instructor

```text
Escritorio del instructor
│
├── Sección: Resumen del instructor
│   ├── Cursos creados
│   ├── Cursos publicados
│   ├── Cursos en borrador
│   ├── Cursos pendientes de revisión
│   ├── Alumnos inscritos
│   └── Exámenes activos
│
├── Botón / menú: Mis cursos creados
│   ↓
│   Mis cursos creados
│   ├── Sección: Lista de cursos propios
│   ├── → Botón: Crear curso → Crear curso
│   ├── → Botón: Editar curso → Editar curso
│   └── → Botón: Administrar curso → Administrar curso
│
├── Botón / menú: Mi cuenta
│   ↓
│   Mi cuenta
│
└── Botón: Cerrar sesión
    └── → Inicio
```

---

## 5.1 Crear curso

```text
Crear curso
│
├── Formulario: Título
├── Formulario: Descripción
├── Formulario: Portada
├── Formulario: Nivel
├── Formulario: Precio en MXN
│
└── Botón: Guardar curso
    ├── → Si los datos son correctos
    │   ├── Acción automática: Crear curso en estado borrador
    │   └── → Administrar curso
    │
    └── → Si los datos son incorrectos
        └── Permanecer en Crear curso y mostrar errores
```

---

## 5.2 Editar curso

```text
Editar curso
│
├── Formulario: Datos actuales del curso
│   ├── Título
│   ├── Descripción
│   ├── Portada
│   ├── Nivel
│   └── Precio en MXN
│
└── Botón: Actualizar curso
    ├── → Si los datos son correctos → Administrar curso
    └── → Si los datos son incorrectos → Permanecer en Editar curso y mostrar errores
```

---

## 5.3 Administrar curso

```text
Administrar curso
│
├── Sección: Datos del curso
│   ├── Estado del curso
│   ├── Título
│   ├── Descripción
│   ├── Nivel
│   └── Precio
│
├── Botón: Editar datos generales → Editar curso
│
├── Botón: Enviar curso a revisión
│   ├── → Si el curso tiene datos mínimos, módulos, lecciones y examen configurado
│   │   ├── Acción: Cambiar estado a pendiente_revision
│   │   └── Mostrar mensaje: Curso enviado a revisión
│   │
│   └── → Si falta información
│       └── Permanecer en Administrar curso y mostrar errores
│
├── Sección: Módulos y lecciones
│   ├── Lista de módulos
│   ├── Lista de lecciones por módulo
│   ├── Botón: Crear módulo
│   │   ├── → Si se guarda correctamente → Actualizar lista de módulos
│   │   └── → Si falla → Mostrar errores
│   │
│   ├── Botón: Editar módulo
│   ├── Botón: Ordenar módulos
│   ├── Botón: Crear lección
│   ├── Botón: Editar lección
│   │
│   └── Sección: Contenido de lección
│       ├── Texto descriptivo
│       ├── Video
│       │   ├── YouTube
│       │   ├── Vimeo
│       │   └── Local
│       └── Recursos adicionales
│           ├── PDF
│           ├── Enlace
│           ├── Archivo
│           └── Repositorio
│
├── Sección: Examen final y banco de preguntas
│   ├── Configurar examen final
│   │   ├── Título
│   │   ├── Descripción
│   │   ├── Tiempo límite
│   │   ├── Número de intentos
│   │   ├── Calificación mínima
│   │   └── Cantidad de preguntas
│   │
│   ├── Banco de preguntas
│   │   ├── Botón: Crear pregunta
│   │   ├── Botón: Editar pregunta
│   │   ├── Botón: Crear / editar opciones
│   │   └── Acción: Definir respuesta correcta
│
├── Sección: Alumnos inscritos
│   └── Tabla de alumnos inscritos al curso
│
├── Sección: Progreso de alumnos
│   └── Tabla con avance por alumno
│
└── Sección: Resultados del examen
    ├── Tabla de calificaciones
    └── Botón: Ver detalle del resultado → Detalle del resultado
```

---

# 6. Zona del administrador

```text
Escritorio del administrador
│
├── Sección: Resumen general
│   ├── Usuarios registrados
│   ├── Cursos publicados
│   ├── Cursos pendientes de revisión
│   ├── Pagos aprobados
│   ├── Pagos pendientes
│   ├── Inscripciones activas
│   └── Solicitudes de instructor pendientes
│
├── Botón / menú: Gestión de usuarios
│   ↓
│   Gestión de usuarios
│
├── Botón / menú: Solicitudes de instructor
│   ↓
│   Solicitudes de instructor
│
├── Botón / menú: Gestión de cursos
│   ↓
│   Gestión de cursos
│
├── Botón / menú: Órdenes y pagos
│   ↓
│   Órdenes y pagos
│
├── Botón / menú: Inscripciones
│   ↓
│   Inscripciones
│
├── Botón / menú: Configuración de pasarela
│   ↓
│   Configuración de pasarela
│
├── Botón / menú: Seguridad y acceso
│   ↓
│   Seguridad y acceso
│
├── Botón / menú: Mi cuenta
│   ↓
│   Mi cuenta
│
└── Botón: Cerrar sesión
    └── → Inicio
```

---

## 6.1 Gestión de usuarios

```text
Gestión de usuarios
│
├── Sección: Lista de usuarios
│   ├── Nombre
│   ├── Correo
│   ├── Rol
│   └── Estado de cuenta
│
├── Botón: Activar usuario
│   └── Acción: Cambiar esta_activo a true
│
├── Botón: Desactivar usuario
│   └── Acción: Cambiar esta_activo a false sin eliminar historial
│
└── Botón: Asignar rol
    ├── → Si corresponde → Cambiar rol del usuario
    └── → Si no corresponde → Mostrar aviso
```

**Nota:**  
El administrador no necesita editar datos personales del usuario como tarea normal. El usuario edita su propia información desde “Mi cuenta”.

---

## 6.2 Solicitudes de instructor

```text
Solicitudes de instructor
│
├── Sección: Lista de solicitudes
│   ├── Alumno solicitante
│   ├── Área de experiencia
│   ├── Fecha de solicitud
│   └── Estado de solicitud
│
└── Botón: Ver detalle de solicitud
    ↓
Detalle de solicitud de instructor
│
├── Sección: Datos del alumno
├── Sección: Experiencia
├── Sección: Portafolio / evidencia
├── Sección: Motivo de solicitud
│
├── Botón: Aceptar solicitud
│   ├── Acción automática: Cambiar solicitud a aceptada
│   ├── Acción automática: Cambiar rol del usuario a Instructor
│   └── Mostrar mensaje: Solicitud aceptada
│
└── Botón: Rechazar solicitud
    ├── Acción automática: Cambiar solicitud a rechazada
    └── Mostrar mensaje: Solicitud rechazada
```

---

## 6.3 Gestión de cursos

```text
Gestión de cursos
│
├── Sección: Lista de todos los cursos
│   ├── Título
│   ├── Instructor
│   ├── Estado
│   ├── Precio
│   └── Fecha de creación
│
└── Botón: Ver detalle administrativo del curso
    ↓
Detalle administrativo del curso
│
├── Sección: Datos generales del curso
├── Sección: Instructor
├── Sección: Módulos y lecciones
├── Sección: Examen final
├── Sección: Alumnos inscritos
│
├── Botón: Publicar curso
│   ├── Acción automática: Cambiar estado a publicado
│   └── Resultado: El curso aparece en el catálogo
│
└── Botón: Despublicar curso
    ├── Acción automática: Cambiar estado a no_publicado
    └── Resultado: El curso ya no aparece para nuevas compras,
        pero los alumnos que ya lo compraron conservan acceso
```

---

## 6.4 Órdenes y pagos

```text
Órdenes y pagos
│
├── Sección: Lista de órdenes
│   ├── Número de orden
│   ├── Alumno
│   ├── Curso
│   ├── Total
│   ├── Moneda
│   └── Estado de orden
│
├── Sección: Lista de pagos
│   ├── Proveedor
│   ├── Estado de pago
│   ├── Monto pagado
│   ├── Moneda
│   └── ID externo de pago
│
├── Sección: Webhooks recibidos
│   ├── Tipo de evento
│   ├── ID externo del evento
│   ├── Estado de procesamiento
│   └── Fecha de recepción
│
└── Botón: Ver detalle de orden / pago
    ↓
Detalle de orden / pago
│
├── Sección: Datos de la orden
├── Sección: Datos del pago
├── Sección: Datos del webhook
└── Sección: Validación de monto, moneda, alumno, curso y orden
```

---

## 6.5 Inscripciones

```text
Inscripciones
│
├── Sección: Lista de inscripciones
│   ├── Alumno
│   ├── Curso
│   ├── Orden
│   ├── Estado de inscripción
│   ├── Fecha de inscripción
│   └── Fecha de finalización
│
└── Botón: Ver detalle de inscripción
    ↓
Detalle de inscripción
│
├── Sección: Alumno
├── Sección: Curso
├── Sección: Orden relacionada
├── Sección: Estado de inscripción
├── Sección: Progreso del alumno
│
└── Botón: Cancelar inscripción
    ├── → Solo en casos excepcionales
    ├── Acción: Cambiar estado de inscripción a cancelada
    └── Motivos válidos:
        ├── Reembolso
        ├── Pago fraudulento
        ├── Error administrativo
        └── Validación incorrecta del pago
```

---

## 6.6 Configuración de pasarela

```text
Configuración de pasarela
│
├── Sección: PayPal Sandbox
├── Sección: Stripe Sandbox
├── Sección: Credenciales de prueba
└── Sección: URL de webhook
```

---

## 6.7 Seguridad y acceso

```text
Seguridad y acceso
│
├── Sección: Reglas por rol
│   ├── Alumno
│   ├── Instructor
│   └── Administrador
│
├── Sección: Protección de cursos
├── Sección: Protección de lecciones
├── Sección: Protección de exámenes
├── Sección: Protección contra URL directa
└── Sección: Revisión de accesos indebidos
```

---

# 7. Flujo resumido de estados importantes

## 7.1 Curso

```text
Instructor crea curso
↓
Curso queda en borrador
↓
Instructor envía a revisión
↓
Curso queda en pendiente_revision
↓
Administrador revisa
├── Si aprueba → publicado
└── Si no aprueba → permanece en pendiente_revision o vuelve a borrador
```

---

## 7.2 Orden y pago

```text
Alumno inicia compra
↓
Orden pendiente
↓
Alumno paga con PayPal / Stripe
↓
Pago pendiente
↓
Webhook confirma resultado
├── Pago aprobado → Orden completada → Crear inscripción activa → Liberar curso
├── Pago rechazado → Orden fallida → No liberar curso
├── Pago cancelado → Orden cancelada → No liberar curso
└── Sin confirmación por mucho tiempo → Orden expirada → No liberar curso
```

---

## 7.3 Inscripción

```text
Pago aprobado + Orden completada
↓
Crear inscripción activa
↓
Alumno accede al curso
↓
Alumno completa lecciones + aprueba examen
↓
Inscripción completada
↓
Generar certificado
```

---

## 7.4 Solicitud de instructor

```text
Alumno autenticado solicita ser instructor
↓
Solicitud pendiente
↓
Administrador revisa
├── Si acepta → Solicitud aceptada → Usuario cambia a rol Instructor
└── Si rechaza → Solicitud rechazada → Usuario conserva rol Alumno
```

---

## 7.5 Examen

```text
Alumno inicia examen
↓
Intento en_progreso
↓
Alumno envía respuestas
↓
Intento finalizado
├── Si aprueba → verificar finalización del curso
├── Si reprueba y tiene intentos → permitir reintento
└── Si reprueba y no tiene intentos → mostrar intentos agotados
```

---

# 8. Indicaciones para revisar el mapa

La entrega debe presentarse como **mapa de navegación con flujo condicional**.

No debe limitarse a poner nombres de pantallas. También debe indicar:

- qué botón lleva a cada pantalla;
- qué pasa si el usuario no inició sesión;
- qué pasa si el pago queda aprobado, pendiente, rechazado o cancelado;
- qué pasa si la lección está bloqueada;
- qué pasa si el examen no tiene intentos disponibles;
- qué pasa si el curso se despublica;
- qué pasa si una solicitud de instructor se acepta o rechaza.

---

# 9. Preguntas de validación para quien haga el mapa

1. ¿Qué pantallas son públicas y cuáles requieren iniciar sesión?
2. ¿Qué botón lleva desde Inicio al Catálogo de cursos?
3. ¿Qué botón lleva desde Catálogo al Detalle del curso?
4. ¿Qué pasa si un visitante presiona “Comprar curso”?
5. ¿Qué pasa si un alumno autenticado presiona “Comprar curso”?
6. ¿Dónde se crea la orden pendiente?
7. ¿Dónde entra PayPal o Stripe?
8. ¿Qué pasa si el pago es aprobado?
9. ¿Qué pasa si el pago es rechazado?
10. ¿Qué pasa si la orden queda sin pago confirmado?
11. ¿Dónde aparece un curso comprado correctamente?
12. ¿Dónde aparece una orden pendiente?
13. ¿Qué pantalla muestra el progreso del curso?
14. ¿Qué pasa si una lección está bloqueada?
15. ¿Qué pasa cuando el alumno completa una lección?
16. ¿Qué pasa si el alumno intenta abrir el examen sin completar las lecciones?
17. ¿Qué pasa si el alumno aprueba el examen?
18. ¿Qué pasa si el alumno reprueba y todavía tiene intentos?
19. ¿Qué pasa si el alumno reprueba y ya no tiene intentos?
20. ¿Dónde se genera el certificado?
21. ¿Dónde se muestra el código del certificado?
22. ¿Cómo solicita un alumno ser instructor?
23. ¿Quién revisa la solicitud de instructor?
24. ¿Qué pasa si la solicitud de instructor es aceptada?
25. ¿Qué pasa si la solicitud de instructor es rechazada?
26. ¿Qué puede hacer el instructor solo con sus cursos?
27. ¿Qué pasa cuando el instructor crea un curso?
28. ¿Qué pasa cuando el instructor envía un curso a revisión?
29. ¿Qué pasa cuando el administrador publica un curso?
30. ¿Qué pasa cuando el administrador despublica un curso?
31. ¿Despublicar un curso quita acceso a alumnos que ya lo compraron?
32. ¿Dónde puede el administrador consultar órdenes, pagos y webhooks?
33. ¿Dónde puede el administrador consultar inscripciones?
34. ¿En qué casos excepcionales se puede cancelar una inscripción?
35. ¿Dónde edita sus datos un usuario?

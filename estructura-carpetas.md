# Estructura final de carpetas del proyecto EduTech

Este documento propone la estructura completa del proyecto cuando EduTech avance a una version funcional con frontend, backend, base de datos, documentacion y recursos.

No representa solamente lo que existe hoy en el repositorio. Representa la organizacion final recomendada.

---

## Estructura general

EduTech/  
├── README.md  
├── docs/  
├── frontend/  
├── backend/  
├── database/  
├── scripts/  
└── .gitignore  

---

## Carpeta docs

EduTech/  
└── docs/  
├── sprint-1.md  
├── actores.md  
├── acciones-actores.md  
├── requerimientos.md  
├── mapa-navegacion.md  
├── pantallas.md  
├── wireframes.md  
├── modelo-base-datos.md  
├── tipos-datos-edutech.md  
├── diagrama-relacional.drawio  
├── diagrama-relacional.pdf  
├── tecnologias.md  
├── estructura-carpetas.md  
├── frontend-visual.md  
├── base-datos.md  
└── seguimiento-entregas.md  

La carpeta docs guarda la documentacion del proyecto y del Sprint 1.

---

## Carpeta frontend

EduTech/  
└── frontend/  
├── index.html  
├── cursos.html  
├── detalle-curso.html  
├── registro.html  
├── login.html  
├── contacto.html  
├── solicitud-instructor.html  
├── comprar-curso.html  
├── compra-aprobada.html  
├── mis-cursos.html  
├── css/  
│   └── styles.css  
├── js/  
│   └── main.js  
└── assets/  
    └── img/  
        ├── banner-cursos-edutech.svg  
        ├── curso-hackeo.jpg  
        ├── curso-ia.jpg  
        ├── curso-canto.jpg  
        └── vegeta-traje.png  

La carpeta frontend guarda las pantallas HTML, estilos, JavaScript e imagenes del prototipo visual.

---

## Carpeta backend

EduTech/  
└── backend/  
├── package.json  
├── .env.example  
├── src/  
│   ├── app.js  
│   ├── server.js  
│   ├── config/  
│   │   ├── database.js  
│   │   └── env.js  
│   ├── routes/  
│   │   ├── auth.routes.js  
│   │   ├── cursos.routes.js  
│   │   ├── ordenes.routes.js  
│   │   ├── pagos.routes.js  
│   │   ├── inscripciones.routes.js  
│   │   ├── examenes.routes.js  
│   │   └── usuarios.routes.js  
│   ├── controllers/  
│   │   ├── auth.controller.js  
│   │   ├── cursos.controller.js  
│   │   ├── ordenes.controller.js  
│   │   ├── pagos.controller.js  
│   │   ├── inscripciones.controller.js  
│   │   ├── examenes.controller.js  
│   │   └── usuarios.controller.js  
│   ├── services/  
│   │   ├── auth.service.js  
│   │   ├── cursos.service.js  
│   │   ├── ordenes.service.js  
│   │   ├── pagos.service.js  
│   │   ├── inscripciones.service.js  
│   │   └── examenes.service.js  
│   ├── repositories/  
│   │   ├── usuario.repository.js  
│   │   ├── curso.repository.js  
│   │   ├── orden.repository.js  
│   │   ├── pago.repository.js  
│   │   └── inscripcion.repository.js  
│   ├── middlewares/  
│   │   ├── auth.middleware.js  
│   │   ├── role.middleware.js  
│   │   └── error.middleware.js  
│   ├── validators/  
│   │   ├── auth.validator.js  
│   │   ├── curso.validator.js  
│   │   └── orden.validator.js  
│   └── utils/  
│       ├── password.js  
│       ├── order-number.js  
│       └── certificate-code.js  
└── uploads/  
    ├── cursos/  
    └── certificados/  

La carpeta backend guarda la API, reglas de negocio, control de acceso, pagos, webhooks, inscripciones, examenes y certificados.

---

## Carpeta database

EduTech/  
└── database/  
├── migrations/  
│   ├── 001_create_catalogos.sql  
│   ├── 002_create_usuarios.sql  
│   ├── 003_create_cursos.sql  
│   ├── 004_create_ordenes_pagos.sql  
│   ├── 005_create_inscripciones.sql  
│   ├── 006_create_examenes.sql  
│   └── 007_create_certificados.sql  
├── seeds/  
│   ├── catalogos.sql  
│   └── usuarios_demo.sql  
└── schema.sql  

La carpeta database guarda scripts SQL, migraciones y datos iniciales.

---

## Carpeta scripts

EduTech/  
└── scripts/  
├── reset-db.sh  
├── seed-db.sh  
└── backup-db.sh  

La carpeta scripts guarda comandos auxiliares para desarrollo.

---

## Criterio de organizacion

| Carpeta | Uso |
|---|---|
| docs | Documentacion del proyecto |
| frontend | Pantallas HTML, CSS, JS e imagenes |
| backend | API, controladores, servicios y reglas de negocio |
| database | Scripts SQL, migraciones y datos iniciales |
| scripts | Automatizaciones de desarrollo |

---

## Frase para recordar

Frontend muestra la interfaz, backend aplica las reglas del sistema, database guarda la informacion y docs explica el proyecto.

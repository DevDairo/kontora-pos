# DEC-002 - Tecnologías seleccionadas

## Estado

Aceptada.

## Contexto

Kontora POS será una aplicación web para gestionar ventas, caja diaria, inventario, depósito, evidencias, usuarios, roles y auditoría.

El sistema requiere una arquitectura mantenible, separada por capas, con backend transaccional, frontend web, base de datos relacional, almacenamiento externo de evidencias, control de versiones y despliegue reproducible.

También se debe considerar que el proyecto tiene fines académicos y técnicos, por lo que las tecnologías deben facilitar el aprendizaje progresivo, la documentación, las pruebas y el despliegue controlado.

## Decisión

Se selecciona el siguiente stack tecnológico principal:

| Área | Tecnología | Uso principal |
|---|---|---|
| Control de versiones | Git | Versionamiento local del proyecto |
| Repositorio remoto | GitHub | Fuente única de verdad, ramas y Pull Requests |
| Documentación | Markdown | README, decisiones técnicas, guías y documentación por módulo |
| Contenedores | Docker | Ejecución reproducible de servicios |
| Orquestación local | Docker Compose | Definición y levantamiento de infraestructura local |
| Base de datos local | PostgreSQL en Docker | Desarrollo y validación inicial |
| Administración BD local | pgAdmin | Consulta visual de PostgreSQL local |
| Backend | Java + Spring Boot | API REST, reglas de negocio, seguridad y transacciones |
| Seguridad backend | Spring Security + JWT | Autenticación, autorización y control de sesiones |
| Persistencia backend | Spring Data JPA / Hibernate | Mapeo entre entidades Java y tablas PostgreSQL |
| Migraciones | Flyway | Versionamiento de cambios en base de datos |
| Base de datos gestionada | Supabase PostgreSQL | Persistencia relacional en entorno gestionado |
| Almacenamiento de evidencias | Supabase Storage | Conservación de comprobantes e imágenes |
| Frontend | React + TypeScript + Vite | Interfaz web modular, tipada y mantenible |
| Despliegue frontend | Vercel | Publicación del frontend web |
| Servidor backend | VM Ubuntu Server | Ejecución del backend Dockerizado |
| Exposición API | Cloudflare Tunnel | Publicación segura de la API sin exponer directamente puertos internos |
| DNS | Cloudflare DNS | Gestión del dominio y subdominio de API |
| Pruebas backend | JUnit 5 y Mockito | Validación de servicios y reglas de negocio |
| Documentación API | OpenAPI / Swagger | Consulta y prueba de endpoints |

## Justificación

### Docker y Docker Compose

Docker permite ejecutar servicios de forma reproducible sin depender de configuraciones manuales del sistema operativo. Docker Compose permite declarar servicios, puertos, variables, redes y volúmenes en un archivo versionable.

En la Fase 1 se usa Docker Compose para levantar PostgreSQL y pgAdmin antes de desarrollar lógica funcional.

### PostgreSQL

PostgreSQL se selecciona porque Kontora POS requiere relaciones fuertes, integridad referencial, trazabilidad histórica y operaciones transaccionales.

El entorno local usará PostgreSQL en Docker para desarrollo. El entorno gestionado usará Supabase PostgreSQL.

### pgAdmin

pgAdmin se utiliza como herramienta visual para inspeccionar la base de datos local durante el desarrollo. Su correo de acceso no forma parte del modelo de usuarios de Kontora POS.

### Java y Spring Boot

Spring Boot permite construir una API REST modular y robusta. Es adecuado para centralizar reglas críticas como:

- Apertura y cierre de caja.
- Registro y anulación de ventas.
- Aplicación de promociones.
- Descuento y restauración de inventario.
- Validación de transferencias.
- Cálculo de depósito.
- Auditoría de operaciones sensibles.

### Spring Security y JWT

Spring Security permitirá controlar autenticación y autorización. JWT se usará para sesiones autenticadas, complementado con control persistido de sesiones cuando el diseño de seguridad lo requiera.

El inicio de sesión de Kontora POS se realizará con nombre de usuario alfanumérico y contraseña. No dependerá de correo electrónico ni de Supabase Auth.

### Spring Data JPA / Hibernate

JPA e Hibernate facilitan el mapeo entre entidades Java y tablas PostgreSQL. Esto permite trabajar con un modelo orientado a objetos sin perder la estructura relacional de la base de datos.

### Flyway

Flyway se usará para controlar los cambios de base de datos mediante migraciones versionadas. Esto evita depender de cambios manuales no documentados y facilita reproducir el esquema en otros entornos.

### React, TypeScript y Vite

React permitirá construir interfaces por componentes. TypeScript reducirá errores al manejar estructuras como ventas, pagos, cajas, usuarios, inventario y evidencias. Vite facilitará el desarrollo frontend y la generación del build.

### Supabase PostgreSQL y Supabase Storage

Supabase PostgreSQL se usará como base de datos gestionada en la arquitectura final. Supabase Storage se usará para almacenar evidencias como comprobantes de transferencias, gastos, consignaciones y pagos de servicios.

La base de datos almacenará metadatos y rutas de archivos, no binarios pesados.

### Vercel

Vercel se usará para desplegar el frontend React de forma sencilla y conectada al repositorio GitHub.

### VM Ubuntu Server

El backend se ejecutará en una VM Ubuntu Server mediante Docker. Esto permite controlar el entorno de ejecución del backend y mantener separación frente al frontend y la base de datos gestionada.

### Cloudflare Tunnel y Cloudflare DNS

Cloudflare Tunnel permitirá exponer la API del backend de forma segura sin abrir directamente puertos internos de la VM. Cloudflare DNS permitirá configurar dominio y subdominio para separar frontend y API.

## Consecuencias positivas

- Separación clara entre frontend, backend, base de datos, almacenamiento e infraestructura.
- Entorno local reproducible.
- Mayor control sobre reglas críticas desde el backend.
- Mejor trazabilidad mediante Git, Pull Requests, migraciones y documentación.
- Facilidad para trabajar por fases.
- Stack alineado con buenas prácticas académicas e industriales.
- Posibilidad de probar localmente antes de desplegar.

## Consecuencias negativas o costos

- Requiere aprender varias tecnologías de forma ordenada.
- Spring Boot, Docker y PostgreSQL exigen configuración inicial cuidadosa.
- El despliegue completo tendrá varios componentes integrados.
- Será necesario documentar variables de entorno, comandos y decisiones técnicas.
- Las migraciones de base de datos deberán mantenerse estrictamente versionadas.

## Aplicación en Kontora POS

Esta decisión se aplicará desde la Fase 1 de la siguiente manera:

- Docker Compose levantará PostgreSQL y pgAdmin.
- Los archivos `.env.example` documentarán variables sin exponer secretos.
- Git y GitHub controlarán los cambios.
- La documentación se mantendrá en Markdown.
- El backend y frontend se crearán en fases posteriores, respetando la arquitectura definida.

## Criterio de validación

Esta decisión se considera aplicada cuando:

- El repositorio contiene estructura base separada para backend, frontend, infraestructura, base de datos y documentación.
- Docker Compose levanta correctamente los servicios locales.
- PostgreSQL local queda accesible por `psql` y pgAdmin.
- Los archivos `.env` reales están ignorados por Git.
- Las variables necesarias están documentadas en `.env.example`.
- Las futuras fases respetan el stack definido.
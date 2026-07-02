# Fase 2 - Infraestructura Base y Estructura del Proyecto

## Objetivo de la fase

Preparar la base tecnica del proyecto Kontora POS despues de la configuracion inicial de Docker realizada en la Fase 1.

Esta fase deja listo el backend Spring Boot, la estructura inicial de paquetes, la ubicacion de scripts auxiliares, las variables de entorno de ejemplo y la documentacion tecnica minima para continuar con el desarrollo modular.

## Orden secuencial

La Fase 2 se organiza en los siguientes pasos. Cada paso debe cerrarse antes de avanzar al siguiente:

| Paso | Estado | Documento principal |
|---|---|---|
| 1. Inicializar backend Spring Boot | Completado | `docs/modules/backend.md` |
| 2. Crear estructura base de paquetes | Completado | `docs/modules/backend.md` |
| 3. Preparar carpetas tecnicas | Completado | `docs/architecture/infraestructura.md` |
| 4. Crear indice de la Fase 2 | Completado | `docs/architecture/fase-2-infraestructura-base.md` |
| 5. Organizar script SQL existente | Completado | `docs/architecture/base-datos.md` |
| 6. Documentar variables de entorno | Completado | `docs/architecture/variables-entorno.md` |
| 7. Crear plantilla de Pull Request | Completado | `docs/architecture/pull-request.md` |
| 8. Actualizar cierre documental de Fase 2 | Completado | `docs/architecture/fase-2-infraestructura-base.md` |

## Documentos de la fase

| Documento | Responsabilidad |
|---|---|
| `docs/modules/backend.md` | Explica la inicializacion del backend, la configuracion base y la estructura de paquetes. |
| `docs/architecture/infraestructura.md` | Explica las carpetas raiz para scripts, assets y Cloudflare. |
| `docs/architecture/base-datos.md` | Explica por que el SQL esta en `scripts/database` y cuando pasara a Flyway. |
| `docs/architecture/variables-entorno.md` | Explica los archivos `.env.example` y las variables iniciales del backend. |
| `docs/architecture/pull-request.md` | Explica la plantilla de Pull Request y su uso. |

Este archivo funciona como indice general. Los detalles tecnicos extensos deben mantenerse en el documento principal de cada paso para evitar duplicar informacion.

## Resultado actual

La fase deja preparada la siguiente base:

- Backend Spring Boot creado en `backend/`.
- Dependencias core configuradas en `backend/pom.xml`.
- Configuracion inicial en `backend/src/main/resources/application.properties`.
- JPA configurado con `ddl-auto=validate`.
- Flyway habilitado para futuras migraciones.
- Paquetes base creados bajo `com.kontora.pos`.
- Script SQL ubicado como referencia en `scripts/database/kontora_pos_schema.sql`.
- Carpeta `scripts/cloudflare/` preparada para configuraciones futuras del tunel.
- Archivos `.env.example` versionados sin secretos reales.
- Plantilla de Pull Request creada en `.github/pull_request_template.md`.

## Estructura de paquetes vigente

La estructura de paquetes vigente se documenta en `docs/modules/backend.md`, que es la fuente principal para los pasos 1 y 2.

El indice no repite la lista completa para evitar duplicidad documental.

## SQL y migraciones

El archivo:

```text
scripts/database/kontora_pos_schema.sql
```

se conserva como script auxiliar de referencia. Todavia no es una migracion oficial de Flyway.

Cuando se revise y se adapte, podra convertirse en:

```text
backend/src/main/resources/db/migration/V1__crear_esquema_inicial.sql
```

Ese cambio debe hacerse en un paso posterior y con validacion propia.

## Cloudflare

La carpeta:

```text
scripts/cloudflare/
```

queda reservada para scripts, comandos o notas tecnicas sobre Cloudflare Zero Trust. No debe contener tokens, credenciales ni archivos `.env` reales.

## Criterios de cierre

La Fase 2 se considera cerrada cuando:

1. El backend compila correctamente.
2. La estructura de paquetes base existe en el repositorio.
3. Las carpetas tecnicas estan versionadas.
4. El script SQL esta organizado en `scripts/database`.
5. Los archivos `.env.example` no contienen secretos reales.
6. La plantilla de Pull Request existe.
7. La documentacion de `docs/` refleja el estado real del repositorio.

## Pendientes para fases posteriores

- Crear `backend/Dockerfile`.
- Integrar el servicio backend en `infra/compose.local.yml`.
- Definir si el health check oficial sera `/actuator/health` o un endpoint propio como `/api/health`.
- Revisar el SQL antes de convertirlo en migracion Flyway.
- Iniciar el primer modulo funcional del backend.

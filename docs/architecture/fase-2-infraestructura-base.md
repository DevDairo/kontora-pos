# Fase 2 - Infraestructura Base y Estructura del Proyecto

## Objetivo de la fase

Construir la infraestructura base del proyecto Kontora POS después de la configuración inicial de Docker realizada en la Fase 1.

Esta fase prepara la estructura técnica mínima para iniciar el desarrollo modular del sistema, manteniendo una separación clara entre backend, documentación, scripts auxiliares, assets, variables de entorno, base de datos y futuras configuraciones de despliegue.

## Alcance de la fase

La Fase 2 comprende:

1. Inicialización del backend con Spring Boot.
2. Configuración inicial de dependencias core.
3. Creación de paquetes base del backend.
4. Preparación de carpetas técnicas para scripts, assets y Cloudflare Zero Trust.
5. Organización inicial del script SQL existente.
6. Creación de archivo de ejemplo para variables de entorno del backend.
7. Creación de plantilla de Pull Request.
8. Documentación interna de la infraestructura inicial del repositorio.

## Estado actual

| Paso | Estado | Documento relacionado |
|---|---|---|
| Paso 1 - Inicialización del backend Spring Boot | Completado | `docs/modules/backend.md` |
| Paso 2 - Estructura base de paquetes backend | Completado | `docs/modules/backend.md` |
| Paso 3 - Carpetas técnicas de infraestructura | Completado | `docs/architecture/infraestructura.md` |
| Paso 4 - Documentación índice de la Fase 2 | Completado | `docs/architecture/fase-2-infraestructura-base.md` |
| Paso 5 - Organización del script SQL existente | Completado | `docs/architecture/base-datos.md` |
| Paso 6 - Variables de entorno del backend | Completado | `docs/architecture/variables-entorno.md` |
| Paso 7 - Plantilla de Pull Request | Completado | `docs/architecture/pull-request.md` |
| Paso 8 - Actualización final del índice de Fase 2 | En proceso | `docs/architecture/fase-2-infraestructura-base.md` |

## Documentos relacionados

| Documento | Propósito |
|---|---|
| `docs/modules/backend.md` | Documenta la creación del backend, configuración inicial, incidencias, soluciones aplicadas y estructura base de paquetes. |
| `docs/architecture/infraestructura.md` | Documenta las carpetas técnicas para scripts, assets, base de datos y Cloudflare Zero Trust. |
| `docs/architecture/base-datos.md` | Documenta la ubicación del script SQL existente y su preparación futura para Flyway. |
| `docs/architecture/variables-entorno.md` | Documenta las variables de entorno del backend y la diferencia entre `.env.example` y `.env`. |
| `docs/architecture/pull-request.md` | Documenta la plantilla de Pull Request y su uso desde GitHub en el navegador. |
| `docs/architecture/fase-2-infraestructura-base.md` | Funciona como índice general de la Fase 2 y relaciona los documentos creados durante esta etapa. |

## Relación con la Fase 1

La Fase 1 dejó preparada la infraestructura Docker local con PostgreSQL y pgAdmin.

La Fase 2 se apoya en esa base para conectar el backend Spring Boot con PostgreSQL local y preparar la estructura del repositorio para el desarrollo modular.

## Relación con el backend

Durante esta fase se creó el proyecto backend dentro de:

```text
backend/
```

El backend fue generado con Spring Boot, Maven y Java 21. Su paquete raíz es:

```text
com.kontora.pos
```

Esta ubicación permite que Spring Boot detecte automáticamente las clases ubicadas dentro del paquete principal y sus subpaquetes.

## Relación con la estructura de paquetes

La estructura inicial del backend se organizó por responsabilidades técnicas y módulos funcionales:

```text
com.kontora.pos.config
com.kontora.pos.common
com.kontora.pos.common.exception
com.kontora.pos.common.response
com.kontora.pos.security
com.kontora.pos.usuario
com.kontora.pos.producto
com.kontora.pos.inventario
com.kontora.pos.venta
com.kontora.pos.caja
com.kontora.pos.transferencia
com.kontora.pos.gasto
com.kontora.pos.deposito
com.kontora.pos.reporte
```

Estos paquetes permiten iniciar el desarrollo por módulos sin mezclar responsabilidades.

## Relación con el script SQL existente

El proyecto cuenta con un script SQL de base de datos ubicado en:

```text
scripts/database/kontora_pos_schema.sql
```

Durante esta fase no se importó directamente como migración Flyway porque primero debe revisarse su contenido, orden de ejecución, convenciones de nombres y compatibilidad con el backend.

Antes de convertirlo en migración oficial, deberá evaluarse si puede moverse o adaptarse a:

```text
backend/src/main/resources/db/migration/V1__crear_esquema_inicial.sql
```

## Relación con variables de entorno

Durante esta fase se creó el archivo:

```text
backend/.env.example
```

Este archivo documenta las variables esperadas por el backend, pero no contiene credenciales reales.

Los archivos `.env` reales no deben versionarse.

## Relación con Cloudflare Zero Trust

El proyecto ya cuenta con dominio en Cloudflare Zero Trust.

Durante esta fase se preparó la carpeta:

```text
scripts/cloudflare
```

para guardar en pasos posteriores scripts, comandos o documentación técnica relacionados con exposición segura, túneles, dominios y despliegue.

No se deben guardar secretos, tokens ni credenciales reales en el repositorio.

## Relación con assets del proyecto

Durante esta fase también se preparó la carpeta:

```text
assets/
```

Esta carpeta queda reservada para recursos visuales o estáticos del proyecto, como imágenes e íconos usados en documentación, prototipos o interfaz.

La estructura inicial definida fue:

```text
assets/
├── icons/
└── images/
```

## Relación con Pull Requests

Durante esta fase se creó la plantilla:

```text
.github/pull_request_template.md
```

Esta plantilla no cambia el flujo de trabajo desde GitHub en el navegador. Solo agrega una guía automática al abrir Pull Requests para registrar descripción, tipo de cambio, validaciones, impacto técnico y checklist básico.

## Validaciones realizadas

Durante la Fase 2 se ejecutaron validaciones con Maven:

```powershell
cd C:\Users\corre\Desktop\kontora-pos\backend
mvn clean test
```

El resultado esperado y obtenido fue:

```text
BUILD SUCCESS
```

Esto confirma que la estructura inicial del backend no rompió la compilación ni las pruebas base del proyecto.

## Criterios de cierre de la fase

La Fase 2 podrá considerarse cerrada cuando:

1. El backend compile correctamente.
2. La estructura de paquetes base esté creada.
3. Las carpetas técnicas estén versionadas.
4. El script SQL existente esté organizado en `scripts/database`.
5. El archivo `backend/.env.example` esté creado sin secretos reales.
6. La plantilla de Pull Request esté creada.
7. La documentación interna mínima esté creada.
8. El repositorio quede limpio y sincronizado con GitHub.

## Recomendaciones de continuidad

Antes de avanzar a la siguiente fase se recomienda:

1. Mantener la documentación actualizada en cada cambio estructural.
2. Evitar mover el script SQL directamente a Flyway sin revisión previa.
3. No guardar credenciales reales en archivos versionados.
4. Confirmar que cada paso compile antes de hacer commit.
5. Hacer commits pequeños y descriptivos por cada avance validado.
6. Empezar el desarrollo funcional desde paquetes pequeños y bien delimitados.

## Estado del Paso 8

El Paso 8 queda completado cuando este archivo queda actualizado en:

```text
docs/architecture/fase-2-infraestructura-base.md
```

y el repositorio vuelve a quedar limpio después del commit y el push.

## Archivos relacionados con este paso

```text
docs/architecture/fase-2-infraestructura-base.md
```

## Estado esperado en Git

Antes de cerrar el Paso 8 con commit, el estado esperado debe incluir:

```text
Changes to be committed:
    modified:   docs/architecture/fase-2-infraestructura-base.md
```

## Commit sugerido

El commit sugerido para cerrar este paso es:

```powershell
git commit -m "docs: actualizar indice de fase 2"
```


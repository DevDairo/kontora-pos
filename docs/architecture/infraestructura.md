# Infraestructura del Repositorio - Scripts, Assets y Cloudflare

## Objetivo

Documentar la preparación de carpetas técnicas del repositorio Kontora POS durante la Fase 2, dejando ubicaciones claras para scripts de base de datos, scripts relacionados con Cloudflare Zero Trust y recursos estáticos del proyecto.

Este paso no configura todavía variables de entorno, despliegue, túneles, migraciones Flyway definitivas ni plantillas de Pull Request. Su propósito es preparar la estructura base del repositorio para que los elementos técnicos futuros tengan una ubicación organizada.

## Contexto del paso

Kontora POS ya cuenta con:

- Un entorno Docker local configurado en la Fase 1.
- PostgreSQL local mediante Docker.
- Un script SQL existente de base de datos.
- Un dominio administrado mediante Cloudflare Zero Trust.

Por esa razón, desde la Fase 2 se preparan carpetas específicas para separar:

- Scripts auxiliares de base de datos.
- Scripts o notas operativas relacionadas con Cloudflare.
- Assets generales del proyecto, como imágenes e íconos.

## Estructura creada

Desde la raíz del repositorio se creó la siguiente estructura:

```text
kontora-pos/
├── assets/
│   ├── icons/
│   │   └── .gitkeep
│   └── images/
│       └── .gitkeep
└── scripts/
    ├── cloudflare/
    │   └── .gitkeep
    └── database/
        └── .gitkeep
```

## Propósito de cada carpeta

| Carpeta | Propósito |
|---|---|
| `scripts/` | Carpeta general para scripts técnicos del proyecto. |
| `scripts/database/` | Ubicación para scripts auxiliares de base de datos que no necesariamente son migraciones Flyway. |
| `scripts/cloudflare/` | Ubicación para scripts, comandos o notas técnicas relacionadas con Cloudflare Zero Trust. |
| `assets/` | Carpeta general para recursos estáticos del proyecto. |
| `assets/images/` | Ubicación para imágenes usadas en documentación, prototipos o recursos visuales del sistema. |
| `assets/icons/` | Ubicación para íconos o recursos visuales pequeños usados en documentación o interfaz. |

## Diferencia entre `scripts/database` y migraciones Flyway

Es importante diferenciar estas dos ubicaciones:

| Ubicación | Uso |
|---|---|
| `backend/src/main/resources/db/migration` | Migraciones oficiales de Flyway ejecutadas por el backend. |
| `scripts/database` | Scripts auxiliares, pruebas SQL, respaldos de referencia o scripts manuales no ejecutados automáticamente por Flyway. |

El script SQL existente del proyecto deberá revisarse antes de decidir si se convierte en una migración Flyway oficial o si se conserva como script auxiliar de referencia.

## Relación con Cloudflare Zero Trust

La carpeta:

```text
scripts/cloudflare
```

queda reservada para documentación técnica o scripts relacionados con el dominio y la exposición segura del sistema mediante Cloudflare Zero Trust.

En este paso no se deben guardar secretos, tokens, credenciales ni archivos `.env` reales dentro de esta carpeta.

## Uso de archivos `.gitkeep`

Git no versiona carpetas vacías. Por esa razón, se creó un archivo `.gitkeep` dentro de cada carpeta nueva.

Los archivos `.gitkeep` no contienen lógica ni configuración. Su única función es permitir que la estructura de carpetas quede registrada en el repositorio.

## Validación realizada

Después de crear las carpetas, se ejecutó:

```powershell
git status
```

El resultado mostró las carpetas nuevas como archivos sin seguimiento:

```text
Untracked files:
    assets/
    scripts/
```

Este resultado es correcto porque las carpetas contienen archivos `.gitkeep` pendientes de agregar al control de versiones.

## Archivos relacionados con este paso

Los archivos creados o modificados para este paso son:

```text
assets/icons/.gitkeep
assets/images/.gitkeep
scripts/cloudflare/.gitkeep
scripts/database/.gitkeep
docs/architecture/infraestructura.md
```

## Estado del Paso 3

El Paso 3 queda completado porque:

- La estructura `assets/` y `scripts/` existe en la raíz del repositorio.
- Las subcarpetas técnicas están versionadas con `.gitkeep`.
- La documentación queda registrada en `docs/architecture/infraestructura.md`.
- El estado de Git queda limpio después del commit y el push.

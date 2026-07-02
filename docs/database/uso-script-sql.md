# Uso de Scripts SQL y Migraciones

## Objetivo

Servir como guia corta para ubicar los documentos de base de datos relacionados con scripts SQL y migraciones.

## Documento principal

La decision vigente de Fase 2 esta documentada en:

```text
docs/architecture/base-datos.md
```

Ese documento explica:

- Por que `kontora_pos_schema.sql` esta en `scripts/database/`.
- Por que todavia no se convirtio en migracion Flyway.
- Que criterios deben revisarse antes de crear `V1__crear_esquema_inicial.sql`.

## Ubicaciones vigentes

| Ubicacion | Uso |
|---|---|
| `scripts/database/kontora_pos_schema.sql` | Script auxiliar de referencia. No se ejecuta automaticamente. |
| `backend/src/main/resources/db/migration/` | Ubicacion futura de migraciones oficiales Flyway. |
| `database/schema/` | Carpeta reservada para documentacion o archivos de esquema auxiliares. |
| `database/migrations/` | Carpeta reservada para referencias o material academico relacionado con migraciones. |

## Regla actual

Durante la Fase 2, el SQL se conserva como referencia en `scripts/database/`. No debe moverse a Flyway hasta que sea revisado y validado.

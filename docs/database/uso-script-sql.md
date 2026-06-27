# Uso de scripts SQL y migraciones

## Objetivo

Documentar cómo se organizarán y utilizarán los scripts SQL y las migraciones de base de datos en Kontora POS.

La base de datos es un componente crítico del sistema porque conservará información de usuarios, roles, caja diaria, ventas, pagos, inventario, depósito, evidencias y auditoría.

## Ubicación de archivos

Los archivos relacionados con base de datos se organizarán en la carpeta `database/`:

```text
database/
├── schema/
└── migrations/
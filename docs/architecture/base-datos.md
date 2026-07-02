# Base de Datos - Script SQL y Preparación para Flyway

## Objetivo

Documentar la ubicación, clasificación y tratamiento inicial del script SQL existente del proyecto Kontora POS, preparando su futura integración con Flyway sin importarlo todavía como migración oficial.

Este documento forma parte de la Fase 2, en la cual se está construyendo la infraestructura base y la estructura técnica del repositorio.

## Contexto

El proyecto Kontora POS ya cuenta con un script SQL existente llamado:

```text
kontora_pos_schema.sql
```

Inicialmente, este archivo se encontraba en la raíz del repositorio:

```text
kontora-pos/kontora_pos_schema.sql
```

Durante este paso se decidió moverlo a una ubicación más adecuada para scripts auxiliares de base de datos:

```text
scripts/database/kontora_pos_schema.sql
```

## Decisión tomada

El script SQL no se movió directamente a la carpeta de migraciones Flyway porque todavía no ha sido revisado completamente bajo los criterios técnicos de migración.

La ubicación temporal y correcta para este momento es:

```text
scripts/database/kontora_pos_schema.sql
```

Esta carpeta se usa para conservar scripts auxiliares, scripts de referencia, pruebas SQL o archivos que aún no deben ser ejecutados automáticamente por el backend.

## Diferencia entre script auxiliar y migración Flyway

| Tipo de archivo | Ubicación | Uso |
|---|---|---|
| Script auxiliar de base de datos | `scripts/database/` | Archivo de referencia, análisis, pruebas o respaldo técnico. No se ejecuta automáticamente. |
| Migración Flyway oficial | `backend/src/main/resources/db/migration/` | Script versionado que Flyway ejecuta automáticamente al iniciar el backend. |

## Ubicación actual del script

Después del movimiento, el archivo queda ubicado en:

```text
scripts/database/kontora_pos_schema.sql
```

Esta ubicación permite mantener el script dentro del repositorio sin activar todavía su ejecución automática.

## Criterios antes de convertir el SQL en migración Flyway

Antes de mover o copiar este script a:

```text
backend/src/main/resources/db/migration/
```

se deben revisar los siguientes puntos:

1. Que el script no contenga comandos destructivos innecesarios, como `DROP DATABASE` o eliminaciones no controladas.
2. Que el script no dependa de una base de datos creada manualmente fuera de Docker.
3. Que los nombres de tablas y campos respeten las convenciones definidas para Kontora POS.
4. Que los nombres estén en español, sin tildes ni caracteres especiales.
5. Que el orden de creación de tablas respete las dependencias de claves foráneas.
6. Que las restricciones `NOT NULL` coincidan con las reglas de negocio confirmadas.
7. Que los campos opcionales no estén definidos como obligatorios.
8. Que las tablas de auditoría o trazabilidad no eliminen información necesaria para revisión gerencial.
9. Que el script pueda ejecutarse desde una base de datos vacía sin intervención manual.
10. Que el nombre del archivo cumpla el formato de Flyway.

## Convención esperada para Flyway

Cuando el script esté listo para convertirse en migración oficial, deberá nombrarse con la convención de Flyway:

```text
V1__crear_esquema_inicial.sql
```

La ubicación esperada será:

```text
backend/src/main/resources/db/migration/V1__crear_esquema_inicial.sql
```

El prefijo `V1__` indica que es la primera migración versionada del sistema. Flyway requiere doble guion bajo entre la versión y la descripción.

## Relación con `application.properties`

El backend ya tiene Flyway habilitado mediante:

```properties
spring.flyway.enabled=true
spring.flyway.locations=classpath:db/migration
```

Esto significa que cualquier archivo ubicado en:

```text
backend/src/main/resources/db/migration
```

con formato válido de Flyway será ejecutado automáticamente cuando el backend inicie y se conecte a la base de datos.

Por esa razón, el script SQL existente debe revisarse antes de convertirse en migración oficial.

## Comando usado para ubicar scripts SQL

Desde la raíz del repositorio se ejecutó:

```powershell
Get-ChildItem -Recurse -Filter *.sql
```

El resultado mostró:

```text
kontora_pos_schema.sql
```

ubicado inicialmente en la raíz del repositorio.

## Comando usado para mover el script

Desde la raíz del repositorio:

```powershell
Move-Item .\kontora_pos_schema.sql .\scripts\database\kontora_pos_schema.sql
```

## Estado observado en Git

Después de mover el archivo, se ejecutó:

```powershell
git status
```

El resultado mostró:

```text
Untracked files:
    scripts/database/kontora_pos_schema.sql
```

Esto indica que el archivo SQL no estaba versionado anteriormente en la raíz del repositorio y ahora Git lo detecta como archivo nuevo en su ubicación organizada.

## Recomendaciones

Antes de hacer commit de este paso se recomienda:

1. Confirmar que el archivo `kontora_pos_schema.sql` esté en `scripts/database/`.
2. Confirmar que el archivo no haya quedado duplicado en la raíz del repositorio.
3. No mover todavía el script a `backend/src/main/resources/db/migration/`.
4. No renombrarlo todavía como `V1__crear_esquema_inicial.sql` hasta revisar su contenido.
5. Incluir esta documentación en el mismo commit del movimiento del archivo.

## Estado del Paso 5

El Paso 5 queda completado cuando:

- El archivo `kontora_pos_schema.sql` queda ubicado en `scripts/database/`.
- Se documenta que el script todavía no es una migración Flyway oficial.
- El repositorio queda limpio después del commit y el push.

## Archivos relacionados con este paso

```text
scripts/database/kontora_pos_schema.sql
docs/architecture/base-datos.md
```

## Estado esperado en Git

Antes de cerrar este paso con commit, el estado esperado debe incluir:

```text
Changes to be committed:
    new file:   docs/architecture/base-datos.md
    new file:   scripts/database/kontora_pos_schema.sql
```

## Commit sugerido

El commit sugerido para cerrar este paso es:

```powershell
git commit -m "chore: organizar script sql de base de datos"
```


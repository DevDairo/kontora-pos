# Variables de Entorno - Backend e Infraestructura

## Objetivo

Documentar las variables de entorno iniciales del backend de Kontora POS, diferenciando entre archivos de ejemplo versionables y archivos reales que no deben subirse al repositorio.

Este documento forma parte de la Fase 2 y permite preparar la configuración futura del backend para desarrollo local, Docker, despliegue y exposición mediante Cloudflare Zero Trust.

## Contexto

Durante la Fase 1 se creó un archivo de ejemplo para la infraestructura Docker local:

```text
infra/.env.example
```

Durante la Fase 2 se agrega un archivo de ejemplo específico para el backend:

```text
backend/.env.example
```

Este archivo no contiene secretos reales. Solo documenta los nombres de variables que el backend puede necesitar para ejecutarse en diferentes entornos.

## Archivo creado

El archivo creado en este paso es:

```text
backend/.env.example
```

## Contenido del archivo `backend/.env.example`

```env
# Application
SPRING_PROFILES_ACTIVE=local

# Server
SERVER_PORT=8080

# PostgreSQL
SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/kontora_pos
SPRING_DATASOURCE_USERNAME=kontora_user
SPRING_DATASOURCE_PASSWORD=change_me_local_only

# Flyway
SPRING_FLYWAY_ENABLED=true

# Cloudflare Zero Trust
APP_PUBLIC_URL=https://example.kontora.local
```

## Propósito de cada variable

| Variable | Propósito |
|---|---|
| `SPRING_PROFILES_ACTIVE` | Define el perfil activo de Spring Boot. Para desarrollo local se usa `local`. |
| `SERVER_PORT` | Define el puerto HTTP donde escuchará el backend. El valor inicial sugerido es `8080`. |
| `SPRING_DATASOURCE_URL` | Define la URL JDBC de conexión a PostgreSQL. |
| `SPRING_DATASOURCE_USERNAME` | Define el usuario de conexión a PostgreSQL. |
| `SPRING_DATASOURCE_PASSWORD` | Define la contraseña de conexión a PostgreSQL. |
| `SPRING_FLYWAY_ENABLED` | Permite activar o desactivar Flyway mediante variable de entorno. |
| `APP_PUBLIC_URL` | Define la URL pública futura de la aplicación cuando se exponga mediante dominio o Cloudflare Zero Trust. |

## Diferencia entre `.env.example` y `.env`

| Archivo | Se versiona | Contiene secretos reales | Propósito |
|---|---|---|---|
| `.env.example` | Sí | No | Documenta las variables necesarias y valores de ejemplo. |
| `.env` | No | Sí puede contenerlos | Define valores reales para el entorno local o despliegue. |

El archivo `.env.example` se debe subir al repositorio porque sirve como guía para otros entornos.

El archivo `.env` real no debe subirse al repositorio.

## Relación con `application.properties`

El backend ya tiene configuraciones que leen variables de entorno con valores por defecto:

```properties
spring.datasource.url=${SPRING_DATASOURCE_URL:jdbc:postgresql://localhost:5432/kontora_pos}
spring.datasource.username=${SPRING_DATASOURCE_USERNAME:kontora_user}
spring.datasource.password=${SPRING_DATASOURCE_PASSWORD:change_me_local_only}
```

Esto permite que el backend funcione en local con valores por defecto, pero también permite reemplazarlos en despliegues futuros mediante variables de entorno reales.

## Relación con Docker

El archivo:

```text
infra/.env.example
```

documenta variables usadas por Docker Compose para PostgreSQL y pgAdmin.

El archivo:

```text
backend/.env.example
```

documenta variables usadas por la aplicación Spring Boot.

Aunque algunas variables comparten valores relacionados, cumplen funciones distintas.

## Relación con Cloudflare Zero Trust

La variable:

```env
APP_PUBLIC_URL=https://example.kontora.local
```

queda como referencia para una URL pública futura.

En pasos posteriores, esta variable podrá adaptarse al dominio real administrado mediante Cloudflare Zero Trust.

No se deben guardar tokens, credenciales de túnel ni secretos de Cloudflare en archivos versionados.

## Recomendaciones de seguridad

1. No crear ni subir `backend/.env` con credenciales reales.
2. Mantener `backend/.env.example` con valores genéricos o de desarrollo.
3. No guardar tokens de Cloudflare en el repositorio.
4. No guardar contraseñas productivas en archivos `.md`.
5. Usar variables de entorno reales en el servidor o plataforma de despliegue.
6. Verificar que `.gitignore` excluya archivos `.env`.

## Validación realizada

Después de crear `backend/.env.example`, se ejecutó:

```powershell
git status
```

El resultado esperado es:

```text
Untracked files:
    backend/.env.example
```

Este resultado es correcto porque el archivo de ejemplo debe ser agregado al control de versiones.

## Estado del Paso 6

El Paso 6 queda completado porque:

- `backend/.env.example` queda creado.
- El archivo solo contiene valores de ejemplo.
- No se crea ni versiona `backend/.env`.
- La documentación queda registrada en `docs/architecture/variables-entorno.md`.
- El repositorio queda limpio después del commit y el push.

## Archivos relacionados con este paso

```text
backend/.env.example
docs/architecture/variables-entorno.md
```

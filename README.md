# Kontora POS

Kontora POS es una aplicación web para apoyar la gestión de ventas, caja diaria, inventario, depósito, evidencias, usuarios, roles y auditoría de un negocio de granizados.

## Estado actual del proyecto

Fase 1: creación del proyecto y entorno Docker.

En esta fase se preparo la estructura base del repositorio, la infraestructura local con Docker, las variables de entorno de ejemplo y la documentación inicial. Todavía no se desarrolla lógica funcional del sistema.

## Arquitectura objetivo

La arquitectura general del sistema estará separada por capas:

- Frontend: React + TypeScript + Vite.
- Backend: Java + Spring Boot.
- Base de datos: PostgreSQL local para desarrollo y Supabase PostgreSQL para entorno gestionado.
- Evidencias: Supabase Storage.
- Contenedores: Docker y Docker Compose.
- Control de versiones: Git y GitHub.

## Infraestructura local

En la Fase 1 se levantaron los siguientes servicios mediante Docker Compose:

- PostgreSQL local.
- pgAdmin local.

## Levantar servicios locales

Desde la carpeta `infra`:

```powershell
docker compose -f compose.local.yml --env-file .env up -d
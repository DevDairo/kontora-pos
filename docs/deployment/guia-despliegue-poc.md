# Guía de despliegue local - Fase 1

## Objetivo

Documentar el procedimiento inicial para levantar la infraestructura local de Kontora POS mediante Docker Compose.

En esta fase no se despliega todavía el backend ni el frontend. El propósito es validar que la base técnica mínima funcione correctamente antes de desarrollar lógica funcional del sistema.

## Servicios incluidos

La infraestructura local inicial incluye:

- PostgreSQL local.
- pgAdmin local.

## Ubicación de archivos relevantes

```text
infra/
├── .env
├── .env.example
└── compose.local.yml
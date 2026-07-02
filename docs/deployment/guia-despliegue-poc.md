# Guia de Despliegue Local - Fase 1

## Objetivo

Documentar el procedimiento inicial para levantar la infraestructura local de Kontora POS mediante Docker Compose.

En esta fase no se despliega todavia el backend ni el frontend. El proposito es validar que la base tecnica minima funcione correctamente antes de desarrollar logica funcional del sistema.

## Servicios incluidos

La infraestructura local inicial incluye:

- PostgreSQL local.
- pgAdmin local.

## Ubicacion de archivos relevantes

```text
infra/
├── .env
├── .env.example
└── compose.local.yml
```

## Relacion con Fase 2

La Fase 2 usa esta base local para conectar el backend Spring Boot con PostgreSQL. La dockerizacion del backend y su integracion en `compose.local.yml` quedan como pendientes de una fase posterior.

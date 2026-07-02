# Pull Request - Plantilla de Revisión

## Objetivo

Documentar la creación de una plantilla de Pull Request para el repositorio Kontora POS.

La plantilla permite que cada Pull Request tenga una estructura mínima de descripción, validación e impacto técnico. Esto ayuda a mantener orden, trazabilidad y buenas prácticas durante el desarrollo del proyecto.

## Contexto

El flujo de trabajo habitual del proyecto puede seguir realizándose desde la interfaz web de GitHub.

La plantilla:

```text
.github/pull_request_template.md
```

no crea Pull Requests automáticamente y no obliga a usar comandos nuevos. Su función es mostrar una guía predeterminada en el campo de descripción cuando se abre un Pull Request desde GitHub.

## Archivo creado

El archivo creado en este paso es:

```text
.github/pull_request_template.md
```

## Contenido de la plantilla

```md
# Descripción

Resume brevemente el cambio realizado.

# Tipo de cambio

Marca con una `x` la opción correspondiente:

- [ ] Configuración / infraestructura
- [ ] Backend
- [ ] Frontend
- [ ] Base de datos
- [ ] Documentación
- [ ] Corrección de error
- [ ] Otro

# Validaciones realizadas

Indica los comandos o pruebas ejecutadas:

- [ ] `mvn clean test`
- [ ] `docker compose up`
- [ ] Revisión manual
- [ ] No aplica

# Impacto técnico

Describe si el cambio afecta:

- Variables de entorno
- Base de datos
- Docker
- Seguridad
- Cloudflare / despliegue
- Módulos funcionales

# Checklist

- [ ] El cambio está limitado al alcance del PR.
- [ ] No se suben secretos ni credenciales reales.
- [ ] La documentación fue actualizada si aplica.
- [ ] El proyecto compila o se documenta por qué no aplica.
```

## Uso desde GitHub en el navegador

El flujo sigue siendo el habitual:

1. Crear una rama de trabajo.
2. Realizar cambios.
3. Hacer commit.
4. Hacer push de la rama.
5. Abrir GitHub en el navegador.
6. Seleccionar `Compare & pull request`.
7. Completar la descripción usando la plantilla cargada automáticamente.

## Beneficios para el proyecto

La plantilla ayuda a:

1. Estandarizar la revisión de cambios.
2. Registrar pruebas ejecutadas.
3. Identificar impacto técnico.
4. Evitar subir secretos o credenciales reales.
5. Recordar actualizar documentación cuando aplique.
6. Mejorar la presentación metodológica del proyecto integrador.

## Recomendaciones

Antes de cerrar un Pull Request se recomienda:

1. Marcar el tipo de cambio realizado.
2. Indicar pruebas o validaciones ejecutadas.
3. Explicar si el cambio afecta base de datos, Docker, seguridad o despliegue.
4. Verificar que no existan archivos `.env` reales en el PR.
5. Confirmar que la documentación se actualizó cuando el cambio modifica estructura, configuración o decisiones técnicas.

## Estado del Paso 7

El Paso 7 queda completado porque:

- Se crea `.github/pull_request_template.md`.
- Se documenta su propósito.
- El archivo queda versionado en GitHub.
- El repositorio queda limpio después del commit y el push.

## Archivos relacionados con este paso

```text
.github/pull_request_template.md
docs/architecture/pull-request.md
```

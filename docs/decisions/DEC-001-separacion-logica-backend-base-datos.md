# DEC-001 - Separación entre lógica de negocio y base de datos

## Estado

Aceptada.

## Contexto

Kontora POS requiere controlar operaciones críticas relacionadas con ventas, pagos, caja diaria, inventario, depósito, evidencias, usuarios, roles y auditoría.

Estas operaciones no deben quedar dispersas entre frontend, base de datos y consultas manuales, porque eso aumentaría el riesgo de errores, inconsistencias y pérdida de trazabilidad.

El sistema necesita conservar integridad transaccional. Por ejemplo, una venta puede afectar pagos, caja diaria, stock diario de vasos, promociones, auditoría y posteriormente cierre de caja. Si estas reglas se ejecutan de forma desordenada, pueden generarse descuadres o datos incompletos.

## Decisión

La lógica crítica del negocio se implementará en el backend mediante servicios transaccionales de Spring Boot.

La base de datos PostgreSQL será responsable de:

- Persistir la información.
- Mantener relaciones entre tablas.
- Aplicar llaves primarias y foráneas.
- Conservar históricos.
- Soportar restricciones básicas.
- Permitir auditoría y consulta posterior.

El frontend no accederá directamente a operaciones críticas de base de datos. Todas las operaciones sensibles deberán pasar por la API del backend.

## Justificación

Centralizar la lógica de negocio en el backend permite:

- Validar permisos antes de ejecutar operaciones.
- Aplicar reglas de caja, ventas, inventario y depósito de forma controlada.
- Usar transacciones para confirmar o revertir operaciones completas.
- Probar reglas mediante pruebas unitarias e integración.
- Evitar que el frontend manipule directamente datos sensibles.
- Facilitar el mantenimiento del sistema por módulos.
- Reducir el riesgo de inconsistencias entre ventas, pagos, inventario y caja.

## Consecuencias positivas

- Mayor seguridad en operaciones críticas.
- Mejor trazabilidad de acciones sensibles.
- Código más mantenible y organizado por módulos.
- Reglas de negocio más fáciles de probar.
- Menor dependencia de procedimientos manuales en base de datos.
- Separación clara entre presentación, lógica y persistencia.

## Consecuencias negativas o costos

- El backend tendrá mayor responsabilidad técnica.
- Será necesario diseñar servicios transaccionales correctamente.
- Las pruebas del backend serán obligatorias para validar reglas críticas.
- No será recomendable modificar datos manualmente desde la base de datos sin pasar por procesos controlados.

## Aplicación en Kontora POS

Esta decisión aplica especialmente a los siguientes procesos:

- Inicio y cierre de sesión.
- Apertura y cierre de caja diaria.
- Registro y anulación de ventas.
- Aplicación de promociones.
- Registro de pagos en efectivo, transferencia o mixtos.
- Validación o rechazo de transferencias.
- Descuento y restauración de stock diario de vasos.
- Registro de gastos.
- Registro de consumos diarios de inventario.
- Solicitud y aprobación de ajustes de inventario.
- Generación de movimientos de depósito.
- Registro de auditoría de operaciones sensibles.

## Relación con futuras fases

En fases posteriores, esta decisión deberá reflejarse en la estructura del backend mediante paquetes por dominio, por ejemplo:

```text
com.kontora.pos.ventas
com.kontora.pos.caja
com.kontora.pos.inventario
com.kontora.pos.deposito
com.kontora.pos.usuarios
com.kontora.pos.auditoria
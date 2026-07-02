# Backend - Inicializacion Spring Boot y Paquetes Base

## Objetivo

Documentar los pasos 1 y 2 de la Fase 2: inicializar el backend Spring Boot y crear la estructura base de paquetes para el desarrollo modular de Kontora POS.

Este documento se limita al backend. La organizacion de scripts, variables de entorno, Cloudflare y Pull Requests se documenta en los archivos de arquitectura correspondientes.

## Paso 1 - Inicializacion del backend Spring Boot

### Resultado esperado

El backend debe quedar creado directamente en:

```text
backend/
```

La estructura correcta es:

```text
kontora-pos/
└── backend/
    ├── pom.xml
    ├── nbactions.xml
    └── src/
```

Se debe evitar una estructura anidada como:

```text
kontora-pos/
└── backend/
    └── kontora-pos-backend/
        ├── pom.xml
        └── src/
```

### Configuracion base

| Elemento | Valor definido |
|---|---|
| Lenguaje | Java |
| Version de Java | 21 |
| Framework | Spring Boot |
| Version de Spring Boot | 3.5.15 |
| Gestor de dependencias | Maven |
| Packaging | Jar |
| Group | `com.kontora` |
| Artifact | `pos` |
| Nombre de la aplicacion | `kontora-pos-backend` |
| Carpeta del proyecto | `backend` |
| Paquete raiz | `com.kontora.pos` |
| Base de datos local | PostgreSQL mediante Docker |
| Migraciones | Flyway |

### Dependencias base

El proyecto Spring Boot incluye:

```text
Spring Web
Spring Data JPA
PostgreSQL Driver
Flyway Migration
Validation
Spring Boot Actuator
Lombok
```

Estas dependencias permiten iniciar una API REST con persistencia JPA, conexion a PostgreSQL, validaciones, migraciones futuras, endpoints de salud mediante Actuator y reduccion de codigo repetitivo con Lombok.

### Clase principal

La clase principal queda ubicada en:

```text
backend/src/main/java/com/kontora/pos/KontoraPosBackendApplication.java
```

Su paquete es:

```java
package com.kontora.pos;
```

Spring Boot escanea automaticamente las clases ubicadas dentro de `com.kontora.pos` y sus subpaquetes. Por eso los modulos futuros deben mantenerse dentro de este paquete raiz.

### Configuracion inicial

El archivo principal de configuracion es:

```text
backend/src/main/resources/application.properties
```

La configuracion inicial relevante es:

```properties
spring.application.name=kontora-pos-backend

spring.datasource.url=${SPRING_DATASOURCE_URL:jdbc:postgresql://localhost:5432/kontora_pos}
spring.datasource.username=${SPRING_DATASOURCE_USERNAME:kontora_user}
spring.datasource.password=${SPRING_DATASOURCE_PASSWORD:change_me_local_only}
spring.datasource.driver-class-name=org.postgresql.Driver

spring.jpa.hibernate.ddl-auto=validate
spring.jpa.open-in-view=false

spring.flyway.enabled=true
spring.flyway.locations=classpath:db/migration

management.endpoints.web.exposure.include=health
```

Puntos importantes:

- `ddl-auto=validate` evita que Hibernate cree o modifique tablas automaticamente.
- Flyway queda habilitado, pero solo ejecutara scripts ubicados en `backend/src/main/resources/db/migration`.
- El endpoint de salud disponible por Actuator se valida por la ruta que exponga Spring Boot para `health`.

### Incidencias resueltas

| Incidencia | Causa | Solucion aplicada |
|---|---|---|
| NetBeans no permitia crear el proyecto dentro de `backend`. | La carpeta ya existia por un `.gitkeep`. | Se verifico que estuviera vacia y se genero el proyecto directamente en `backend`. |
| Riesgo de crear `backend/kontora-pos-backend`. | El nombre del proyecto podia convertirse en subcarpeta. | Se uso `backend` como carpeta y `kontora-pos-backend` como nombre logico. |
| Maven no encontraba `spring-boot-starter-parent:3.5.16.RELEASE`. | Esa version no existe y Spring Boot 3 no usa el sufijo `.RELEASE`. | Se corrigio a `3.5.15`. |
| `mvn clean test` fallo por datasource sin configurar. | JPA, Flyway y PostgreSQL requieren datos de conexion. | Se configuro `application.properties` con variables y valores locales de ejemplo. |
| No existia Maven Wrapper. | El proyecto generado no incluyo `mvnw.cmd`. | Se uso Maven instalado con `mvn clean test`. |

### Validacion del paso

Comando usado desde `backend/`:

```powershell
mvn clean test
```

Resultado documentado:

```text
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
BUILD SUCCESS
```

Con esto el Paso 1 queda completado.

## Paso 2 - Estructura base de paquetes

### Objetivo

Crear paquetes iniciales para separar responsabilidades tecnicas y funcionales antes de desarrollar logica de negocio.

Este paso no agrega entidades, repositorios, servicios ni controladores. Solo prepara la arquitectura interna del backend.

### Ubicacion base

Los paquetes se crean dentro de:

```text
backend/src/main/java/com/kontora/pos
```

### Paquetes creados

```text
com.kontora.pos.auditoria
com.kontora.pos.caja
com.kontora.pos.catalogos
com.kontora.pos.common
com.kontora.pos.common.audit
com.kontora.pos.common.config
com.kontora.pos.common.exception
com.kontora.pos.common.response
com.kontora.pos.common.security
com.kontora.pos.config
com.kontora.pos.deposito
com.kontora.pos.evidencias
com.kontora.pos.gasto
com.kontora.pos.inventario
com.kontora.pos.pagos
com.kontora.pos.producto
com.kontora.pos.reporte
com.kontora.pos.security
com.kontora.pos.transferencia
com.kontora.pos.usuario
com.kontora.pos.venta
```

### Proposito de los paquetes

| Paquete | Proposito |
|---|---|
| `auditoria` | Registro y consulta de operaciones sensibles. |
| `caja` | Apertura, control y cierre de caja diaria. |
| `catalogos` | Catalogos base reutilizables del sistema. |
| `common` | Codigo compartido entre modulos. |
| `common.audit` | Componentes transversales de auditoria. |
| `common.config` | Configuraciones compartidas de soporte tecnico. |
| `common.exception` | Excepciones y manejo centralizado de errores. |
| `common.response` | Respuestas comunes de la API. |
| `common.security` | Utilidades transversales de seguridad. |
| `config` | Configuracion general propia de la aplicacion. |
| `deposito` | Calculo y registro de depositos de cierre. |
| `evidencias` | Metadatos y referencias de soportes o comprobantes. |
| `gasto` | Registro, edicion, anulacion y consulta de gastos. |
| `inventario` | Inventario general, diario, entradas, salidas y ajustes. |
| `pagos` | Pagos en efectivo, transferencia o formas mixtas. |
| `producto` | Productos, referencias, tamanos, precios y promociones. |
| `reporte` | Consultas y reportes administrativos. |
| `security` | Seguridad de aplicacion, autenticacion, autorizacion y JWT. |
| `transferencia` | Registro, aceptacion, rechazo y consulta de transferencias. |
| `usuario` | Usuarios, roles, permisos y autenticacion funcional. |
| `venta` | Registro de ventas, detalles, promociones y anulaciones permitidas. |

### Convencion de nombres

La convencion vigente usa nombres en singular para modulos principales como `usuario`, `producto`, `venta` y `gasto`.

Se mantienen nombres plurales cuando representan una agrupacion natural o tecnica, por ejemplo `catalogos`, `pagos` y `evidencias`.

### Archivos `package-info.java`

Cada paquete tiene un archivo `package-info.java` para que Git pueda versionar la carpeta aunque todavia no existan clases funcionales.

Ejemplo:

```java
package com.kontora.pos.venta;
```

### Estructura esperada

```text
com/kontora/pos/
├── KontoraPosBackendApplication.java
├── auditoria/
├── caja/
├── catalogos/
├── common/
│   ├── audit/
│   ├── config/
│   ├── exception/
│   ├── response/
│   └── security/
├── config/
├── deposito/
├── evidencias/
├── gasto/
├── inventario/
├── pagos/
├── producto/
├── reporte/
├── security/
├── transferencia/
├── usuario/
└── venta/
```

### Validacion del paso

El Paso 2 queda completado cuando:

1. Todos los paquetes base existen bajo `com.kontora.pos`.
2. Cada paquete tiene su `package-info.java`.
3. No se agrego logica de negocio prematura.
4. La documentacion refleja la estructura real del repositorio.

## Archivos principales relacionados

```text
backend/pom.xml
backend/src/main/java/com/kontora/pos/KontoraPosBackendApplication.java
backend/src/main/resources/application.properties
backend/src/test/java/com/kontora/pos/KontoraPosBackendApplicationTests.java
docs/modules/backend.md
```

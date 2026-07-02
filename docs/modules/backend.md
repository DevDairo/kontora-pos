# Backend - Inicialización Spring Boot

## Objetivo

Documentar la inicialización técnica del backend de Kontora POS durante la Fase 2 del proyecto, incluyendo la estructura generada, configuración base, incidencias encontradas, causas, soluciones aplicadas, recomendaciones previas a la compilación y comandos usados para validar el funcionamiento inicial.

Este documento sirve como guía interna para evitar errores repetidos durante la configuración del backend y como evidencia técnica del proceso de construcción de la infraestructura base del proyecto.

## Contexto del paso

El backend fue creado dentro del repositorio principal de Kontora POS, específicamente en la carpeta:

```text
kontora-pos/backend
```

Antes de generar el proyecto, ya existía una carpeta `backend` vacía versionada con un archivo `.gitkeep`. Como NetBeans no permitía crear el proyecto sobre una carpeta existente, se verificó primero que la carpeta estuviera vacía y luego se eliminó para permitir que el IDE generara correctamente el proyecto Spring Boot en la ubicación esperada.

La estructura correcta esperada es:

```text
kontora-pos/
└── backend/
    ├── pom.xml
    ├── nbactions.xml
    └── src/
```

La estructura incorrecta que se debía evitar era:

```text
kontora-pos/
└── backend/
    └── kontora-pos-backend/
        ├── pom.xml
        └── src/
```

## Configuración base usada

| Elemento | Valor definido |
|---|---|
| Lenguaje | Java |
| Versión de Java | 21 |
| Framework | Spring Boot |
| Versión de Spring Boot | 3.5.15 |
| Gestor de dependencias | Maven |
| Packaging | Jar |
| Group | `com.kontora` |
| Artifact | `pos` |
| Nombre lógico de la aplicación | `kontora-pos-backend` |
| Carpeta del proyecto | `backend` |
| Package principal | `com.kontora.pos` |
| Base de datos | PostgreSQL local mediante Docker |
| Motor de migraciones | Flyway |
| IDE usado para generación | NetBeans |
| Validación por consola | PowerShell en Windows |

## Dependencias base seleccionadas

Durante la creación del proyecto Spring Boot se seleccionaron las siguientes dependencias:

```text
Spring Web
Spring Data JPA
PostgreSQL Driver
Flyway Migration
Validation
Spring Boot Actuator
Lombok
```

Estas dependencias permiten iniciar la base técnica del backend con soporte para API REST, persistencia con JPA, conexión a PostgreSQL, migraciones de base de datos con Flyway, validación de datos, endpoints de salud y reducción de código repetitivo mediante Lombok.

## Estructura inicial generada

La estructura inicial del backend quedó organizada de la siguiente manera:

```text
backend/
├── .gitattributes
├── .gitignore
├── nbactions.xml
├── pom.xml
└── src/
    ├── main/
    │   ├── java/
    │   │   └── com/
    │   │       └── kontora/
    │   │           └── pos/
    │   │               └── KontoraPosBackendApplication.java
    │   └── resources/
    │       ├── application.properties
    │       ├── db/
    │       ├── static/
    │       └── templates/
    └── test/
        └── java/
            └── com/
                └── kontora/
                    └── pos/
                        └── KontoraPosBackendApplicationTests.java
```

## Clase principal generada

La clase principal del backend quedó ubicada en:

```text
backend/src/main/java/com/kontora/pos/KontoraPosBackendApplication.java
```

Su paquete principal es:

```java
package com.kontora.pos;
```

Este paquete raíz es importante porque Spring Boot escanea automáticamente los componentes ubicados dentro de `com.kontora.pos` y sus subpaquetes. Por esa razón, los futuros módulos del backend deben quedar dentro de este paquete base.

## Paquetes base recomendados

Desde el inicio de la Fase 2 se definió que los paquetes funcionales del backend deben organizarse progresivamente así:

```text
com.kontora.pos.config
com.kontora.pos.common
com.kontora.pos.common.exception
com.kontora.pos.common.response
com.kontora.pos.security
com.kontora.pos.usuario
com.kontora.pos.producto
com.kontora.pos.inventario
com.kontora.pos.venta
com.kontora.pos.caja
com.kontora.pos.transferencia
com.kontora.pos.gasto
com.kontora.pos.deposito
com.kontora.pos.reporte
```

Esta organización permitirá separar responsabilidades por dominio funcional y mantener el backend preparado para crecer de forma ordenada.

## Incidencias encontradas y soluciones aplicadas

| Incidencia | Causa | Solución aplicada |
|---|---|---|
| NetBeans no permitía crear el proyecto dentro de `backend`. | La carpeta `backend` ya existía porque contenía un archivo `.gitkeep` usado para versionar la carpeta vacía. | Se verificó que la carpeta estuviera vacía, se eliminó y luego se generó el proyecto Spring Boot usando `backend` como carpeta final. |
| Existía riesgo de crear el proyecto anidado como `backend/kontora-pos-backend`. | Si se usaba `kontora-pos-backend` como nombre de carpeta desde el IDE, NetBeans podía crear una subcarpeta dentro de `backend`. | Se definió `backend` como nombre de carpeta del proyecto y `kontora-pos-backend` como nombre lógico de la aplicación. |
| VS Code mostraba el aviso `non-project file, only syntax errors are reported`. | VS Code todavía no reconocía el archivo Java como parte de un proyecto Maven válido. | Se priorizó la validación con Maven y NetBeans. Después de corregir el `pom.xml`, el proyecto pudo ser reconocido correctamente. |
| El comando `.\mvnw.cmd clean test` falló en PowerShell. | El proyecto generado no incluyó Maven Wrapper, por lo tanto no existía el archivo `mvnw.cmd`. | Se usó Maven instalado en el sistema con el comando `mvn clean test`. |
| Maven no encontraba `spring-boot-starter-parent:3.5.16.RELEASE`. | La versión `3.5.16.RELEASE` no existe en Maven Central. Además, Spring Boot 3 ya no usa el sufijo `.RELEASE`. | Se corrigió el `pom.xml` usando la versión válida `3.5.15`. |
| NetBeans no mostraba correctamente la carpeta `src`. | El proyecto Maven estaba roto por la versión incorrecta del parent de Spring Boot. | Tras corregir la versión de Spring Boot en el `pom.xml`, NetBeans volvió a mostrar correctamente la estructura del proyecto. |
| `mvn clean test` falló con `Failed to determine a suitable driver class`. | Spring Boot detectó las dependencias de JPA, Flyway y PostgreSQL, pero no tenía configurada la conexión `spring.datasource`. | Se configuró `application.properties` con URL, usuario, contraseña y driver de PostgreSQL. |
| Durante las pruebas apareció una advertencia de `byte-buddy-agent`. | Java mostró una advertencia por carga dinámica de agentes usada internamente por herramientas de prueba. | No requirió corrección porque las pruebas terminaron correctamente con `BUILD SUCCESS`. Se documentó como advertencia no bloqueante. |

## Corrección aplicada en `pom.xml`

El proyecto fue generado inicialmente con una versión incorrecta de Spring Boot:

```xml
<version>3.5.16.RELEASE</version>
```

Esa versión no existe en Maven Central. La versión corregida fue:

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.5.15</version>
    <relativePath/>
</parent>
```

Esta corrección permitió que Maven descargara correctamente las dependencias del proyecto.

## Configuración aplicada en `application.properties`

El archivo de configuración principal del backend quedó ubicado en:

```text
backend/src/main/resources/application.properties
```

La configuración aplicada fue:

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

## Explicación de la configuración aplicada

| Propiedad | Propósito |
|---|---|
| `spring.application.name` | Define el nombre lógico de la aplicación backend. |
| `spring.datasource.url` | Define la URL de conexión a PostgreSQL. Incluye un valor por defecto para desarrollo local. |
| `spring.datasource.username` | Define el usuario de conexión a la base de datos. |
| `spring.datasource.password` | Define la contraseña de conexión a la base de datos. |
| `spring.datasource.driver-class-name` | Indica explícitamente el driver JDBC de PostgreSQL. |
| `spring.jpa.hibernate.ddl-auto=validate` | Evita que Hibernate cree o modifique tablas automáticamente. Solo valida que el modelo coincida con la base de datos. |
| `spring.jpa.open-in-view=false` | Desactiva una práctica que puede ocultar problemas de consultas fuera de la capa transaccional. |
| `spring.flyway.enabled=true` | Activa Flyway para manejar migraciones de base de datos. |
| `spring.flyway.locations` | Define la ubicación de los scripts de migración dentro del classpath. |
| `management.endpoints.web.exposure.include=health` | Expone el endpoint de salud para verificar el estado básico de la aplicación. |

## Relación con Docker y PostgreSQL local

La configuración del backend fue alineada con los valores definidos en la infraestructura Docker local:

```text
POSTGRES_DB=kontora_pos
POSTGRES_USER=kontora_user
POSTGRES_PASSWORD=change_me_local_only
POSTGRES_PORT=5432
```

Por esa razón, la URL local de conexión usada por defecto es:

```text
jdbc:postgresql://localhost:5432/kontora_pos
```

Esta configuración permite que el backend se conecte al contenedor PostgreSQL levantado desde la carpeta `infra`.

## Recomendaciones antes de ejecutar pruebas del backend

Antes de ejecutar pruebas o compilaciones del backend, se deben verificar los siguientes puntos:

1. Confirmar que el archivo `pom.xml` use una versión válida de Spring Boot.
2. Confirmar que la versión de Spring Boot no tenga el sufijo `.RELEASE`.
3. Confirmar que Java 21 esté instalado y disponible.
4. Confirmar que Maven esté instalado y disponible en el `PATH`, si el proyecto no tiene Maven Wrapper.
5. Confirmar que PostgreSQL esté levantado con Docker.
6. Confirmar que el archivo `.env` de `infra` exista y tenga las credenciales correctas.
7. Confirmar que `application.properties` tenga configurada la conexión a PostgreSQL.
8. Confirmar que el proyecto no haya quedado anidado dentro de otra carpeta.
9. Confirmar que NetBeans haya recargado el proyecto después de modificar el `pom.xml`.
10. Confirmar que las futuras migraciones Flyway se ubiquen en `src/main/resources/db/migration`.

## Comandos usados para validar

### Verificar contenido de la carpeta backend

Desde la raíz del repositorio:

```powershell
cd C:\Users\corre\Desktop\kontora-pos
Get-ChildItem .\backend
```

El resultado esperado debe incluir archivos como:

```text
pom.xml
src
nbactions.xml
.gitignore
.gitattributes
```

### Entrar al backend

```powershell
cd C:\Users\corre\Desktop\kontora-pos\backend
```

### Ejecutar pruebas con Maven

Si el proyecto no tiene Maven Wrapper, usar:

```powershell
mvn clean test
```

No usar este comando si no existe `mvnw.cmd`:

```powershell
.\mvnw.cmd clean test
```

### Levantar PostgreSQL local con Docker

Desde la carpeta `infra`:

```powershell
cd C:\Users\corre\Desktop\kontora-pos\infra
docker compose -f compose.local.yml --env-file .env up -d
```

### Volver al backend y ejecutar pruebas

```powershell
cd C:\Users\corre\Desktop\kontora-pos\backend
mvn clean test
```

## Resultado final de validación

Después de aplicar las correcciones, la prueba del backend terminó correctamente:

```text
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
BUILD SUCCESS
```

La advertencia relacionada con `byte-buddy-agent` no bloqueó la compilación ni las pruebas.

## Estado del Paso 1

El Paso 1 de la Fase 2 quedó completado con los siguientes resultados:

- El proyecto backend fue generado correctamente dentro de `kontora-pos/backend`.
- La versión de Spring Boot fue corregida a `3.5.15`.
- NetBeans reconoció correctamente el proyecto después de corregir el `pom.xml`.
- El backend quedó conectado a la configuración local de PostgreSQL.
- Maven ejecutó las pruebas iniciales con resultado exitoso.
- La eliminación de `backend/.gitkeep` es válida porque la carpeta `backend` ya contiene un proyecto real.
- La documentación técnica inicial del backend quedó registrada en este archivo.

## Paso 2 - Estructura base de paquetes del backend

### Objetivo

Crear la estructura inicial de paquetes del backend para organizar el código por responsabilidades técnicas y módulos funcionales del sistema Kontora POS.

Este paso no incluye lógica de negocio, entidades, repositorios, servicios ni controladores. Su propósito es dejar preparada la base arquitectónica para que los futuros módulos se desarrollen de forma ordenada dentro del paquete raíz `com.kontora.pos`.

### Ubicación base

Los paquetes fueron creados dentro de:

```text
backend/src/main/java/com/kontora/pos
```

La ruta corresponde al paquete raíz:

```java
package com.kontora.pos;
```

### Paquetes creados

Los paquetes base creados dentro de `com.kontora.pos` fueron:

```text
com.kontora.pos.config
com.kontora.pos.common
com.kontora.pos.common.exception
com.kontora.pos.common.response
com.kontora.pos.security
com.kontora.pos.usuario
com.kontora.pos.producto
com.kontora.pos.inventario
com.kontora.pos.venta
com.kontora.pos.caja
com.kontora.pos.transferencia
com.kontora.pos.gasto
com.kontora.pos.deposito
com.kontora.pos.reporte
```

### Propósito de cada paquete

| Paquete | Propósito |
|---|---|
| `com.kontora.pos.config` | Contendrá configuraciones generales del backend, beans compartidos y ajustes técnicos transversales. |
| `com.kontora.pos.common` | Contendrá clases reutilizables por varios módulos del sistema. |
| `com.kontora.pos.common.exception` | Contendrá excepciones personalizadas y clases para manejo centralizado de errores. |
| `com.kontora.pos.common.response` | Contendrá estructuras comunes de respuesta para mantener consistencia en la API. |
| `com.kontora.pos.security` | Contendrá la configuración futura de seguridad, autenticación, autorización, JWT y filtros. |
| `com.kontora.pos.usuario` | Contendrá la gestión de usuarios, roles, permisos y autenticación funcional. |
| `com.kontora.pos.producto` | Contendrá la gestión de productos, categorías, referencias, tamaños, precios y promociones. |
| `com.kontora.pos.inventario` | Contendrá el control de inventario general, inventario diario, entradas, salidas y ajustes. |
| `com.kontora.pos.venta` | Contendrá el registro de ventas, detalle de ventas, promociones aplicadas y anulaciones permitidas. |
| `com.kontora.pos.caja` | Contendrá la apertura, control, cierre de caja diaria, base, efectivo contado y bloqueo de operaciones posteriores al cierre. |
| `com.kontora.pos.transferencia` | Contendrá el registro, aceptación, rechazo y consulta de transferencias. |
| `com.kontora.pos.gasto` | Contendrá el registro, edición, anulación y consulta de gastos de la jornada. |
| `com.kontora.pos.deposito` | Contendrá el cálculo y registro de depósitos de cierre. |
| `com.kontora.pos.reporte` | Contendrá consultas, reportes administrativos y resúmenes gerenciales. |

### Archivos `package-info.java`

Como Git no versiona carpetas vacías, se creó un archivo `package-info.java` dentro de cada paquete.

Ejemplo para el paquete de ventas:

```java
/**
 * Modulo de ventas del sistema Kontora POS.
 */
package com.kontora.pos.venta;
```

Este tipo de archivo cumple dos funciones:

1. Permite que Git registre la existencia del paquete aunque todavía no tenga clases funcionales.
2. Documenta de forma mínima el propósito del paquete desde el código fuente.

### Estructura esperada después del Paso 2

Después de crear los paquetes base, la estructura dentro de `backend/src/main/java/com/kontora/pos` debe verse de forma similar a:

```text
com/kontora/pos/
├── KontoraPosBackendApplication.java
├── caja/
│   └── package-info.java
├── common/
│   ├── package-info.java
│   ├── exception/
│   │   └── package-info.java
│   └── response/
│       └── package-info.java
├── config/
│   └── package-info.java
├── deposito/
│   └── package-info.java
├── gasto/
│   └── package-info.java
├── inventario/
│   └── package-info.java
├── producto/
│   └── package-info.java
├── reporte/
│   └── package-info.java
├── security/
│   └── package-info.java
├── transferencia/
│   └── package-info.java
├── usuario/
│   └── package-info.java
└── venta/
    └── package-info.java
```

### Validación realizada en el Paso 2

Después de crear los paquetes base, se ejecutó:

```powershell
cd C:\Users\corre\Desktop\kontora-pos\backend
mvn clean test
```

El resultado obtenido fue exitoso:

```text
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
BUILD SUCCESS
```

### Estado del Paso 2

El Paso 2 de la Fase 2 quedó validado con los siguientes resultados:

- La estructura base de paquetes del backend fue creada dentro de `com.kontora.pos`.
- Cada paquete fue preparado con su respectivo archivo `package-info.java`.
- No se agregó lógica de negocio todavía.
- La compilación y las pruebas Maven finalizaron correctamente.
- La arquitectura interna queda lista para iniciar posteriormente la creación de clases comunes, configuración base y módulos funcionales.

## Archivos relacionados con los pasos 1 y 2

Los archivos principales creados o modificados durante estos pasos fueron:

```text
backend/.gitattributes
backend/.gitignore
backend/nbactions.xml
backend/pom.xml
backend/src/main/java/com/kontora/pos/KontoraPosBackendApplication.java
backend/src/main/java/com/kontora/pos/caja/package-info.java
backend/src/main/java/com/kontora/pos/common/package-info.java
backend/src/main/java/com/kontora/pos/common/exception/package-info.java
backend/src/main/java/com/kontora/pos/common/response/package-info.java
backend/src/main/java/com/kontora/pos/config/package-info.java
backend/src/main/java/com/kontora/pos/deposito/package-info.java
backend/src/main/java/com/kontora/pos/gasto/package-info.java
backend/src/main/java/com/kontora/pos/inventario/package-info.java
backend/src/main/java/com/kontora/pos/producto/package-info.java
backend/src/main/java/com/kontora/pos/reporte/package-info.java
backend/src/main/java/com/kontora/pos/security/package-info.java
backend/src/main/java/com/kontora/pos/transferencia/package-info.java
backend/src/main/java/com/kontora/pos/usuario/package-info.java
backend/src/main/java/com/kontora/pos/venta/package-info.java
backend/src/main/resources/application.properties
backend/src/test/java/com/kontora/pos/KontoraPosBackendApplicationTests.java
docs/modules/backend.md
```

## Estado esperado en Git para cerrar el Paso 2

Antes de cerrar el Paso 2 con commit, el estado esperado debe incluir los archivos `package-info.java` de los paquetes nuevos y la actualización de este documento:

```text
Changes to be committed:
    new file:   backend/src/main/java/com/kontora/pos/caja/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/common/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/common/exception/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/common/response/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/config/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/deposito/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/gasto/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/inventario/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/producto/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/reporte/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/security/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/transferencia/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/usuario/package-info.java
    new file:   backend/src/main/java/com/kontora/pos/venta/package-info.java
    modified:   docs/modules/backend.md
```

## Commit sugerido

El commit sugerido para cerrar el Paso 2 es:

```powershell
git commit -m "chore: crear estructura base de paquetes backend"
```

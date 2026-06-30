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

## Paquetes futuros recomendados

Aunque en este paso solo se generó la estructura inicial del proyecto, los paquetes funcionales del backend deberán organizarse progresivamente así:

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

## Archivos relacionados con este paso

Los archivos principales creados o modificados en este paso fueron:

```text
backend/.gitattributes
backend/.gitignore
backend/nbactions.xml
backend/pom.xml
backend/src/main/java/com/kontora/pos/KontoraPosBackendApplication.java
backend/src/main/resources/application.properties
backend/src/test/java/com/kontora/pos/KontoraPosBackendApplicationTests.java
docs/modules/backend.md
```

## Estado esperado en Git

Antes de cerrar el paso con commit, el estado esperado debe incluir los archivos nuevos del backend y este documento:

```text
Changes to be committed:
    new file:   backend/.gitattributes
    new file:   backend/.gitignore
    deleted:    backend/.gitkeep
    new file:   backend/nbactions.xml
    new file:   backend/pom.xml
    new file:   backend/src/main/java/com/kontora/pos/KontoraPosBackendApplication.java
    new file:   backend/src/main/resources/application.properties
    new file:   backend/src/test/java/com/kontora/pos/KontoraPosBackendApplicationTests.java
    new file:   docs/modules/backend.md
```

## Commit sugerido

El commit sugerido para cerrar este paso es:

```powershell
git commit -m "chore: inicializar backend Spring Boot"
```


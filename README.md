# DAW - Despliegue de Aplicaciones Web

Repositorio de trabajo del módulo de Despliegue de Aplicaciones Web (2º DAW).

## Estructura

- `README.md` — este fichero.
- `docs/` — documentación general del módulo.
- `entregas/tema1/` — entregas del Tema 1 (actividades 1.1 y 1.2).
- `escaparate/` — aplicación Java/Maven de ejemplo ("Escaparate") usada como hilo conductor del módulo. Incluye su propio `pom.xml`, el wrapper `mvnw` y su propio `.gitignore`.
- `practicas/` — ficheros técnicos de configuración y despliegue creados durante el módulo.
- `.github/workflows/validar.yml` — comprobación automática que se ejecuta en cada Pull Request hacia `main`: verifica que existen `escaparate/pom.xml`, `escaparate/mvnw` y `practicas/README.md`, y que no se han versionado ficheros `.env`, `target/` ni `.class`.

## Requisitos

- Git instalado y configurado (`user.name` y `user.email`).
- Cuenta de GitHub con acceso al repositorio (privado).
- Maven, para compilar `escaparate/`.

## Flujo de trabajo

Cada sesión se trabaja en una rama propia `sesion-NN` (por ejemplo, `sesion-02`), nunca directamente sobre `main`. Al terminar, se abre una Pull Request `sesion-NN → main`. La comprobación automática (`validar.yml`) debe terminar correctamente antes de fusionar, usando **Create a merge commit**.

## Puesta en marcha

Se completará en sesiones posteriores.

## Entregas

- [Actividad 1.1 - Qué se puede saber de un despliegue desde fuera](entregas/tema1/actividad-1.1/actividad-1.1.md)
- [Actividad 1.2 - Tu repositorio como herramienta de despliegue](entregas/tema1/actividad-1.2/actividad-1.2.md)

## Estado actual

Etiqueta `v0.1.0` publicada, marcando el primer estado estable integrado en `main`.

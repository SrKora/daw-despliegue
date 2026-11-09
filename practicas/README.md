## Puesta en marcha

### Requisitos

- [Docker Engine](https://docs.docker.com/engine/install/) con el plugin **Docker Compose v2**
  (el comando es `docker compose`, sin guion). Compruébalo con:

  ```bash
  docker --version
  docker compose version
  ```

- Puerto **8080** libre en el equipo.

### Ubicación

Toda la configuración del sistema está en `practicas/compose/`:

```
practicas/compose/
├── compose.yaml                  # definición de los servicios bd y app
├── .env.example                  # plantilla de variables (versionada)
├── .env                          # tus valores locales (NO versionado)
└── secrets/
    ├── bd_clave.example.txt      # contraseña ficticia de ejemplo (versionada)
    └── bd_clave.txt              # contraseña real (NO versionada)
```

Todos los comandos siguientes se ejecutan desde esa carpeta:

```bash
cd practicas/compose
```

### 1. Crear la configuración local

Copia las plantillas:

```bash
cp .env.example .env
cp secrets/bd_clave.example.txt secrets/bd_clave.txt
```

Después, edita los dos ficheros:

- **`.env`**: revisa y completa estas variables:

  | Variable      | Qué es                         | Ejemplo      |
  |---------------|--------------------------------|--------------|
  | `DB_NAME`     | Nombre de la base de datos     | `escaparate` |
  | `DB_USER`     | Usuario de PostgreSQL          | `escaparate` |

- **`secrets/bd_clave.txt`**: sustituye la contraseña ficticia del ejemplo por
  una propia. El fichero debe contener **solo la contraseña**, en una línea.

### 2. Ficheros que no deben subirse a Git

Estos ficheros contienen configuración o secretos locales y están en `.gitignore`:

- `practicas/compose/.env`
- `practicas/compose/secrets/bd_clave.txt`

Solo se versionan las plantillas (`.env.example` y `bd_clave.example.txt`).
Antes de hacer commit, comprueba que no aparecen en `git status`.

### 3. Arrancar

```bash
docker compose up -d
```

Compose arranca primero `bd` y **espera a que PostgreSQL esté preparado**
(healthcheck con `pg_isready`) antes de arrancar `app`. La primera vez puede
tardar unos segundos más, porque se inicializa la base de datos y se cargan
los datos de ejemplo.

Comprueba el estado:

```bash
docker compose ps
```

`bd` debe aparecer como `Up (healthy)` y `app` como `Up`.

### 4. Comprobar que funciona

- Catálogo: <http://localhost:8080/>
- Readiness (la aplicación está lista para atender peticiones):

  ```bash
  curl -fsS http://localhost:8080/api/salud/listo
  ```

  Respuesta esperada: `{"estado":"ok"}`

Si algo falla, consulta los logs:

```bash
docker compose logs app
docker compose logs bd
```

### 5. Detener y desmontar

| Comando                  | Qué hace                                                     |
|--------------------------|--------------------------------------------------------------|
| `docker compose stop`    | Detiene los contenedores sin eliminarlos. Se reanudan con `docker compose start`. |
| `docker compose down`    | Elimina contenedores y red. El volumen `datos-bd` se conserva; con `docker compose up -d` vuelve todo como estaba. |
| `docker compose down -v` | Elimina contenedores, red **y volúmenes**.                   |

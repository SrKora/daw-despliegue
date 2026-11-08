## Preguntas

### 1. ¿Con qué herramienta se compila?

Con la propia herramienta de Java, ya que tiene compilador propio. O con el compilador de Maven.

### 2. ¿Qué fichero declara las dependencias?

El `pom.xml`.

### 3. ¿Qué artefacto se genera y en qué directorio?

.jar

### 4. ¿Qué versión de Java necesita?

Java 21 (me imagino que las versiones posteriores también servirán).

### 5. ¿Qué variables utiliza para conectarse a PostgreSQL?

| Variable      | Valor por defecto |
| ------------- | ----------------- |
| `DB_HOST`     | `localhost`       |
| `DB_PORT`     | `15432`           |
| `DB_NAME`     | `escaparate`      |
| `DB_USER`     | `escaparate`      |
| `DB_PASSWORD` | `escaparate`      |

### 6. ¿En qué puerto escucha?

`8080`

### Tiempo de compilacion 

real    0m58.401s
user    0m0.142s
sys     0m0.252s

### Tamaño de la imagen
415MB


### Tiempo de compilacion 4.4

real    1m22.846s
user    0m0.211s
sys     0m0.322s


### Evidencia 2

uid=100(escaparate) gid=101(escaparate) groups=101(escaparate)
  'printf "%s\n" "$APP_STORAGE_PATH"; test -w "$APP_STORAGE_PATH" && echo "writable"'
/app/uploads
writable

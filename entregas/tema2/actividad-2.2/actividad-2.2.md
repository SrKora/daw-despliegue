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


### Tabla de comparar datos

|            | Construcción inicial | Tras modificar código | SIZE final |
|------------|----------------------|-----------------------|------------|
| Ingenua    | 0m58s                | 1m20s                 | 1.06GB     |
| Optimizada | 1m22s                | 1m37s                 | 1.07GB     |


**1. ¿Qué trabajo reutilizó la imagen optimizada que la ingenua tuvo que repetir?**
	El COPY, cualquier cambio obliga a descargar todo de nuevo

**2. ¿Qué contiene la imagen ingenua que la imagen final no necesita?**
	El JDK completo
	Las dependencias descargadas
	El codigo fuente

**3. ¿Qué mejora explica principalmente la reducción del tiempo de reconstrucción?**
	Copiar las dependencias y luego descargarlas, esto varia si el codigo cambia ya 
	que las dependencias seguiran en cache

**4. ¿Qué mejora explica principalmente la reducción de tamaño?**
	Usar dos etapas.

**5. ¿Qué decisiones mejoran la seguridad aunque no reduzcan necesariamente el tiempo de build?**
	Usar un usuario sin privilegios
	Excluir el `.env` en el `.dockerignore` 
	Reducir los permisos de escritura

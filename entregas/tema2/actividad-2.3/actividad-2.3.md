### Evidencia 1
```
app-1  | 2026-11-08T19:45:11.749Z  WARN 1 --- [escaparate] [           main] org.hibernate.orm.jdbc.error             : The connection attempt failed.
app-1  | 2026-11-08T19:45:11.750Z  WARN 1 --- [escaparate] [           main] org.hibernate.orm.jdbc                   : HHH100046: Could not obtain connection to query JDBC database metadata
app-1  | org.hibernate.exception.JDBCConnectionException: Unable to obtain isolated JDBC connection [The connection attempt failed.] [n/a]
app-1  | Caused by: org.postgresql.util.PSQLException: The connection attempt failed.
app-1  |        at org.postgresql.core.v3.ConnectionFactoryImpl.openConnectionImpl(ConnectionFactoryImpl.java:437) ~[postgresql-42.7.11.jar!/:42.7.11]
app-1  |        at org.postgresql.core.ConnectionFactory.openConnection(ConnectionFactory.java:52) ~[postgresql-42.7.11.jar!/:42.7.11]
app-1  |        at org.postgresql.jdbc.PgConnection.<init>(PgConnection.java:290) ~[postgresql-42.7.11.jar!/:42.7.11]
app-1  |        at org.postgresql.Driver.makeConnection(Driver.java:448) ~[postgresql-42.7.11.jar!/:42.7.11]
app-1  |        at org.postgresql.Driver.connect(Driver.java:298) ~[postgresql-42.7.11.jar!/:42.7.11]
app-1  |        at com.zaxxer.hikari.util.DriverDataSource.getConnection(DriverDataSource.java:144) ~[HikariCP-7.0.2.jar!/:na]
app-1  |        at com.zaxxer.hikari.pool.PoolBase.newConnection(PoolBase.java:373) ~[HikariCP-7.0.2.jar!/:na]
app-1  |        at com.zaxxer.hikari.HikariDataSource.getConnection(HikariDataSource.java:111) ~[HikariCP-7.0.2.jar!/:na]
app-1  |        at org.hibernate.engine.jdbc.connections.internal.DataSourceConnectionProvider.getConnection(DataSourceConnectionProvider.java:149) ~[hibernate-core-7.4.1.Final.jar!/:7.4.1.Final]
app-1  |        at org.hibernate.engine.jdbc.env.internal.JdbcEnvironmentInitiator$ConnectionProviderJdbcConnectionAccess.obtainConnection(JdbcEnvironmentInitiator.java:508) ~[hibernate-core-7.4.1.Final.jar!/:7.4.1.Final]
`app-1  | Caused by: java.net.UnknownHostException: postgres`
app-1  |        at org.postgresql.core.PGStream.createSocket(PGStream.java:271) ~[postgresql-42.7.11.jar!/:42.7.11]
app-1  |        at org.postgresql.core.PGStream.<init>(PGStream.java:132) ~[postgresql-42.7.11.jar!/:42.7.11]
app-1  |        at org.postgresql.core.v3.ConnectionFactoryImpl.tryConnect(ConnectionFactoryImpl.java:219) ~[postgresql-42.7.11.jar!/:42.7.11]
app-1  |        at org.postgresql.core.v3.ConnectionFactoryImpl.openConnectionImpl(ConnectionFactoryImpl.java:365) ~[postgresql-42.7.11.jar!/:42.7.11]
app-1  | Caused by: org.hibernate.service.spi.ServiceException: Unable to create requested service [org.hibernate.engine.jdbc.env.spi.JdbcEnvironment] due to: Unable to determine Dialect without JDBC metadata (please set 'jakarta.persistence.jdbc.url' for common cases or 'hibernate.dialect' when a custom Dialect implementation must be provided)
kora@Kora:~/daw-despliegue/practicas/compose$
```
### Evidencia 2

```
kora@Kora:~/daw-despliegue/practicas/compose$ docker compose ps
NAME            IMAGE                                 COMMAND                  SERVICE   CREATED          STATUS          PORTS
compose-app-1   ghcr.io/srkora/escaparate:sesion-04   "java -jar /app/app.…"   app       21 seconds ago   Up 21 seconds   0.0.0.0:8080->8080/tcp, [::]:8080->8080/tcp
compose-bd-1    ghcr.io/srkora/escaparate-db:1.0.0    "docker-entrypoint.s…"   bd        2 minutes ago    Up 2 minutes    0.0.0.0:5433->5432/tcp, [::]:5433->5432/tcp
```

### Paso 2
**Antes de añadir el producto**
El producto se añadira con normalidad incluidas las fotos y demás información

**Después de añadir el producto**
El producto se ha añadido con normalidad, con nombre, foto, descripción y precio.

### 2.1. Detener y volver a arrancar
El producto se mantiene una vezz hemos parado e iniciado el contenedor

### 2.2. Eliminar y recrear contenedores
El producto se mantiene una vezz hemos eliminado y recreado el contendor, pero en cambio esta vez la imagen no se ha mantenido(supongo que será porque no guarda la carpeta de imagenes)

### 2.3. Eliminar también el volumen
El producto hah desaparecido por completo, ya que al hacer el -v también hemos eliminado los volumenes

### Evidencia 3
```
kora@Kora:~/daw-despliegue/practicas/compose$ git check-ignore -v .env secrets/bd_clave.txt
git ls-files -- .env secrets/bd_clave.txt
.gitignore:10:/practicas/compose/.env   .env
.gitignore:11:/practicas/compose/secrets/bd_clave.txt   secrets/bd_clave.txt

kora@Kora:~/daw-despliegue/practicas/compose$ docker compose exec -T app \
  sh -c 'test -r /run/secrets/spring.datasource.password && test -s /run/secrets/spring.datasource.password && echo "Secreto accesible y no vacío"'
Secreto accesible y no vacío
```
### Evidencia 4
```
kora@Kora:~/daw-despliegue/practicas/compose$ docker compose ps
NAME            IMAGE                                 COMMAND                  SERVICE   CREATED         STATUS                   PORTS
compose-app-1   ghcr.io/srkora/escaparate:sesion-04   "java -jar /app/app.…"   app       2 minutes ago   Up About a minute        0.0.0.0:8080->8080/tcp, [::]:8080->8080/tcp
compose-bd-1    ghcr.io/srkora/escaparate-db:1.0.0    "docker-entrypoint.s…"   bd        2 minutes ago   Up 2 minutes (healthy)   5432/tcp

kora@Kora:~/daw-despliegue/practicas/compose$ for i in {1..30}; do
    if curl -fsS http://localhost:8080/api/salud/listo; then
        break
    fi
    sleep 2
done
{"estado":"ok"}kora@Kora:~/daw-despliegue/practicas/compose$
```

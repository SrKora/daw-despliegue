\### Evidencia 1
```
kora@Kora:~/daw-despliegue/practicas/compose$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS                    PORTS                                 NAMES
f3305dad1915   nginx:1.30.4-alpine                   "/docker-entrypoint.…"   43 seconds ago   Up 36 seconds             0.0.0.0:80->80/tcp, [::]:80->80/tcp   compose-web-1
```
### Resolución DNS de `escaparate.127.0.0.1.nip.io`

| Dato              | Valor                     |
|-------------------|---------------------------|
| Tipo de registro  | `A`                       |
| Dirección         | `127.0.0.1`               |
| TTL               | `3600` segundos (1 hora)  |

- **Tipo A:** asocia un nombre de dominio a una dirección IPv4
- **Dirección 127.0.0.1:** es la propia máquina (loopback)
- **TTL 3600:** durante una hora, los resolvers pueden guardar esta respuesta en
  caché antes de volver a consultarla

### Evidencia 2

![foto de evidencia 2 sobre escaparate y documetnacion](img/evidencia-1.png)

### Comprobacion de redireccion

**Antes**
Cuando alguien hacia una peticion hacia cualquier lugar resolvia a `127.0.0.1` 
y lo redireccionaba directamente al pirmer bloque del server que es el catalogo

**Después**

Ahora se añadió el bloque de server name `_` para que si se hace una petición a un
nombre que no existe en el nginx redirecciona a un `404`

### Evidencia 3

```
kora@Kora:~/daw-despliegue/practicas/compose$ curl -I \
  -H "Accept-Encoding: gzip" \
  http://escaparate.127.0.0.1.nip.io/css/app.css
HTTP/1.1 200 OK
Server: nginx/1.30.4
Date: Tue, 10 Nov 2026 14:55:24 GMT
Content-Type: text/css
Last-Modified: Mon, 31 Dec 1979 23:00:00 GMT
Connection: keep-alive
Vary: Accept-Encoding
ETag: W/"12ce97f0-17c3"
Expires: Tue, 17 Nov 2026 14:55:24 GMT
Cache-Control: max-age=604800
Content-Encoding: gzip

kora@Kora:~/daw-despliegue/practicas/compose$ curl -s \
  -H "Accept-Encoding: identity" \
  -o /dev/null \
  -w "sin comprimir: %{size_download} bytes\n" \
  http://escaparate.127.0.0.1.nip.io/css/app.css

curl -s \
  -H "Accept-Encoding: gzip" \
  -o /dev/null \
  -w "comprimido:   %{size_download} bytes\n" \
  http://escaparate.127.0.0.1.nip.io/css/app.css
sin comprimir: 6083 bytes
comprimido:   2066 bytes
kora@Kora:~/daw-despliegue/practicas/compose$ curl -s -D - -o /dev/null \
  http://escaparate.127.0.0.1.nip.io/api/salud/listo
HTTP/1.1 200
Server: nginx/1.30.4
Date: Tue, 10 Nov 2026 14:56:31 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding

kora@Kora:~/daw-despliegue/practicas/compose$ curl -i http://cualquier-cosa.127.0.0.1.nip.io/
HTTP/1.1 404 Not Found
Server: nginx/1.30.4
Date: Tue, 10 Nov 2026 14:56:44 GMT
Content-Type: text/html
Content-Length: 153
Connection: keep-alive

<html>
<head><title>404 Not Found</title></head>
<body>
<center><h1>404 Not Found</h1></center>
<hr><center>nginx/1.30.4</center>
</body>
</html>
```

## Compresión y caché

Medición sobre `css/app.css`:

| Petición                          | Tamaño transferido |
|-----------------------------------|--------------------|
| Sin `Accept-Encoding: gzip`       | 6083 bytes         |
| Con `Accept-Encoding: gzip`       | 2066 bytes         |

Reducción: 4017 bytes (≈ 66 %). El archivo comprimido ocupa ≈ 34 % del original

Cabeceras obtenidas:

Content-Encoding: gzip
Vary: Accept-Encoding
Cache-Control: max-age=604800
Expires: Tue, 17 Nov 2026 15:00:08 GMT


### Reflexión

- **gzip** reduce mucho el tamaño de los ficheros de texto (CSS, JS, JSON),
  Solo se comprime si el cliente lo pide con `Accept-Encoding: gzip`

- **`Vary: Accept-Encoding`** indica a navegadores y cachés intermedias que
  hay dos versiones del recurso (comprimida y sin comprimir) y no deben
  mezclarlas

- Las imágenes no se incluyen en `gzip_types` porque ya están comprimidas y
  apenas se reducirían

- **`expires 7d`** genera `Cache-Control: max-age=604800` y `Expires`: el
  navegador puede reutilizar los estáticos durante 7 días sin volver a pedirlos

- La caché solo se aplica a los estáticos, no a `/api/`, porque los datos del
  catálogo cambian y deben pedirse siempre a la aplicación

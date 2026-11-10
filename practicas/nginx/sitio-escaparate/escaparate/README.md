# Escaparate — frontend servido por Nginx

Estos archivos HTML, CSS y JavaScript se publican con Nginx. En la actividad, el catálogo obtiene los productos de la API de Escaparate; no funciona como un catálogo autónomo sin backend.

`config.js` selecciona el modo `api` y la ruta relativa `/api`. Nginx sirve los archivos y reenvía las peticiones `/api/` a la aplicación, conectada a PostgreSQL. El archivo `data/productos.json` es un ejemplo incluido en la distribución y no se utiliza en este modo.

Extrae el ZIP en `practicas/nginx/sitio-escaparate/`: debe quedar `practicas/nginx/sitio-escaparate/escaparate/index.html`. Sigue la actividad 3.1 para configurar Nginx y abre el sitio mediante HTTP. No abras el HTML con `file://`.

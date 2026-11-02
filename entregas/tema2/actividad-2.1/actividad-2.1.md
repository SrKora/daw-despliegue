# Actividad 2.1
 
## ¿Por qué `stop` y `start` conservaron el cambio?
 
Porque `stop` solo para el contenedor, no lo borra. Al hacer `start` vuelve el mismo contenedor con sus ficheros.
 
## ¿Por qué `web-1` y `web-2` podían contener ficheros diferentes?
 
Porque cada contenedor tiene su propia copia de los ficheros. Lo que cambias en uno no afecta al otro, aunque vengan de la misma imagen.
 
## ¿Por qué eliminar y recrear hizo desaparecer la modificación?
 
Porque al eliminar el contenedor se borran sus cambios. El nuevo `web-1` empieza desde cero con los ficheros originales de la imagen.
 
## ¿Qué problema tendría este comportamiento si el fichero modificado contuviera datos importantes?
 
Que se perderían al borrar el contenedor. Para evitarlo hay que guardar los datos fuera del contenedor, por ejemplo con volúmenes.
 

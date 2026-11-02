# Actividad 1.2

## Qué he decidido excluir del repositorio

En el `.gitignore` de la raíz he excluido:

- `target/` → la carpeta que genera Maven al compilar. Se puede regenerar, no hace falta guardarla.
- `*.class` → ficheros compilados sueltos.
- `.env` → ahí van contraseñas y claves, no debe subirse nunca.
- Carpetas de IDEs (`.idea/`, `.vscode/`...) → son ajustes personales de cada editor, no del proyecto.
- `escaparate/uploads/` → imágenes que se generan al probar la app, no es código.

Lo comprobé con `git check-ignore -v` y esos ficheros de prueba quedaron bien ignorados.

## Recuperación de errores

### Cambio no preparado
- **Zona afectada:** directorio de trabajo
- **Comando:** `git restore README.md`
- **Por qué:** el cambio solo estaba en el fichero, sin hacer `add`. Este comando lo descarta y recupera el último commit tal cual.

### Cambio preparado
- **Zona afectada:** área de preparación (staging)
- **Comando:** `git restore --staged README.md`
- **Por qué:** ya había hecho `add`. Este comando lo saca del "listo para commitear" pero sin borrar lo escrito.

### Commit publicado
- **Zona afectada:** historial ya compartido (remoto)
- **Comando:** `git revert <hash>`
- **Por qué:** crea un commit nuevo que deshace el error, sin borrar ni tocar el commit original. Así no rompo el historial si alguien más ya se había descargado la rama.

## Reflexiones

- Añadir un fichero a `.gitignore` **no lo borra** si ya estaba en commits anteriores. Sigue en el historial. Si fuera una contraseña real, no bastaría con borrarla después: habría que darla por comprometida y cambiarla.
- Cuando un error ya está publicado, conviene conservar el commit original y corregir con uno nuevo (`revert`), porque reescribir el historial podría romper el trabajo de otra persona que ya lo tenga descargado.
- La comprobación automática de hoy es básica. Antes de un despliegue real también tendría sentido pasar tests, revisar que el código compila bien, y buscar vulnerabilidades en las dependencias.
- Etiquetar una versión (`v0.1.0`) permite señalar siempre el mismo estado exacto del repositorio. Decir solo "lo que haya en `main`" no sirve, porque `main` cambia con el tiempo.

## Evidencias

![Evidencia 1: git check-ignore](img/evidencia-1-check-ignore.png)

![Evidencia 2: historial con el error y su corrección](img/evidencia-2-historial-revert.png)

![Evidencia 3: Pull Request](img/evidencia-3-pull-request.png)

![Evidencia 4: etiqueta v0.1.0](img/evidencia-4-tag.png)
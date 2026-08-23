# Lista de liberación

Esta lista prepara un ZIP instalable privado o una futura publicación. No
publica ni hace push por sí sola.

## Código y calidad

- [ ] Build `Editor Development` de UE 5.4 termina correctamente.
- [ ] No hay warnings nuevos relevantes ni código temporal de depuración.
- [ ] `git diff --check` pasa.
- [ ] Todos los cambios funcionales tienen una prueba manual asociada.
- [ ] Se registra la versión y los cambios visibles en `CHANGELOG.md`.

## Contenido y dependencias

- [ ] El plugin no contiene arte descargado ni Assets del juego host.
- [ ] Referencias a actores artísticos, como una antorcha, son blandas y se
  explican al usuario.
- [ ] No se versionan `Binaries`, `Intermediate`, `Saved`, `.vs` ni soluciones
  generadas.
- [ ] `DungeonBlueprintForge.uplugin` tiene versión, EngineVersion y descripción correctas.

## Prueba de instalación

- [ ] Empaquetar desde `Edit > Plugins > Package...`.
- [ ] Extraer el resultado en `ProyectoVacio/Plugins/DungeonBlueprintForge/`.
- [ ] Abrir con UE 5.4, activar el plugin y compilar.
- [ ] Crear Config, Definition, Room y Generator siguiendo la guía sin usar
  Assets del laboratorio.
- [ ] Generar una seed fija y una aleatoria; probar salas Rectangle, L y T.

## Antes de GitHub/Fab

- [ ] README explica qué hace, requisitos, límites e instalación.
- [ ] Las guías y enlaces se leen correctamente desde GitHub.
- [ ] La referencia cubre cada Data Asset y Actor público.
- [ ] No hay documentación rota por codificación ni capturas con datos privados.
- [ ] Revisar `git status`, rama, diff y archivos incluidos antes de commit/push.

Para Fab harán falta además sus requisitos de empaquetado, versión de motor y
revisión externa. No es necesario publicar en Fab para que el plugin aparezca
en la sección Plugins de un proyecto.

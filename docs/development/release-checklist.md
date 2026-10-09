# Lista de liberación

Esta lista prepara un ZIP instalable privado o una futura publicación. No
publica ni hace push por sí sola.

## Código y calidad

- [x] Builds `DungeonLab54Editor` y `DungeonLab54` Win64 Development de UE 5.4 terminan correctamente tras la última corrección C++ (2026-09-27).
- [x] La copia privada de UE 5.8.3 compila Editor, Game Development y `BuildPlugin` Win64 (2026-10-09).
- [ ] Resolver el `CookAll` de UE 5.8 y repetirlo sin errores; revisar `CR_Mannequin_Procedural`, `HeroAnin` y los avisos UDIM del laboratorio.
- [ ] Abrir la copia UE 5.8 en el editor y probar una seed fija, Packed Rooms y el encaje visual de puertas.
- [ ] No hay warnings nuevos relevantes ni código temporal de depuración.
- [ ] `git diff --check` pasa.
- [ ] Todos los cambios funcionales tienen una prueba manual asociada.
- [ ] Probar Rooms Packed nuevas con bounds, 2--4 salidas, puertas libres,
  offsets y escala; repetir una seed fija.
- [ ] Probar `Direct Contact` staged: un marco por abertura y limpieza tras
  un fallo de presentación.
- [x] Barrida visual del laboratorio completada de 5 a 70 Rooms (2026-09-19).
- [ ] Repetir una seed fija después del build final de cleanup.
- [ ] Validar `Free Expansion`, `Adaptive Floors` y `Automatic` con seeds fijas.
- [ ] Confirmar huella XY, transición Stairwell, recorrido, colisión y NavMesh.
- [ ] Confirmar que el debug de huella se puede activar y desactivar sin cambiar el layout.
- [ ] Probar 55 normales con huella 12000x12000 y medir el tiempo de generación.
- [ ] Confirmar que suelos y techos de plantas contiguas no se solapan.
- [ ] Probar 65 normales y confirmar la reubicación de Hubs/Stairwell diferidos.
- [ ] Probar 69 normales y confirmar el máximo de 75 Rooms totales.
- [ ] Confirmar que la auditoría final rechaza cruces de pasillos e invasiones laterales.
- [ ] Repetir una seed problemática con backtracking 0 y 2, y confirmar que 2 conserva el total de normales cuando recupera Hub o Key.
- [ ] Registrar el tiempo mostrado por `Print Generation Retry Debug` y comprobar que el layout final conserva colisión, NavMesh y luces de pasillo.
- [ ] Con `stat unit` y `stat gpu`, confirmar que un mensaje amarillo de backtracking no coincide con creación repetida de HISM de pasillo.
- [ ] Registrar `Performance Stats`, especialmente `Planning`, `Room Build`, `Deferred Content` y `Slowest Staged Item`, en hardware objetivo.
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
- [ ] Probar una Packed Room en una build empaquetada y confirmar que conserva
  la huella precisa guardada en su Blueprint.

## Antes de GitHub/Fab

- [ ] README explica qué hace, requisitos, límites e instalación.
- [ ] Las guías y enlaces se leen correctamente desde GitHub.
- [ ] La referencia cubre cada Data Asset y Actor público.
- [ ] No hay documentación rota por codificación ni capturas con datos privados.
- [ ] Revisar `git status`, rama, diff y archivos incluidos antes de commit/push.

Para Fab harán falta además sus requisitos de empaquetado, versión de motor y
revisión externa. No es necesario publicar en Fab para que el plugin aparezca
en la sección Plugins de un proyecto.

# Dungeon Blueprint Forge

Plugin de Unreal Engine 5.4 para construir mazmorras modulares, reproducibles por *seed* y controlables desde Blueprint. El plugin resuelve la distribución, las conexiones, los pasillos rectos y la validación de solapes; tu juego sigue siendo dueño del jugador, combate, UI, inventario y arte.

> Estado: desarrollo activo. Antes de publicar una versión se ejecutará la lista de liberación y las pruebas visuales de Unreal.

## Empieza por aquí

| Si quieres... | Lee... |
|---|---|
| Instalarlo y comprobar que funciona | [Instalación y primera mazmorra](docs/guides/01-installation-and-first-dungeon.md) |
| Conectarlo a un mapa mediante Blueprints | [Implementación Blueprint paso a paso](docs/guides/02-blueprint-implementation.md) |
| Crear salas y contenido reutilizable | [Crear salas](docs/guides/03-authoring-rooms.md) |
| Entender los ajustes de la sala procedural | [Ajustes de salas procedurales, explicados](docs/guides/04-procedural-room-settings.md) |
| Entender cada Data Asset | [Referencia de Data Assets](docs/reference/data-assets.md) |
| Configurar Generator, Room y componentes | [Referencia de Actors y componentes](docs/reference/actors-and-components.md) |
| Preparar una versión para otra persona o para Fab | [Lista de liberación](docs/development/release-checklist.md) |

## Qué incluye hoy

- Generación determinista desde `Seed`, con reintentos de distribución si una combinación no cabe.
- Salas `Start`, `Normal`, `Hub`, `Reward`, `Key` y `Boss` elegidas mediante Data Assets y pesos.
- Salas prehechas y salas modulares `Rectangle`, `L` y `T`.
- Pasillos horizontales rectos, con protección contra cruces e invasiones de salas.
- Paredes, suelo, techo, pilares, decoración HISM, antorchas de proyecto host y una luz de relleno opcional sin sombras.

## Límites actuales

No hay pasillos curvos/en L, verticalidad, bucles entre salas ni replicación multijugador terminada. Una luz, malla o Blueprint artístico pertenece al proyecto que usa el plugin y se asigna mediante referencias blandas.

## Este repositorio contiene solo documentación

```text
docs/guides/     Tutoriales de uso.
docs/reference/  Referencia de Data Assets, Actors y componentes.
docs/development/ Arquitectura, pruebas y publicación.
docs/images/     Capturas que acompañan los tutoriales.
```

No contiene el plugin, código fuente, `.uplugin`, Assets ni binarios. La [documentación completa](docs/README.md) mantiene el índice.

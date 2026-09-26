# Documentación de Dungeon Blueprint Forge

Esta carpeta está organizada por intención. Empieza por una guía; utiliza la referencia solo cuando quieras entender un campo concreto.

## Guías para usar el plugin

1. [Recorrido completo de usuario](guides/00-complete-user-workflow.md)
2. [Instalación y primera mazmorra](guides/01-installation-and-first-dungeon.md)
3. [Implementación Blueprint paso a paso](guides/02-blueprint-implementation.md)
4. [Crear salas prehechas y procedurales](guides/03-authoring-rooms.md)
5. [Ajustes de salas procedurales, explicados](guides/04-procedural-room-settings.md)
6. [Iluminación y marcos de puerta en pasillos](guides/05-corridor-lighting-and-door-frames.md)
7. [Cofres procedurales](guides/06-procedural-chests.md)
8. [Generación multijugador](guides/07-multiplayer-generation.md)
9. [Encuentros de enemigos en Blueprints del proyecto host](guides/08-host-enemy-encounters-blueprints.md)
10. [Sala Stairwell: subir una planta](guides/09-procedural-stairwell.md)
11. [Diagnóstico y errores frecuentes](guides/10-troubleshooting.md)
12. [Generación staged y rendimiento](guides/11-staged-generation-and-performance.md)
13. [Rooms prehechas con Packed Level Actor](guides/12-prebuilt-packed-level-actor-rooms.md)

La guía Stairwell y la referencia de Data Assets incluyen el prototipo de
`Adaptive Floors`, con límites XY por planta, selección dedicada de Stairwell,
modo automático y debug visual. Está compilado en el checkpoint privado de
desarrollo. También incluye preparación ligera de Rooms modulares y fallback de
conexiones para layouts compactos. El laboratorio completó una barrida visual
de 5 a 70 Rooms; NavMesh, red y hardware modesto siguen requiriendo
pruebas dedicadas.

Packed Rooms con `Automatic Bounds` y puertas de salidas libres ya se probaron
visualmente en el laboratorio. El ajuste de escala/desplazamiento completo,
las nuevas variantes y una build empaquetada aún necesitan prueba manual.

## Referencia

- [Data Assets](reference/data-assets.md): qué crear y qué significa cada ajuste de configuración, definición de sala y estilo de pasillo.
- [Actors y componentes](reference/actors-and-components.md): Generator, Room Base, Modular Room, Corridor y componentes de Bounds/Connection/Marker.

## Desarrollo y publicación

- [Lista de liberación](development/release-checklist.md)
- [Estado de validación del 2026-09-26](development/validation-status-2026-09-26.md)

## Alcance público

Este repositorio publica documentación e imágenes de uso. El código, el
archivo `.uplugin`, los Assets y las versiones instalables se mantienen en el
canal privado de distribución.

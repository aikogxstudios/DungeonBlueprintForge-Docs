# Dungeon Blueprint Forge — Unreal Engine 5.8

![Unreal Engine 5.8](https://img.shields.io/badge/Unreal%20Engine-5.8-0E1128?logo=unrealengine&logoColor=white)
![Solo documentación](https://img.shields.io/badge/Repositorio-Solo%20documentaci%C3%B3n-5B3CC4)

Generador de mazmorras modulares y reproducibles por seed, controlable desde
Blueprint. **La versión de trabajo actual es UE 5.8.3**. Las referencias a
UE 5.4 aparecen únicamente en resultados históricos.

Este repositorio contiene **guías e imágenes**. Para instalar el plugin necesitas
obtener su distribución privada autorizada; aquí no se publica código, binarios
ni assets Unreal del proyecto.

## Empieza aquí

1. [Guía de uso paso a paso](docs/guides/00-complete-user-workflow.md): qué crear, dónde se configura y cómo generar.
2. [Todas las opciones del editor](docs/reference/editor-options.md): 225 campos explicados en español, con sus valores iniciales.
3. [Instalación](docs/guides/01-installation-and-first-dungeon.md) y [flujo Blueprint](docs/guides/02-blueprint-implementation.md).
4. [Índice completo](docs/README.md) y [estado de UE 5.8](docs/development/ue58-validation-2026-10-09.md).

## Qué hace

- Selecciona salas Start, Normal, Key, Boss, Hub y Reward mediante Data Assets.
- Construye salas procedurales Rectangle, L y T, o utiliza tu arte prehecho
  mediante Packed Rooms.
- Conecta salas con pasillos rectos o contacto directo.
- Conecta alturas con Stairwell y puede limitar la huella XY por planta.
- Genera opcionalmente decoración, luces, cofres del proyecto y marcos.
- Permite marcar cada salida que ya trae marco para evitar duplicarlo.
- Conserva el tamaño del actor que cierra una salida Packed libre: solo lo centra.
- Usa instancias HISM para geometría repetida y presentación Staged opcional.

Tu juego controla arte, personajes, combate, UI, inventario y enemigos. Las
opciones de Gameplay Zone comunican metadatos; no crean un sistema de combate.

## Dónde configuro cada cosa

| Objetivo | Lugar | Guía |
|---|---|---|
| Elegir salas, cantidad y expansión | Generation Config | [Uso completo](docs/guides/00-complete-user-workflow.md) |
| Tamaño, forma, pilares, props y luces | Blueprint Modular Room | [Ajustes procedurales](docs/guides/04-procedural-room-settings.md) |
| Arte prehecho y salidas libres | Blueprint Packed Room | [Packed Rooms](docs/guides/12-prebuilt-packed-level-actor-rooms.md) |
| Pasillos y marcos | Corridor / Door Frame Style | [Pasillos y marcos](docs/guides/05-corridor-lighting-and-door-frames.md) |
| Cofres | Chest Spawn Style | [Cofres](docs/guides/06-procedural-chests.md) |
| Escaleras y plantas | Stairwell y Generation Config | [Stairwell](docs/guides/09-procedural-stairwell.md) |
| Presentación progresiva | Nodos Staged y Generation Config | [Rendimiento](docs/guides/11-staged-generation-and-performance.md) |
| Diagnosticar un fallo | Last Result y seed | [Diagnóstico](docs/guides/10-troubleshooting.md) |

## Estado y alcance

La versión del plugin se conserva en 0.10.1 mientras se revisa su aceptación.
El límite actual es 69 salas normales y 75 salas totales. La seed reproduce
el resultado si mantienes assets y configuración; no garantiza que cualquier
combinación de rooms quepa.

Los resultados de compilación y pruebas están en [la revisión de mantenimiento](docs/development/maintenance-review-2026-10-09.md).
La comprobación visual del nuevo panel, red, NavMesh y empaquetado completo
tienen pruebas propias. El CookAll anterior conserva errores de dos assets del
laboratorio descritos en [el estado de UE 5.8](docs/development/ue58-validation-2026-10-09.md).

[AikoGx Studios](https://aikogxstudios.itch.io/)

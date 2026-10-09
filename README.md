# Dungeon Blueprint Forge

> A Blueprint-friendly modular dungeon generator. Unreal Engine 5.4 is the
> documented checkpoint; a separate Unreal Engine 5.8 development copy is under validation.

![Unreal Engine 5.4](https://img.shields.io/badge/Unreal%20Engine-5.4-0E1128?logo=unrealengine&logoColor=white)
![Unreal Engine 5.8](https://img.shields.io/badge/Unreal%20Engine-5.8%20validation-orange?logo=unrealengine&logoColor=white)
![Documentation](https://img.shields.io/badge/Repository-Documentation%20Only-5B3CC4)
![Status](https://img.shields.io/badge/Status-0.10.1%20Checkpoint-2EA44F)

**Dungeon Blueprint Forge** is a reusable Unreal Engine plugin for building
modular, seed-based dungeons from Blueprints. It handles the technical layout:
selecting rooms, placing them safely, connecting doors with corridors and
building modular room geometry. Your game keeps control of its own art,
characters, combat, UI, inventory and gameplay rules.

This repository is the public documentation portal for the plugin. It is made
for designers and Blueprint users who want to understand the workflow before
using the private plugin build.

**To use the plugin, obtain its authorized private build separately.** This
repository contains guides and images, not an installable plugin. Start with
[installation and your first dungeon](docs/guides/01-installation-and-first-dungeon.md);
the [validation status](docs/development/validation-status-2026-09-26.md) lists
features that still need testing before production use. The step-by-step guides
are in Spanish.

The separate UE 5.8.3 copy has passed C++ builds and a referenced-package cook,
but full asset cooking and visual acceptance remain open. See the
[UE 5.8 migration status](docs/development/ue58-validation-2026-10-09.md).

## What problem does it solve?

Building a different dungeon layout by hand for every play session is slow and
hard to maintain. Dungeon Blueprint Forge gives you a structured way to create
reusable room Blueprints and let a deterministic seed assemble them into a
repeatable dungeon.

The same seed and configuration reproduce the same result or failure. That makes testing,
debugging, sharing layouts and reproducing a player report much easier.

## What the plugin does

- Generates deterministic dungeon layouts from a `Seed`.
- Chooses `Start`, `Normal`, `Hub`, `Reward`, `Key` and `Boss` rooms through
  Data Assets and selection weights.
- Supports handcrafted rooms and modular procedural rooms in `Rectangle`, `L`
  and `T` shapes.
- Connects compatible doors with straight horizontal corridors.
- Connects lower and upper floors with `DBF Stairwell Room`, using complete stair meshes and automatic low/high connections.
- Documents development modes `Free Expansion`, `Adaptive Floors` and
  `Automatic`, with bounded XY footprints, deterministic size variation,
  dedicated Stairwell Definitions and optional green footprint debugging.
- Validates room overlap and protects rooms from corridor invasions and
  corridor crossings.
- Builds floor, walls, ceilings and repeated decorative geometry with HISM for
  efficient rendering.
- Generates optional wall decorations, host-project torch and chest Actors,
  shadowless fill lights, and generic `Gameplay Zone` metadata.
- Exposes the generation flow through Blueprint-friendly Actors, Components,
  Data Assets and events.

## How it fits into your game

```text
Your modular meshes + room Blueprints + Data Assets
                         ↓
              Dungeon Blueprint Forge
       layout, validation, rooms and corridors
                         ↓
      Your game: player, AI, loot, combat, UI and art
```

The plugin never requires you to move your artistic Assets into it. Meshes,
materials, decorations and torch Blueprints remain in your project and are
assigned from the Unreal Details panel.

## Start here

| Your goal | Read this |
|---|---|
| Understand the complete first-use flow | [Installation and first dungeon](docs/guides/01-installation-and-first-dungeon.md) |
| Return to the plugin or start a complete project | [Complete user workflow](docs/guides/00-complete-user-workflow.md) |
| Connect the Generator to your map using Blueprints | [Blueprint implementation, step by step](docs/guides/02-blueprint-implementation.md) |
| Create a handcrafted or modular room | [Author rooms](docs/guides/03-authoring-rooms.md) |
| Configure room size, decorations, banners, torches and fill light | [Procedural room settings, explained](docs/guides/04-procedural-room-settings.md) |
| Light corridors and add multi-material door frames | [Corridor lighting and door frames](docs/guides/05-corridor-lighting-and-door-frames.md) |
| Add optional procedural chests | [Procedural chests](docs/guides/06-procedural-chests.md) |
| Build upper and lower floors | [Stairwell room](docs/guides/09-procedural-stairwell.md) |
| Connect host-project encounters | [Host enemy encounters](docs/guides/08-host-enemy-encounters-blueprints.md) |
| Generate large dungeons across frames | [Staged generation and performance](docs/guides/11-staged-generation-and-performance.md) |
| Use a Packed Level Actor as a room | [Packed rooms, automatic bounds and exit doors](docs/guides/12-prebuilt-packed-level-actor-rooms.md) |
| Understand every Data Asset | [Data Asset reference](docs/reference/data-assets.md) |
| Understand Generator, Room and Component options | [Actor and Component reference](docs/reference/actors-and-components.md) |
| Check what has been validated | [Validation status](docs/development/validation-status-2026-09-26.md) |
| Follow the Unreal 5.8 migration | [UE 5.8 validation status](docs/development/ue58-validation-2026-10-09.md) |

## Main workflow

1. Create a child Blueprint from `DungeonBlueprintForgeModularRoom`, or create
   a handcrafted room from `DungeonBlueprintForgeRoomBase`.
2. Assign your meshes, set the room's doors and validate it.
3. Create Room Definition and Generation Config Data Assets.
4. Place one `DungeonBlueprintForgeGenerator` in your map and assign the
   Generation Config.
5. Generate from a known seed, inspect the result, then use the resolved seed
   to reproduce it whenever necessary.

You can begin with a simple rectangular room and expand gradually: first room
layout, then doors, then decoration, then lighting.

## Designed for performance-conscious projects

- Repeated static room and decoration meshes use Hierarchical Instanced Static
  Meshes rather than one Actor per piece.
- Modular rooms use lightweight logical preparation while a layout is being
  searched. Mesh instances, collision, navigation, decoration and lights are
  built once after the topology is accepted.
- Staged generation can preload soft references, build Start first and spread
  connected Rooms, corridors and deferred content across frames.
- Generation is event-driven; the generated content does not depend on Tick.
- Torch art belongs to the host project, so each game controls the number,
  light radius, draw distance and fade range it can afford.
- The optional fill light is shadowless and intended only to prevent unreadable
  black areas; it does not require Lumen.

## Current scope

Dungeon Blueprint Forge uses straight horizontal corridors. Vertical travel is
handled inside `DBF Stairwell Room`; curved/L-shaped corridors and room loops
are outside the current scope. The 0.10.1 checkpoint compiles in Unreal 5.4.
Adaptive Floors and Packed Rooms are documented as development work on the private plugin branch.
The laboratory completed a visual sweep from 5 to 70 Rooms on 2026-09-19 in 5.4;
NavMesh, Lumen, networking and low-end hardware acceptance remain part of each
project's QA.

## Documentation repository only

This public repository intentionally contains **documentation and usage images
only**. It does not contain the private plugin source, `.uplugin` file, Unreal
Assets, binaries or a distributable package.

```text
docs/guides/       Step-by-step usage guides.
docs/reference/    Data Asset, Actor and Component reference.
docs/development/  Release preparation notes.
docs/images/       Screenshots used by the guides.
```

Browse the complete [documentation index](docs/README.md) or visit
[AikoGx Studios on itch.io](https://aikogxstudios.itch.io/).

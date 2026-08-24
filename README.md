# Dungeon Blueprint Forge

> A Blueprint-friendly modular dungeon generator for Unreal Engine 5.4.

![Unreal Engine 5.4](https://img.shields.io/badge/Unreal%20Engine-5.4-0E1128?logo=unrealengine&logoColor=white)
![Documentation](https://img.shields.io/badge/Repository-Documentation%20Only-5B3CC4)
![Status](https://img.shields.io/badge/Status-Active%20Development-F59E0B)

**Dungeon Blueprint Forge** is a reusable Unreal Engine plugin for building
modular, seed-based dungeons from Blueprints. It handles the technical layout:
selecting rooms, placing them safely, connecting doors with corridors and
building modular room geometry. Your game keeps control of its own art,
characters, combat, UI, inventory and gameplay rules.

This repository is the public documentation portal for the plugin. It is made
for designers and Blueprint users who want to understand the workflow before
using the private plugin build.

## What problem does it solve?

Building a different dungeon layout by hand for every play session is slow and
hard to maintain. Dungeon Blueprint Forge gives you a structured way to create
reusable room Blueprints and let a deterministic seed assemble them into a
repeatable dungeon.

The same seed always produces the same valid result. That makes testing,
debugging, sharing layouts and reproducing a player report much easier.

## What the plugin does

- Generates deterministic dungeon layouts from a `Seed`.
- Chooses `Start`, `Normal`, `Hub`, `Reward`, `Key` and `Boss` rooms through
  Data Assets and selection weights.
- Supports handcrafted rooms and modular procedural rooms in `Rectangle`, `L`
  and `T` shapes.
- Connects compatible doors with straight horizontal corridors.
- Validates room overlap and protects rooms from corridor invasions and
  corridor crossings.
- Builds floor, walls, ceilings and repeated decorative geometry with HISM for
  efficient rendering.
- Generates optional wall decorations, host-project torch Actors and one
  shadowless room fill light.
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
| Connect the Generator to your map using Blueprints | [Blueprint implementation, step by step](docs/guides/02-blueprint-implementation.md) |
| Create a handcrafted or modular room | [Author rooms](docs/guides/03-authoring-rooms.md) |
| Configure room size, decorations, banners, torches and fill light | [Procedural room settings, explained](docs/guides/04-procedural-room-settings.md) |
| Light corridors and add multi-material door frames | [Corridor lighting and door frames](docs/guides/05-corridor-lighting-and-door-frames.md) |
| Understand every Data Asset | [Data Asset reference](docs/reference/data-assets.md) |
| Understand Generator, Room and Component options | [Actor and Component reference](docs/reference/actors-and-components.md) |

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
- Generation is event-driven; the generated content does not depend on Tick.
- Torch art belongs to the host project, so each game controls the number,
  light radius, draw distance and fade range it can afford.
- The optional fill light is shadowless and intended only to prevent unreadable
  black areas; it does not require Lumen.

## Current scope

Dungeon Blueprint Forge currently focuses on horizontal modular dungeons in
Unreal Engine 5.4. Curved/L-shaped corridors, vertical generation, room loops
and finished multiplayer replication are outside the current scope.

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

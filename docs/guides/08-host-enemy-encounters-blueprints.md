# Encuentros de enemigos en Blueprints del proyecto host

Esta guía construye encuentros de enemigos **fuera del plugin**. Dungeon
Blueprint Forge solo clasifica cada `Room Definition` mediante `Gameplay Zone` y
devuelve al terminar la generación la zona, seed y `World Transform` de cada
sala. El proyecto host decide qué enemigo existe y cómo funciona.

El flujo se probó en el laboratorio `DungeonLab54` con un Pawn de esferas
simple. Es una base de Blueprint para comprender el flujo, no un sistema de IA,
animaciones, loot ni combate final.

## 1. Responsabilidad de cada Actor

```text
DungeonBlueprintForgeGenerator termina
              ↓ On Generation Finished
BP_Lab_DungeonMaster (uno por mazmorra)
              ↓ registra controladores Combat
BP_Lab_RoomEnemyGenerator (uno por sala Combat)
              ↓ Start Encounter cuando el jugador está cerca
Pawns enemigos del proyecto host
              ↓ OnDestroyed
Room Generator avisa que su sala se limpió
              ↓ Event Dispatcher
Dungeon Master cuenta salas Combat limpias
```

Hay dos contadores distintos:

| Dónde | Qué cuenta | Cuándo se completa |
|---|---|---|
| `BP_Lab_RoomEnemyGenerator` | Enemigos vivos de **una** sala | `Alive Count <= 0`: llama `On Encounter Cleared`. |
| `BP_Lab_DungeonMaster` | Salas Combat ya completadas | Llega al número de controladores registrados: llama `On All Encounters Cleared`. |

El mensaje global solo debe aparecer al limpiar todas las salas `Combat`. Si
solo existe una sala Combat, ambos finales coinciden correctamente.

## 2. Preparar las Room Definitions

1. Abre cada `Dungeon Blueprint Forge Room Definition`.
2. En **Gameplay Zone**, selecciona `Combat` para salas con encuentro.
3. Usa `Safe` si no habrá enemigos automáticos. Reserva `Special` y
   `MiniBoss` para reglas del proyecto host que crearás más adelante.
4. Trabaja desde `On Generation Finished`: cada elemento de `Result.Rooms`
   contiene `Gameplay Zone`, `Room Seed`, `World Transform` y `Gameplay Anchor World Transform`.

Cada Blueprint de Room tiene una flecha heredada `GameplayAnchor`. Muévela dentro
de la Room donde quieras situar el controlador del encuentro. El Master puede
usar `Gameplay Anchor World Transform` para crear `BP_Lab_RoomEnemyGenerator`
en esa posición. Si no mueves la flecha, coincide con el origen anterior de la
Room. La flecha solo define el origen del controlador: sus `SpawnPoint_01/02/03`
siguen determinando dónde aparece cada enemigo.

No guardes clases de Pawn, salud, IA, drops o UI en el Data Asset del plugin.

## 3. Crear BP_Lab_RoomEnemyGenerator

Crea un Blueprint de tipo **Actor** en el proyecto host. Añade Scene Components
llamados, por ejemplo, `Spawn Point 01`, `Spawn Point 02` y `Spawn Point 03`.
Muévelos a posiciones navegables, lejos de puertas y paredes.
En el laboratorio existente se llaman `SpawnPoint_01`, `SpawnPoint_02` y
`SpawnPoint_03` y pertenecen a `BP_Lab_RoomEnemyGenerator`; no aparecen en
los componentes de la Packed Room.
Sitúa cada punto por encima de la superficie real del suelo: `SpawnActor` usa
exactamente su `World Transform`. Para el `DungeonLab54TestEnemyPawn` de prueba,
cuyo componente raíz es una esfera de radio `55 cm`, deja el centro del punto
unos `55 cm` por encima del suelo (más `2–5 cm` de holgura). Para otra clase,
usa la semialtura de su colisión y comprueba visualmente el pivote. En una
Packed Room, mide desde el suelo de su geometría, no desde el origen de la Room.

### Variables

| Variable | Tipo | Uso |
|---|---|---|
| `Enemy Class` | Class Reference de tu Pawn | Clase creada en esta sala. |
| `Spawn Count` | Integer | Máximo solicitado para el encuentro. |
| `Spawn Points` | Array de Scene Component Reference | Puntos disponibles. |
| `Spawned Enemies` | Array de Actor/Pawn Reference | Pawns creados por esta sala. |
| `Alive Count` | Integer | Número local de enemigos vivos. |
| `Encounter Cleared` | Boolean | Evita completar la sala dos veces. |
| `Encounter Started` | Boolean | Evita iniciar el encuentro dos veces. |

Crea además el Event Dispatcher sin parámetros `On Encounter Cleared`.

### Custom Event Start Encounter

Debe ejecutarse en servidor:

```text
Start Encounter
  → Switch Has Authority (Authority)
  → Clear Spawned Enemies
  → Clear Spawn Points
  → Add Spawn Point 01 / 02 / 03 a Spawn Points
  → Set Alive Count = 0
  → Set Encounter Cleared = false
  → For Loop
```

Conecta el `For Loop` así:

```text
First Index = 0
Last Index  = Min(Spawn Count, Length(Spawn Points)) - 1
```

Dentro de `Loop Body`:

```text
Spawn Points → Get (Index del For Loop) → Get World Transform
    → SpawnActor from Class (Enemy Class)
    → Add Return Value a Spawned Enemies
    → Set Alive Count = Length(Spawned Enemies)
    → Bind Event to OnDestroyed (Target: Return Value)
       Event: HandleEnemyDestroyed
```

`Min` evita pedir un Spawn Point inexistente. Para depurar, imprime
`Length(Spawned Enemies)`, no `Length(Spawn Points)`: los segundos son puntos
configurados, no Pawns que realmente llegaron a crearse.

### Custom Event HandleEnemyDestroyed

Enlaza `OnDestroyed` del Pawn al Custom Event `HandleEnemyDestroyed`:

```text
HandleEnemyDestroyed
  → Alive Count - 1
  → Set Alive Count
  → Alive Count <= 0
  → Branch
      False → Print "Enemy defeated - enemies remaining"
      True  → Set Encounter Cleared = true
            → Call On Encounter Cleared
```

Como defensa futura, en la ruta `True` puedes comprobar también
`Encounter Cleared == false` antes de enviar el Dispatcher.

## 4. Registrar los encuentros en BP_Lab_DungeonMaster

El Master del laboratorio hereda de `DungeonBlueprintForgeGenerator`. Añade:

| Variable/Dispatcher | Tipo | Uso |
|---|---|---|
| `Active Room Enemy Generators` | Array de `BP_Lab_RoomEnemyGenerator` | Controladores de salas Combat de esta mazmorra. |
| `Cleared Encounter Count` | Integer | Salas Combat completadas. |
| `Activation Distance` | Float | Radio de inicio en cm; el laboratorio usa `2500`. |
| `On All Encounters Cleared` | Event Dispatcher | Aviso global, sin depender de una puerta. |
| `On Dungeon Ready` | Event Dispatcher | Señal separada: generación y registro terminaron. |

1. Vincula `On Generation Finished` al Custom Event `Handle Generation Finished`.
2. Al comenzar, ejecuta `Clear Active Room Enemy Generators` y `Set Cleared
   Encounter Count = 0`.
3. Usa `For Each Loop` sobre `Result.Rooms`.
4. Conecta cada elemento a `Break DungeonBlueprintForgeGeneratedRoomInfo` y
   después a `Switch on EDungeonBlueprintForgeRoomGameplayZone`.
5. Solo en la salida `Combat`, usa `SpawnActor BP_Lab_RoomEnemyGenerator` con
   `Gameplay Anchor World Transform` de la sala. Sustituye únicamente el cable
   que antes venía de `World Transform` al pin `Spawn Transform`.
6. Desde `Return Value`: haz `Add` al array activo, `Set Spawn Count`, `Set
   Enemy Class` y **Bind Event to On Encounter Cleared** hacia el Custom Event
   del Master `Handle Encounter Cleared`.

No conectes aún `Start Encounter` a este registro: primero se vincula el
Dispatcher y el inicio se realizará por proximidad. Tras `For Each Completed`,
llama `On Dungeon Ready` si otro sistema necesita saber que ya existe el
registro de salas.

### Evento global Handle Encounter Cleared

```text
Handle Encounter Cleared
  → Cleared Encounter Count + 1
  → Set Cleared Encounter Count
  → Cleared Encounter Count >= Length(Active Room Enemy Generators)
  → Branch
      False → Print "Room encounter cleared - rooms remaining"
      True  → Call On All Encounters Cleared
            → Print "ALL ENCOUNTERS CLEARED"
```

`On All Encounters Cleared` se deja genérico. Podrá servir a objetivos, UI,
recompensas o puertas, pero no se debe convertir ahora en una puerta obligatoria:
el jugador tendrá varios objetivos posibles.

## 5. Inicio por proximidad cada 0,50 segundos

El laboratorio no crea Pawns en salas lejanas y no usa `Event Tick`.

En `Begin Play` del Master añade `Set Timer by Event`:

```text
Time = 0.50
Looping = true
Event = Check Room Activation
```

En el Custom Event `Check Room Activation`:

```text
For Each Loop (Active Room Enemy Generators)
  Array Element → Get Distance To
      Other Actor = Get Player Character (Player Index 0)
  Return Value <= Activation Distance
  → Branch
      True → Get Encounter Started (Target: Array Element)
           → NOT
           → Branch
               True → Set Encounter Started = true (Target: Array Element)
                    → Start Encounter (Target: Array Element)
```

Con `2500` Unreal Units, un timer único revisa dos veces por segundo si el
jugador está a unos 25 m. Solo inspecciona referencias ligeras; Pawns, IA y
replicación no existen hasta que empieza un encuentro.

Esto es **spawn diferido**, no congelación ni borrado al alejarse. No destruyas
un Pawn para ahorrar recursos: `Destroy Actor` dispara `OnDestroyed`, reduce
`Alive Count` y puede limpiar la sala por error.

## 6. Adaptación futura al enemigo real

No modifiques el Pawn de prueba para imitar el enemigo final. Cuando se entre al
proyecto principal, inspecciona la variable que ya desactiva todo el
comportamiento del enemigo y crea un contrato claro, por ejemplo
`Suspend Encounter Enemy` / `Resume Encounter Enemy` mediante Blueprint
Interface o componente compartido.

La suspensión debe conservar vida, estado y pertenencia a la sala. En especial,
`Set Actor Tick Enabled(false)` no basta si el Pawn usa Timers, Movement
Component u otra fuente independiente de Tick. No uses `Destroy Actor` como
suspensión temporal.

Estados recomendados:

```text
Dormant   → controlador registrado, sin Pawns
Active    → encuentro iniciado y Pawns vivos
Suspended → pausa segura futura
Cleared   → no vuelve a generar automáticamente
```

## 7. Prueba manual mínima

1. Configura dos Spawn Points y `Spawn Count = 2`.
2. Genera una mazmorra con al menos una Room Definition `Combat`.
3. Acércate: los dos enemigos aparecen solo dentro de `Activation Distance`.
4. Mata uno: debe salir el mensaje de enemigos restantes, no el global.
5. Mata el último: el Room Generator dispara `On Encounter Cleared`.
6. Si hay más salas Combat, el Master informa de salas restantes.
7. Solo al limpiar todas debe ocurrir `On All Encounters Cleared`.

Para multijugador, repite la prueba como Listen Server con cliente: el servidor
debe decidir el inicio y crear los Pawns. Esa prueba de red es independiente de
la prueba local de este laboratorio.

## 8. Errores frecuentes

| Síntoma | Revisa |
|---|---|
| Se completa al matar el primero | `Alive Count` debe inicializarse desde `Length(Spawned Enemies)` y decrementar solo con Pawns de esa sala. |
| Indica tres creados cuando aparecen dos | El debug está leyendo `Length(Spawn Points)`; usa `Length(Spawned Enemies)`. |
| No llega Handle Encounter Cleared | El Dispatcher debe enlazarse antes de `Start Encounter` y el Pawn debe destruirse de verdad. |
| Sale ALL ENCOUNTERS CLEARED tras una sala | Comprueba `Length(Active Room Enemy Generators)`. Si hay una sola sala Combat, es correcto. |
| Se crean enemigos al iniciar la mazmorra | Desconecta el `Start Encounter` inmediato y conserva el inicio en `Check Room Activation`. |
| El Pawn sigue moviéndose al desactivar Tick | Puede usar Timers; implementa pausa real en el enemigo del proyecto principal. |
| Los enemigos aparecen dentro del suelo | Sube los `Spawn Point` hasta que el centro de la colisión quede al menos una semialtura por encima del suelo real; comprueba también el pivote del Pawn. |

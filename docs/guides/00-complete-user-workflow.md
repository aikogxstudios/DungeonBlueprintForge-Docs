# Guía de uso paso a paso — Unreal Engine 5.8

Dungeon Blueprint Forge genera el mapa a partir de tus salas y una seed.
Tu proyecto aporta arte, personajes, enemigos, combate, UI y reglas de juego.
La versión de trabajo actual es **UE 5.8.3**. El nombre interno del laboratorio
`DungeonLab54` se conserva por compatibilidad; no significa que uses UE 5.4.

## 1. Entender dónde se configura cada cosa

| Quiero cambiar… | Abro… |
|---|---|
| Cuántas normales se generan y qué salas pueden aparecer | `Generation Config` |
| Si una sala es Start, Normal, Key, Boss, Hub o Reward | Su `Room Definition` |
| Tamaño, forma, paredes, pilares, decoración o luces de una procedural | Class Defaults de su Blueprint `Modular Room` |
| Arte prehecho, bounds o actor que cierra puertas libres | Class Defaults de su Blueprint `Packed Room` |
| Si una puerta ya trae marco | El componente `Exit` → `Already Has Door Frame` |
| Mallas y luces de pasillos | `Corridor Style` |
| Aspecto de los marcos generados | `Door Frame Style` |
| Cofres de una sala | `Chest Spawn Style`, asignado a su Room Definition |
| Seed y resultado del mapa | El Actor `Generator` del nivel |

El [catálogo de las 225 opciones](../reference/editor-options.md) explica los
campos con sus nombres exactos de Unreal. Pasa el ratón por una opción para
leer la misma ayuda en español. Un campo gris depende de otro interruptor.
Todas las longitudes usan **centímetros**: 100 cm = 1 m.

## 2. Crear una sala sencilla

Para empezar, crea un Blueprint hijo de `DungeonBlueprintForgeModularRoom`.

1. Abre Class Defaults → **01 Room Layout**. Elige `Room Shape = Rectangle`
   y un tamaño compatible con tus tiles; por ejemplo, 800 × 800 × 300 cm.
2. En **02 Surface Modules**, asigna las mallas de suelo y pared. Asigna techo
   si activas `Generate Ceiling`. Conserva los materiales del asset dejando
   vacíos sus overrides.
3. En **04 Connections**, activa `Use Automatic Connections` y elige las
   direcciones. `Opening Size` es anchura y altura; `Opening Center Height`
   es la altura de su centro sobre el suelo.
4. Compila el Blueprint. Coloca una instancia, usa `Preview Seed = 1` y pulsa
   `Rebuild Preview`. Comprueba suelo, puertas y colisión.
5. Añade después pilares, decoración y luces con [la guía procedural](04-procedural-room-settings.md).

Para una sala artística usa `DungeonBlueprintForgePackedRoom` y
[la guía Packed](12-prebuilt-packed-level-actor-rooms.md). El cierre de una
salida libre conserva el tamaño de tu actor y solo centra sus mallas visibles.
El plugin no añade una pared ni un fondo a ese actor.

## 3. Crear las definiciones

En Content Browser → **Add → Miscellaneous → Data Asset**, crea una
`Dungeon Blueprint Forge Room Definition` por candidata. Asigna su Blueprint
en `Room Class`, su `Category` y un `Selection Weight` positivo.

| Papel | Uso | ¿Necesario? |
|---|---|---|
| Start | Inicio; una sala | Sí |
| Normal | Salas que consumen el presupuesto de normales | Sí, si solicitas normales |
| Key | Objetivo o llave; una sala | Sí |
| Boss | Final; una sala | Sí |
| Hub | Bifurcación; necesita varias salidas | Opcional; hay fallback a normales compatibles |
| Reward | Rama terminal de recompensa | Opcional |

`Gameplay Zone` informa al juego host del uso de una sala; no crea enemigos.
Los papeles históricos o futuros sin generación activa no se ofrecen como
nuevas categorías. Mantén las Definitions separadas aunque compartan el
mismo Blueprint de geometría.

## 4. Crear el Generation Config

1. Crea un `Dungeon Blueprint Forge Generation Config` y rellena las listas
   Start, Normal, Key y Boss con sus Definitions.
2. En **02 Layout**, empieza con `Default Normal Room Count = 8` o 10.
   Este presupuesto excluye Start, Key, Boss y Hubs; el límite es 69 normales
   y 75 salas totales.
3. Elige `Connection Mode`: `Corridor` para pasillos, `Direct Contact` para
   puertas contiguas o `Random` para una mezcla reproducible.
4. Si usas pasillos, crea un `Corridor Style`, asigna suelo y las paredes
   que necesites, activa `Generate Straight Corridors` y asígnalo. Apagar la
   presentación del pasillo puede dejar una separación sin suelo.
5. En **05 Door Frames**, activa `Generate Door Frames` y asigna el estilo
   solo si quieres marcos generados.

Si el arte de una salida ya incluye marco, activa `Already Has Door Frame`
en ese Exit. En contacto directo se omite también el marco de la puerta vecina
que comparte ese punto. Con pasillo, el otro extremo conserva su marco.

## 5. Generar y comparar

1. Coloca un solo `DungeonBlueprintForgeGenerator` en el mapa.
2. Asigna el Config a `Configuration`.
3. Introduce `Preview Seed = 1` y pulsa `Generate Preview`.
4. Si falla, lee **Last Result → Message**. Conserva `Resolved Seed`.
5. Cambia una opción y repite la seed para comprobar qué ha cambiado.

La misma seed reproduce el resultado mientras conserves assets, reglas,
pesos y número solicitado. Una seed no garantiza que toda combinación de
rooms quepa: los bounds y salidas siguen determinando las posiciones válidas.

## 6. Integrarlo en Blueprint

Enlaza **On Generation Finished antes de generar**. Desde BeginPlay, valida
el Generator y llama a `Generate Dungeon From Seed` o `Generate Random Dungeon`.
En el evento, comprueba `Success`. Solo entonces sitúa jugador, enemigos o
loot que dependan de las salas. Consulta [el flujo Blueprint](02-blueprint-implementation.md).

Para presentar progresivamente, usa `Generate Dungeon From Seed Staged` o
`Generate Dungeon Staged`; empieza con 6 ms y 4 elementos por frame en
**06 Staged Presentation**. La planificación sigue siendo síncrona y un
elemento pesado puede superar el presupuesto; consulta [rendimiento](11-staged-generation-and-performance.md).

## 7. Escaleras y plantas adaptativas

Usa el Blueprint especializado `DBF Stairwell Room`: su malla completa
determina la subida y `Stair Repeat Count` repite copias sin estirarlas.
Configura descansillos y comprueba las dos puertas con [la guía Stairwell](09-procedural-stairwell.md).

En **03 Adaptive Floors**, selecciona `Adaptive Floors`, asigna
`Stairwell Room Definitions` válidas y define la huella XY. Por ejemplo,
15000 × 15000 cm significa 150 × 150 m. La huella es un límite horizontal
centrado en el Generator; no fija cuántas plantas aparecerán.

`Automatic` usa expansión libre hasta el umbral de normales y adaptativa
por encima. Con 40, 40 o menos usa expansión libre; 41 o más, adaptativa.
El tamaño fijo y el rango aleatorio se habilitan según `Randomize Footprint Size`.

## 8. Comprobar antes de ampliar

Recorre una mazmorra pequeña, revisa puertas, colisión y alturas, y conserva
su seed como comparación. Después añade variantes L/T, decoración, luz,
cofres y escaleras. Las pruebas de NavMesh, multijugador, rendimiento y
empaquetado tienen su propio alcance: consulta [el estado de UE 5.8](../development/ue58-validation-2026-10-09.md).

Si aparece un problema, usa [Diagnóstico](10-troubleshooting.md).

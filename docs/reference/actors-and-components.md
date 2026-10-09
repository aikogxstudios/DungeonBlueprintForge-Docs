# Referencia de Actors y componentes

## DungeonBlueprintForgeGenerator

Es el Actor que colocas **una vez** en el mapa. Es dueño de las salas y pasillos
que genera; `Clear Dungeon` borra solo lo que pertenece a ese Generator.

| Propiedad o nodo | Uso |
|---|---|
| `Configuration` | Generation Config activo. Es lo único obligatorio en Details. |
| `Preview Seed` + `Generate Preview` | Prueba desde editor sin crear UI. |
| `Generate Random Dungeon` | Usa una seed nueva y el número del Config. |
| `Generate Dungeon From Seed` | Repite exactamente una seed conocida. |
| `Generate Dungeon` | Recibe Request con seed, aleatoria y cantidad de normales. |
| `Regenerate Last Dungeon` | Repite el último resultado correcto. |
| `On Generation Finished` | Evento que debes enlazar antes de generar. |
| `Last Result` | Resultado, seed resuelta, mensaje y razón de fallo. |
| `Spawned Rooms/Corridors` | Solo lectura: Actors creados por esta generación. |
| `Get Generated Marker Transforms` | Da posiciones semánticas; úsalo para player, loot o enemigos. |

## DungeonBlueprintForgeRoomBase

Clase base para una sala construida a mano. `RoomRoot` es la raíz y `Allowed
Rotations` limita los giros de 0/90/180/270 que puede probar el generador.

- `Get Room Descriptor`: resumen de Bounds, Connections y Markers que usa el
  generador.
- `Validate Room And Log`: botón de autoría. Corrige todos los errores antes
  de añadir la sala a un Data Asset.
- `Prepare Room`: evento llamado con la seed de la sala. Solo sobrescríbelo si
  tu Blueprint necesita variación determinista.
- `Finalize Room Connections`: recibe las puertas usadas; las salas modulares
  cierran las demás. `Used Connection Ids` es solo lectura tras generar.
- `Generate Chest Spawns`: evento posterior a la topología final. Las rooms
  modulares prueban puntos centrados o ligeramente desplazados junto a paredes
  interiores y descartan puertas, pilares, decoración y otros cofres. Una room
  prehecha puede sobrescribirlo con posiciones artísticas propias.

## DungeonBlueprintForgePackedRoom

Contenedor de un `Packed Level Actor` del proyecto host. `Number Of Exits`
activa entre una y cuatro flechas; `Bounds Mode = Manual` usa cajas editadas
por el autor y `Automatic` aproxima la geometría con hasta 16 cajas. En
`Unused Exits`, la clase Blueprint de puerta cierra cada salida no conectada;
Height, Forward y Right Offset corrigen su posición. El actor conserva su escala original y solo se centra por sus Static Mesh visibles.
Consulta la [guía Packed](../guides/12-prebuilt-packed-level-actor-rooms.md).

## DungeonBlueprintForgeModularRoom

Sala construida con HISM. Crea un Blueprint hijo y ajusta sus valores en Class
Defaults; todos los campos de resultado y Components son informativos, no se
editan manualmente.

| Grupo | Opciones | Decisión recomendada |
|---|---|---|
| Forma | `Room Size`, `Room Shape`, `L Arm Size`, `T Arm Size` | Define el volumen interior. Rectangle es el inicio más sencillo; L/T requieren Bounds automáticos múltiples. |
| Variación | `Randomize Room Size`, mínimos/máximos, `Auto Detect Grid Step`, `Manual Grid Step` | Cambia tamaño por seed. Mantén un paso igual a tu módulo de pared/suelo. |
| Superficies | `Floor Module`, `Wall Module`, `Ceiling Module` | Cada módulo aporta mesh, material, rotación, offset y escala. No alteres el mesh original para corregir pivotes. |
| Opciones | `Generate Ceiling`, `Ceiling Vertical Offset` | Techo y ajuste vertical. |
| Pilares | `Generate Structural Pillars`, tamaño, offset, mesh/material, `Generate Rectangle Pillars`, layout y spacing | Los pilares se colocan dentro de la sala y decoración/antorchas los respetan. Usa Corners Only primero. |
| Colisión | `Enable Collision`, `Enable Physics Collision`, `Affect Navigation` | Query Only y navegación apagada es la opción ligera mientras diseñas. |
| Conexiones | `Snap Connections To Walls`, `Close Unused Connections` | Déjalas activadas. Mantienen puertas en pared y sellan las no usadas. |
| Automáticas | `Use Automatic Connections`, North/East/South/West, type, opening y height | Genera conectores cardinales. Para control artístico usa conexiones manuales. |
| Decoración | Enable, profile, máximo, clearance de pilares, área de combate, intentos y Rules | Se crea solo tras aceptar la sala. Cada Rule contiene mesh, superficie, zona, capa, cantidad, separación, escala y rotación. |
| Antorchas | Enable, Actor Class, máximo, spacing, altura, door clearance, wall inset, pillar clearance | Asigna un Actor de tu proyecto host. Las antorchas se reparten por paredes distintas y no crean sombras. |
| Rendimiento de antorcha | radius, max draw distance, fade range | Limita cuánta geometría afecta la luz. Menor radio es más barato. |
| Luz de relleno | Enable, intensity, color, height below ceiling, local offset | Una Point Light sin sombras en el centro útil. Para **bajarla**, aumenta `Height Below Ceiling`; X/Y solo la desplazan lateralmente. |
| Rendimiento de relleno | attenuation radius, draw distance, fade range | Mantén el radio dentro de la sala y el draw distance cerca del tamaño real. |
| Cofres | Se configuran en el `Chest Spawn Style` de la Room Definition | La room prueba puntos junto a paredes interiores, elimina decoración que invada el cofre y lo orienta hacia el interior. |
| Preview | `Preview Seed`, `Rebuild Preview` | Prueba una habitación sin generar toda la Dungeon. |

Para una explicación práctica de cada grupo y valores iniciales para banners,
antorchas y luz de relleno, lee [Ajustes de salas procedurales,
explicados](../guides/04-procedural-room-settings.md).

## DungeonBlueprintForgeStraightCorridor

Actor creado por el Generator, no se coloca normalmente a mano. `Build Corridor`
recibe el Style, dos conexiones y apertura. Usa HISM para suelo, dos paredes y
techo; sus componentes son de solo lectura.

## Componentes que añades a las salas

| Componente | Campos | Uso correcto |
|---|---|---|
| `DungeonBlueprintForgeBoundsComponent` | `Bounds Id`, Box Extent y Transform | Volumen sólido real. Box Extent es la mitad de tamaño. Usa varias cajas para formas no rectangulares. |
| `DungeonBlueprintForgeConnectionComponent` | `Enabled`, `Connection Id`, `Connection Type`, `Opening Size`, `Already Has Door Frame` | Una puerta. ID único; tipo igual al que debe conectar; +X hacia fuera; Opening Size = ancho/alto libre. |
| `DungeonBlueprintForgeMarkerComponent` | `Marker Type` | Punto de gameplay que el plugin devuelve como Transform, sin crear dependencia con tus Actors. |

El [catálogo completo](editor-options.md) explica las 225 opciones editables.
El panel del editor presenta un grupo por salida; las mallas y volúmenes internos
se inspeccionan en Components del editor completo.

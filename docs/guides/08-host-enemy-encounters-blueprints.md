# Encuentros de enemigos en Blueprints del proyecto host

El plugin no conoce tus Pawns, IA, vida ni botín. Clasifica cada `Room
Definition` con `Gameplay Zone` y entrega esa zona, la seed y el transform al
terminar la generación. Tu proyecto crea los encuentros.

## Flujo recomendado

```text
On Generation Finished
  -> leer Result.Rooms
  -> detectar Gameplay Zone = Combat
  -> crear un BP_RoomEncounterGenerator por sala Combat
  -> iniciar el encuentro al acercarse el jugador
  -> contar enemigos vivos y avisar cuando la sala queda limpia
```

## Blueprint de controlador de sala

Crea `BP_RoomEncounterGenerator` en tu proyecto y añade Scene Components como
puntos de spawn. Variables mínimas: `Enemy Class`, `Spawn Count`, `Spawn
Points`, `Spawned Enemies`, `Alive Count`, `Encounter Started` y `Encounter
Cleared`.

En `Start Encounter`, ejecuta `Switch Has Authority`, selecciona hasta
`Min(Spawn Count, Length(Spawn Points))` posiciones, usa `SpawnActor from
Class`, guarda cada Pawn y enlaza su `OnDestroyed`. Al destruirse uno, reduce
`Alive Count`; cuando llegue a cero, emite un dispatcher `On Encounter Cleared`.

Un manager de mazmorra registra controladores tras `On Generation Finished`.
Puede activar cada uno con un Timer de 0.5 s y `Get Distance To` el jugador.
No uses `Event Tick` ni destruyas enemigos para una pausa temporal: destruirlos
puede contar erróneamente la sala como limpiada. En multijugador, creación e
interacción de Pawns deben ejecutarse en servidor.

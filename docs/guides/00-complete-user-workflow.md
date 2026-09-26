# Recorrido completo de usuario

Usa esta guía si empiezas desde cero o vuelves al plugin después de un tiempo.
El plugin genera estructura modular; tu proyecto conserva personajes, IA,
combate, UI, loot, arte y reglas de juego.

## Ruta de aprendizaje

1. Instala el plugin y crea una mazmorra pequeña con [la primera guía](01-installation-and-first-dungeon.md).
2. Aprende el flujo `Generator -> On Generation Finished` en [Blueprint](02-blueprint-implementation.md).
3. Crea una Room rectangular, luego añade L, T, decoración y luces.
4. Añade pasillos, marcos, cofres y antorchas solo cuando la geometría sea estable.
5. Introduce Stairwell con una seed fija antes de generar mapas grandes.
6. Para geometría creada como Packed Level Actor, usa la
   [guía de Packed Rooms](12-prebuilt-packed-level-actor-rooms.md) y valida
   bounds, salidas y puertas con una seed fija.

## Assets mínimos

Necesitas Blueprints de Room `Start`, `Normal`, `Key` y `Boss`; una `Room
Definition` por papel; un `Generation Config`; y un `Corridor Style` si usas
pasillos. Usa `Gameplay Zone` para que tu Blueprint host decida encuentros,
enemigos o recompensas.

## Generación segura

En `BeginPlay`, valida el `DungeonBlueprintForgeGenerator`, enlaza `On Generation
Finished` y solo después genera. Del resultado, comprueba `Success`, guarda
`Resolved Seed` y muestra `Message` si falla. No coloques jugador, enemigos o
loot dependiente de markers hasta que `Success` sea verdadero.

La seed permite comparar cambios: ajusta una opción, repite la misma seed y
comprueba qué varió. `Maximum Placement Attempts` prueba candidatas de una Room;
`Maximum Generation Attempts` reinicia la mazmorra si una seed no cabe.

## Escaleras y crecimiento

La Stairwell crea puerta baja y alta automáticamente: puerta baja -> descansillo
-> escalera completa repetida -> descansillo -> puerta alta. Se añade a `Normal
Rooms` con categoría `Normal`; la generación continúa desde la puerta alta.
Consulta [la guía Stairwell](09-procedural-stairwell.md).

Empieza con 8--12 normales. El checkpoint 0.10.1 permite hasta 69 normales y
75 Rooms totales. Exige pruebas propias de rendimiento, colisión, NavMesh y
arte.

Para limitar la expansión por planta, usa `Adaptive Floors` en el Generation
Config, asigna `Stairwell Room Definitions` y define una huella XY. `Automatic`
activa ese modo al superar 40 normales. La caja verde de debug permite revisar
el límite, pero la transición Stairwell, la colisión y la NavMesh deben probarse
recorriendo el resultado en Unreal.

En layouts compactos, una normal o Hub que no cabe al final puede intentarse
desde otra conexión libre. Las Rooms modulares preparan bounds y conectores
durante la búsqueda y construyen su presentación una vez al aceptar el layout.
Esto reduce el coste de los reintentos; la planificación sigue en el Game
Thread incluso al usar staged y debe medirse con las seeds y assets reales.

Durante una partida puede usarse `Generate Dungeon Staged`: enlaza primero `On
Generation Progress` y `On Generation Finished`, y deja inicialmente el
presupuesto en 6 ms con 4 elementos por frame. Los nodos `Generate Dungeon`
anteriores permanecen disponibles para compatibilidad y herramientas.

## Checklist

1. Usa una seed fija.
2. Comprueba puertas, suelo, colisión y conexiones.
3. Prueba dos seeds nuevas.
4. Comprueba NavMesh real para IA y el resultado visual para Lumen.

Si algo falla, conserva la seed y consulta [Diagnóstico](10-troubleshooting.md).

# Crear salas prehechas y procedurales

## Sala prehecha

1. Crea un Blueprint hijo de `DungeonBlueprintForgeRoomBase`.
2. Añade tus meshes y un `DungeonBlueprintForgeBoundsComponent` por cada
   volumen sólido. Una L o T necesita varias cajas, no una caja enorme que
   cubra su hueco interior.
3. Añade un `DungeonBlueprintForgeConnectionComponent` por puerta real.
   Sitúalo en el centro del hueco; la flecha local **+X mira hacia fuera**.
4. Da a cada conexión un `Connection Id` único, un `Connection Type` compatible
   y un `Opening Size` con anchura y altura libres.
5. Opcionalmente añade `DungeonBlueprintForgeMarkerComponent` (`PlayerSpawn`,
   `Chest`, etc.).
6. En Class Defaults ajusta `Allowed Rotations`. Coloca una instancia temporal
   en un nivel y pulsa **Validate Room And Log** desde Details; lee Output Log
   antes de crear el Room Definition.

Si tu geometría ya es un Packed Level Actor, crea un hijo de
`DungeonBlueprintForgePackedRoom` y sigue la [guía Packed](12-prebuilt-packed-level-actor-rooms.md).

## Sala procedural

1. Crea un Blueprint hijo de `DungeonBlueprintForgeModularRoom`.
2. Configura `Room Size`, Shape y los módulos Floor/Wall/Ceiling.
3. Usa conexiones manuales como arriba o activa `Use Automatic Connections`.
   Las automáticas se ajustan a la pared correcta tras cambiar la seed.
4. Compila y guarda el Blueprint; después genera una seed fija desde un
   `DungeonBlueprintForgeGenerator` del nivel para ver el resultado.
5. Crea una Room Definition que apunte a este Blueprint.

Las propiedades de tamaño, pilares, decoración, antorchas y luz de relleno se
explican en la [referencia de Actors](../reference/actors-and-components.md).
No edites los HISM ni `Generated Bounds` a mano: son resultado de la
reconstrucción procedural.

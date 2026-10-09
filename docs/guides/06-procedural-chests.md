# Cofres procedurales

Los cofres son Actors del proyecto que usa el plugin. Dungeon Blueprint Forge
solo decide si una room obtiene cofres, cuántos, dónde caben y su orientación.
No incluye inventario, interacción ni botín.

## Preparación

1. Crea o usa tu Blueprint Actor de cofre en el proyecto host. Su eje Forward
   (`+X`) debe mirar hacia el frente del cofre, es decir, hacia donde se abre.
2. En Content Browser crea `Miscellaneous > Data Asset >
   DungeonBlueprintForgeChestSpawnStyle`.
3. En `Chest Actor Class`, elige el Blueprint de cofre.
   Para multijugador, activa también `Replicates` en ese Blueprint y replica
   su estado de abierto/botín; el plugin solo crea el Actor desde el servidor.
4. Empieza con `Base Chest Chance = 25`, `Minimum Chests = 0` y
   `Maximum Chests = 2`.
5. Abre la `DungeonBlueprintForgeRoomDefinition` de la room y asigna ese
   Asset en `Chest Spawn Style`.
6. Genera con una seed fija para comprobar el resultado antes de cambiar
   reglas.

## Qué posición elige

El cofre se coloca siempre junto a una pared interior, en un punto centrado o
ligeramente desplazado de esa pared. Si hay varios cofres, el generador prioriza
paredes distintas para que la composición se vea natural y el centro quede libre.

No se aceptan posiciones cerca de conexiones. Si una decoración HISM ocupa el
espacio del cofre, se retira esa instancia visual: el cofre tiene prioridad.
También se rechazan pilares estructurales. `Wall Inset` y `Extra Distance From
Wall` separan el pivote y el volumen visual del cofre respecto a la pared.
La reserva usa ahora los bounds reales del Actor de cofre y del mesh de cada
decoración; no solo sus pivotes. Si el Actor real invade un pilar, puerta u
otro cofre, se destruye esa tentativa y se prueba otra pared.

## Corregir un cofre que mira al revés

El plugin calcula primero la dirección jugable. Si el mesh/Blueprint tiene su
frente en otro eje, cambia solo `Rotation Offset` del Chest Spawn Style:

- Normalmente: `0, 0, 0`.
- Si el frente queda invertido: Yaw `180`.
- Si queda vertical o lateral: corrige el eje de importación en el Blueprint,
  después usa el offset solo como ajuste fino.

`Position Offset.Z` baja o sube el cofre. No cambies la posición procedural
para solucionar un pivote mal colocado.

## Debug y revisión

Activa `Draw Chest Debug`. Durante la generación aparecerán una esfera amarilla,
una flecha cian y texto `Chest Wall` con la clase de room y su Room Definition.
Con la misma seed obtendrás el mismo diagnóstico.

Si `Base Chest Chance` está en `100` y una room no recibe cofre, consulta el
Output Log: aparecerá `Chest blocked: no valid wall point`. Eso significa que
puertas, pilares o las distancias configuradas ocupan todas las paredes válidas.
Prueba a bajar `Door Clearance` o `Extra Distance From Wall`, no la probabilidad.

Prueba Rectangle, L y T, una room con pilares, una con decoración densa y una
con puertas cercanas. Revisa que no haya cofres en puertas, dentro de pilares,
solapados con props ni mirando hacia una pared.

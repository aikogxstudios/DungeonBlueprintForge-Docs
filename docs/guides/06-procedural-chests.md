# Cofres procedurales

Los cofres son Actors del proyecto host. El plugin decide si una Room recibe
cofres, cuántos caben y dónde colocarlos; no incluye inventario, interacción ni
botín.

## Configuración

1. Crea un Blueprint Actor de cofre cuyo eje `+X` mire hacia el frente.
2. Crea `Miscellaneous > Data Asset > DungeonBlueprintForgeChestSpawnStyle`.
3. Asigna el Blueprint en `Chest Actor Class`.
4. Empieza con `Base Chest Chance = 25`, `Minimum Chests = 0` y `Maximum Chests = 2`.
5. Asigna el estilo en `Chest Spawn Style` de la `Room Definition`.
6. Genera una seed fija para comprobar pivote, espacio y orientación.

El sistema prueba paredes interiores y descarta puertas, pilares, otros cofres
y decoración que invada el volumen. Si hay varias opciones, reparte cofres por
paredes distintas y mantiene libre el centro de la Room.

| Campo | Cuándo tocarlo |
|---|---|
| `Door Clearance` | Sube el valor si un cofre queda cerca de una apertura. |
| `Chest Clearance` | Sube el margen para cofres o props grandes. |
| `Wall Inset` | Ajusta la distancia del pivote respecto a la pared. |
| `Rotation Offset` | Usa Yaw `180` si el mesh mira al revés. |
| `Draw Chest Debug` | Muestra posiciones y motivos de descarte. |

En Stairwell solo se permite un cofre en el `Lower Landing`, con espacio real
suficiente; nunca aparece en escalones ni puertas. Para red, activa `Replicates`
en el Blueprint de cofre y replica estado de abierto y botín.

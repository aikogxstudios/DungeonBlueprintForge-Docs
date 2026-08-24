# Iluminación y marcos de puerta en pasillos

Estas dos opciones se configuran en el mismo `Dungeon Blueprint Forge Corridor
Style` Data Asset que ya usas para suelo, paredes y techo. Se aplican solo a
pasillos que el Generator acepta durante una generación válida.

## Luz procedural de pasillo

Activa `Enable Corridor Fill Lights` en la categoría **Corridor Lighting**.
El plugin añade Point Lights nativas sin sombras al propio Actor del pasillo:
un pasillo corto recibe una luz centrada y uno largo recibe varias, con un límite
máximo para mantener el rendimiento controlado.

| Ajuste | Valor inicial | Uso |
|---|---:|---|
| `Intensity` | `350 lm` | Brillo de la luz. Mantén el valor bajo para que las antorchas sigan destacando. |
| `Color` | Azul/gris suave | Separa la luz de apoyo de la luz cálida de antorchas. |
| `Height From Floor` | `180 cm` | Sube o baja las luces dentro del pasillo. |
| `Light Spacing` | `900 cm` | Distancia preferida entre luces. |
| `Maximum Lights Per Corridor` | `3` | Tope de seguridad para pasillos largos. |
| `Attenuation Radius` | `750 cm` | Área iluminada. Procura que no ilumine salas lejanas. |
| `Max Draw Distance` | `2500 cm` | Distancia a la que la luz deja de renderizarse. |
| `Fade Range` | `400 cm` | Fundido antes de ese límite. |

Las luces no proyectan sombras y no usan Tick. Si el pasillo queda demasiado
oscuro, aumenta primero `Intensity`; si una luz se ve demasiado baja, aumenta
`Height From Floor`.

## Marcos de puerta automáticos

Activa `Enable Door Frames` en la categoría **Door Frames** y asigna un mesh en
`Door Frame Mesh`. El Generator crea un `DungeonBlueprintForgeDoorFrame` en
cada extremo de cada pasillo aceptado: uno en la salida de una sala y otro en
la entrada de la siguiente.

Los marcos no aparecen en conexiones descartadas, por lo que no verás un marco
aislado en una puerta que no tenga pasillo.

| Ajuste | Uso |
|---|---|
| `Door Frame Mesh` | Mesh del marco. Su pivote debería estar en el centro de la abertura. |
| `Material Overrides Per Slot` | Lista opcional, un elemento por slot de material. Una entrada vacía mantiene el material original de ese slot. |
| `Rotation Offset` | Corrige el eje del mesh. Usa Yaw `180` solo si el marco mira al revés. |
| `Position Offset` | Ajuste fino desde el centro de la abertura. X positivo mueve el marco hacia el pasillo. |
| `Scale` | Ajuste del tamaño del mesh. Procura que coincida con tus aperturas modulares. |
| `Enable Frame Collision` | Apagado por defecto. Actívalo solo si el mesh debe bloquear al jugador. |
| `Frame Affects Navigation` | Solo úsalo con colisión si el marco cambia realmente el NavMesh. |

Un marco puede tener piedra, metal, runas o cualquier otra combinación de
materiales. No hay un override único que sustituya todos los slots: deja la
lista vacía para conservar los materiales del mesh, o sobrescribe únicamente
los índices que quieras cambiar.

## Preparado para puertas futuras

El marco es un Actor individual, no una instancia HISM. Hay pocos por
mazmorra y esta decisión permite convertirlo más adelante en una puerta real
con animación, llave, bloqueo o interacción sin cambiar la colocación
procedural.

## Prueba rápida

1. Activa luces y marcos en tu Corridor Style.
2. Asigna un mesh de marco con sus materiales ya configurados.
3. Genera una seed fija.
4. Comprueba un pasillo corto y otro largo.
5. Si cambias offsets o escala, regenera la misma seed para comparar el cambio.

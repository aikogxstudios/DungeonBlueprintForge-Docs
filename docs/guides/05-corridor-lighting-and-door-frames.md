# Iluminación y marcos de puerta en pasillos

La luz se configura en el `Dungeon Blueprint Forge Corridor Style` que ya usas
para suelo, paredes y techo. Los marcos se configuran aparte, desde el
`Generation Config`, para cubrir todas las puertas finales de la mazmorra.

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

En tu `Generation Config`, activa `Generate Door Frames` y crea un Data Asset
de clase `DungeonBlueprintForgeDoorFrameStyle`. Asígnalo en `Door Frame Style`.

El Generator espera a que la topología completa esté terminada: primero cierra
las conexiones no utilizadas y, como último paso, crea un
`DungeonBlueprintForgeDoorFrame` en cada abertura utilizada. Esto incluye los
dos extremos de un pasillo y las puertas de `Direct Contact`. Si dos rooms
comparten el mismo hueco de contacto directo, se crea un único marco para evitar
dos Actors superpuestos.

No se crean marcos en conexiones descartadas o cerradas.

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

1. Activa las luces en tu `Corridor Style` si las quieres usar.
2. En el `Generation Config`, activa `Generate Door Frames`.
3. Crea y asigna un `DungeonBlueprintForgeDoorFrameStyle` con su mesh y materiales.
4. Genera una seed fija.
5. Comprueba un pasillo corto, uno largo y una conexión `Direct Contact` si tu configuración la utiliza.
6. Si cambias offsets o escala, regenera la misma seed para comparar el cambio.

## Solución de problemas: Door Frame Style no se asigna

El campo `Door Frame Style` **no acepta** el Actor
`DungeonBlueprintForgeDoorFrame`. Debe recibir un Data Asset creado con la
clase `DungeonBlueprintForgeDoorFrameStyle`.

Si has creado el Data Asset correcto pero Unreal lo rechaza o el campo vuelve a
quedar vacío, probablemente el `Generation Config` conserva una ruta antigua
del asset. No borres el asset ni lo muevas desde el Explorador de Windows.

1. Abre el `Generation Config` y usa la flecha de reset del campo `Door Frame Style`.
2. Guarda el `Generation Config`.
3. En el Content Browser, clic derecho sobre la carpeta que contiene los assets y usa `Fix Up Redirectors in Folder`.
4. Vuelve a abrir el `Generation Config` y selecciona el `DungeonBlueprintForgeDoorFrameStyle` desde su carpeta actual.
5. Guarda de nuevo y genera la misma seed para confirmar el resultado.

Para reorganizar assets en el futuro, muévelos siempre desde el Content Browser
de Unreal; así las referencias se actualizan de forma segura.

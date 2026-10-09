# Referencia de Data Assets

Los Data Assets separan reglas y contenido del código. Crea los necesarios desde el
Content Browser con `Add > Miscellaneous > Data Asset`; asigna siempre arte y
Blueprints de tu proyecto, no dentro de `Source/` del plugin.

## 1. Dungeon Blueprint Forge Generation Config

Es el Asset que se asigna en **Configuration** del `DungeonBlueprintForgeGenerator`.
Define qué salas son candidatas y cómo se construye el mapa; no guarda el
progreso de una partida.

Consulta [todas las opciones del editor](editor-options.md) para sus valores iniciales y ayudas.

### Rooms

| Campo | Qué hace | Regla práctica |
|---|---|---|
| `Start Rooms` | Definitions posibles para el inicio. | Debe haber al menos una válida. |
| `Normal Rooms` | Lista de Definitions candidatas normales. | Debe existir si `Normal Room Count` es mayor que cero. |
| `Hub Rooms` | Salas con tres o más salidas. | Si queda vacío, se usan Normal Rooms. |
| `Reward Rooms` | Salas terminales de tesoro/evento. | Opcional; vacío significa que no se generan. |
| `Key Rooms` | Definitions de la llave/objetivo. | Debe existir al menos una válida. |
| `Boss Rooms` | Definitions del final. | Debe existir al menos una válida. |

### Generation y Placement

| Campo | Significado |
|---|---|
| `Default Normal Room Count` | Cantidad usada por `Generate Random Dungeon` y `Generate Dungeon From Seed`. Start, Key y Boss no cuentan aquí. |
| `Maximum Placement Attempts` | Límite de combinaciones de sala, puerta y rotación por hueco. Más alto encuentra más opciones, pero tarda más. Empieza con 100. |
| `Randomize Connection Gap` | Hace que la seed elija la separación de cada unión. Mantiene el mismo resultado al repetir la seed. |
| `Connection Gap` | Distancia fija entre puertas cuando la opción anterior está desactivada. |
| `Minimum/Maximum Connection Gap` | Rango de separación cuando está activada. El Boss usa una distancia larga dentro de esta regla. |
| `Overlap Tolerance` | Margen pequeño que permite que límites solo se toquen. No es una herramienta para forzar salas que se solapan. Déjalo en 1 cm salvo ajuste muy justificado. |
| `Connection Mode` | `Corridor` crea pasillo, `Direct Contact` une puertas sin pasillo y `Random` decide de forma reproducible por seed. |

### Plantas adaptativas

| Campo | Significado |
|---|---|
| `Generation Expansion Mode` | `Free Expansion` conserva el crecimiento actual sin límite XY. `Adaptive Floors` mantiene las salas y pasillos dentro de una huella XY y reserva las Stairwell para continuar verticalmente. `Automatic` usa plantas adaptativas solo si `Normal Room Count` supera el umbral. |
| `Automatic Adaptive Floor Threshold` | Umbral de `Normal Room Count` para `Automatic`. Con el valor predeterminado `40`, 40 o menos usa expansión libre y 41 o más usa plantas adaptativas. |
| `Stairwell Room Definitions` | Definitions `Normal` de `DBF Stairwell Room` reservadas para el cambio de planta. Es obligatorio rellenarla en plantas adaptativas. |
| `Default Footprint Size` | Anchura y profundidad, en centímetros, de cada planta cuando no se aleatoriza. Es una zona local centrada en el Generator, no un límite de altura. |
| `Randomize Footprint Size` y `Minimum/Maximum Footprint Size` | Eligen una huella XY dentro del rango mediante la seed. El tamaño resuelto se devuelve en `Last Result`. |
| `Stairwell Direction` | `Up Only`, `Down Only` o `Either`, según qué puerta vertical de la Stairwell puede usarse para seguir el recorrido. |
| `Maximum Adaptive Stairwell Transitions` | Salvaguarda técnica de intentos verticales por generación; no define un número objetivo de plantas. |
| `Draw Adaptive Floor Footprint` | Dibuja una caja verde que marca el alcance horizontal máximo de la generación adaptativa. No modifica colisión ni placement. |
| `Adaptive Floor Debug Half Height` / `Duration` | Altura visual de la caja y segundos visibles tras generar. Solo sirven para depurar el límite XY. |
| `Maximum Generation Attempts` | Reinicios completos de layout por solicitud, limitado a 1--32 y 20 por defecto. Aumentarlo puede mejorar la tasa de éxito, pero también el tiempo síncrono de generación. |
| `Maximum Local Backtrack Steps` | Rooms normales recientes que se pueden retirar para liberar conexiones antes de reiniciar el layout. Rango 0--8, valor predeterminado 2; las normales retiradas se restauran tras colocar Hub o Key. |
| `Print Generation Retry Debug` | Muestra en pantalla reinicios completos, tiempo por intento y pasos de backtracking. Solo diagnóstico; desactivado por defecto. |
| `Staged Generation Time Budget` | Tiempo máximo aproximado de presentación por frame para los nodos Staged. Predeterminado 6 ms. |
| `Maximum Staged Items Per Frame` | Límite de Rooms, pasillos u otros elementos construidos en un frame. Predeterminado 4. |

La altura real y el número de tramos pertenecen al actor `DBF Stairwell Room`: ajusta allí
`Stair Repeat Count`, descansillos y mallas. El Config solo decide cuándo esa Definition
puede ser seleccionada para salir de una huella llena.

El debug se dibuja una sola vez tras el éxito o fallo final. `Last Result`
conserva el modo, la huella resuelta y las transiciones también en un fallo, para
que una seed problemática pueda diagnosticarse sin reconstruir su configuración.

### Corridors y Special Rooms

| Campo | Significado |
|---|---|
| `Generate Straight Corridors` | Activa la geometría visual del corredor. Requiere un Style válido. |
| `Straight Corridor Style` | Data Asset visual de suelo, paredes, techo, alineación y colisión. |
| `Generate Door Frames` | Coloca marcos cuando toda la topología está finalizada. Funciona con pasillos y `Direct Contact`. |
| `Door Frame Style` | Data Asset independiente con el mesh y reglas visuales de todos los marcos. |
| `Minimum Start To Key Graph Distance` | Mínimo de pasos lógicos desde Start antes de colocar Key. No es distancia en centímetros. |

## 2. Dungeon Blueprint Forge Room Definition

Representa una sala seleccionable. Crea una Definition por rol aunque dos roles
apunten temporalmente al mismo Blueprint.

| Campo | Significado |
|---|---|
| `Room Class` | Blueprint hijo de `DungeonBlueprintForgeRoomBase`, `DungeonBlueprintForgeModularRoom` o `DungeonBlueprintForgePackedRoom`. |
| `Additional Preload Assets` | Referencias blandas opcionales que staged generation carga antes de presentar esta variante. |
| `Category` | Rol: Start, Normal, Hub, Reward, Key o Boss. Debe coincidir con la lista donde se añade. |
| `Selection Weight` | Probabilidad relativa entre candidatas compatibles. `0` evita que se seleccione. |
| `Enabled` | Activa/desactiva sin borrar el Asset. |
| `Chest Spawn Style` | Reglas opcionales de cofres para esta variante concreta de room. Vacío significa que esa room nunca genera cofres. |
| `Gameplay Zone` | Etiqueta genérica para el juego host: `Safe`, `Combat`, `Special` o `MiniBoss`. Se copia al resultado de la sala; no crea ni configura enemigos. |

Consulta [Rooms prehechas con Packed Level Actor](../guides/12-prebuilt-packed-level-actor-rooms.md)
para el actor contenedor, cajas, flechas y validación.

### Gameplay Zone y sistemas del juego

Usa `Safe` para salas sin encuentro automático y `Combat` para las que un
sistema del proyecto host debe considerar como combate normal. `Special` y
`MiniBoss` reservan variantes con reglas propias. El plugin no conoce Pawns,
vida, IA, loot, UI ni puertas de tu juego: esos elementos se crean desde el
proyecto que utiliza el plugin después de `On Generation Finished`.

## 3. Dungeon Blueprint Forge Corridor Style

Define el aspecto reutilizable de cualquier pasillo recto. Todas las referencias
son blandas para que las mallas descargadas sigan perteneciendo al juego host.

| Grupo | Campo | Qué hace |
|---|---|---|
| Meshes | `Floor Mesh`, `Wall Mesh`, `Ceiling Mesh` | Piezas repetidas de suelo, ambas paredes y techo. Floor y Wall son necesarios si generas sus superficies; Ceiling es opcional. |
| Materials | `Floor/Wall/Ceiling Material` | Override opcional; vacío conserva el material del mesh. |
| Alignment | `Floor`, `Left Wall`, `Right Wall`, `Ceiling Alignment` | Corrige un pack con pivote/eje distinto sin modificar su Static Mesh. |
| Alignment interno | `Rotation Offset` | Gira la pieza antes de encajarla. |
| Alignment interno | `Position Offset` | Desplazamiento local final en cm. |
| Alignment interno | `Size Multiplier` | Multiplica X longitud, Y grosor/anchura y Z altura. Usa `1,1,1` por defecto. |
| Options | `Generate Ceiling/Left Wall/Right Wall` | Elige qué superficies se construyen. |
| Options | `Enable Collision` | Usa la colisión simple del Static Mesh. |
| Options | `Enable Physics Collision` | QueryAndPhysics; normalmente no hace falta para pasillos estáticos. |
| Options | `Affect Navigation` | Incluye la geometría en NavMesh. Déjalo apagado mientras iteras el diseño. |
| Corridor Lighting | `Enable Corridor Fill Lights`, color, intensidad, altura, spacing y rendimiento | Point Lights sin sombras generadas dentro del pasillo. Usa el límite máximo para controlar el coste. |

Consulta [Iluminación y marcos de puerta en pasillos](../guides/05-corridor-lighting-and-door-frames.md) para valores iniciales y pruebas.

## 4. Dungeon Blueprint Forge Door Frame Style

Define el aspecto de todos los marcos de puerta de una mazmorra. Se asigna en
`Door Frame Style` del `Generation Config`, no en el Corridor Style.

| Campo | Qué hace |
|---|---|
| `Door Frame Mesh` | Mesh del marco; su pivote debe quedar centrado en la abertura. |
| `Material Overrides Per Slot` | Sustituye solo los slots que rellenes; una entrada vacía conserva el material del mesh. |
| `Rotation Offset` | Corrige un mesh que se haya creado con otro eje. Usa Yaw `180` únicamente si mira al revés. |
| `Position Offset` | Corrección local fina desde el centro de la abertura. |
| `Scale` | Tamaño final del mesh. Empieza en `1,1,1`. |
| `Enable Frame Collision` | Hace que el marco bloquee al jugador solo si el mesh tiene colisión útil. |
| `Frame Affects Navigation` | Actívalo únicamente si la colisión del marco cambia realmente el NavMesh. |

## 5. Dungeon Blueprint Forge Chest Spawn Style

Define cofres como **Actors del proyecto host**. No incluye un mesh, inventario
ni lógica de apertura: asigna tu Blueprint de cofre en `Chest Actor Class`.
El Generator lo ejecuta después de aceptar la mazmorra y de cerrar puertas.

| Grupo | Campo | Qué hace |
|---|---|---|
| Content | `Chest Actor Class` | Blueprint Actor de cofre que el plugin crea. Debe tener su propia lógica de abrir, loot y colisión. |
| Quantity | `Enable Chest Spawning` | Desactiva cofres sin perder el preset. |
| Quantity | `Base Chest Chance` | Probabilidad base de que una room tenga cofres. |
| Quantity | `Bonus Chance Per Extra Chest` | Aumento automático por cada cofre extra permitido; hace que una room capaz de alojar más cofres tenga una probabilidad algo mayor. |
| Quantity | `Minimum/Maximum Chests` | Rango seleccionado tras superar la probabilidad. Con mínimo `0`, una tirada aprobada genera como mínimo uno; la ausencia ya la controla la probabilidad. `0 / 3` es el punto de partida recomendado. |
| Placement | Colocación junto a pared | El sistema prueba puntos centrados o levemente desplazados en paredes interiores y elige caras distintas si hay varios cofres. No usa esquinas ni centro de la room. |
| Safety | `Wall Inset` | Distancia interior respecto a la pared medida desde el pivote del Actor. |
| Safety | `Extra Distance From Wall` | Hueco visual adicional entre el volumen del cofre y la pared. Empieza en `100 cm`; súbelo si aún parece pegado. |
| Safety | `Chest Clearance` | Margen extra reservado para el cofre. Se suma al volumen real del Actor y al de cada decoración HISM que invada el espacio; el cofre tiene prioridad. |
| Safety | `Door Clearance` | Distancia mínima desde aperturas para no bloquear entradas, salidas o pasillos. |
| Visual Adjustment | `Position Offset`, `Rotation Offset`, `Scale` | Corrige pivote, altura, orientación y tamaño del Blueprint de cofre sin modificarlo. El Forward (+X) calculado apunta hacia la zona jugable. |
| Debug | `Draw Chest Debug`, `Debug Duration` | Dibuja esfera, flecha y texto temporal con posición, tipo de room y Room Definition para encontrar una regla incorrecta. |

### Valores iniciales

Para una room normal: `Base Chest Chance 25`, `Bonus 5`, `Minimum 0`,
`Maximum 2`, pesos `1/1/1`, `Wall Inset 120 cm`, `Extra Distance From Wall
100 cm`, `Chest Clearance 180 cm` y `Door Clearance 240 cm`. En una room
Reward puedes usar `65`, `10`, `1`, `3`.

## Validación rápida de los Assets

1. Todas las listas obligatorias tienen al menos una Definition `Enabled`.
2. Cada Definition apunta a una Room Class válida y su Category coincide.
3. Si Connection Mode puede crear pasillos, el Style tiene mallas válidas.
4. Repite una seed fija antes de cambiar pesos, tamaños o conexiones: es la
   manera más rápida de saber qué ajuste cambió el resultado.
5. Para cofres, asigna el Chest Spawn Style a la Room Definition; no se asigna
   en el Generation Config ni en el Actor Generator.

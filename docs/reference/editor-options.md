# Todas las opciones del editor — UE 5.8

Referencia generada desde las descripciones del plugin. Los nombres de campo se mantienen
en inglés para coincidir con Unreal; las explicaciones están en español.

Empieza por [la guía de uso](../guides/00-complete-user-workflow.md). Consulta esta página
cuando necesites saber qué hace un campo concreto. **Las medidas son centímetros** salvo
que se indique otra unidad; 100 cm = 1 m. Un campo gris depende de otra opción.

Los valores de esta tabla son los predeterminados del código, no los valores de tus assets.

## Dónde configurar cada cosa

| Objeto | Lugar en Unreal |
|---|---|
| Generation Config | Data Asset asignado al Generator |
| Room Definition | Data Asset de cada candidata y su papel |
| Modular / Stairwell / Packed Room | Class Defaults del Blueprint de la sala |
| Connection Component | Seleccionar Exit en Components, o su grupo en Connections |
| Surface Module / Decoration Rule | Desplegar módulo o elemento de Decoration Rules |
| Corridor / Door Frame / Chest Style | Data Asset del estilo correspondiente |
| Generator | Actor colocado en el mapa |

## UDungeonBlueprintForgeChestSpawnStyle


### Chest → Content

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Chest Actor Class` | `Sin asignar` | Actor Blueprint del cofre creado por el juego host. |

### Chest → Quantity

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Enable Chest Spawning` | `Activado` | Interruptor para conservar el estilo configurado sin generar cofres. |
| `Base Chest Chance` | `35.0` | Probabilidad base de que esta room contenga al menos un cofre. |
| `Bonus Chance Per Extra Chest` | `5.0` | Aumenta la probabilidad final por cada cofre adicional que permita Maximum Chests. |
| `Minimum Chests` | `0` | Mínimo de cofres si la probabilidad de la room tiene éxito. Cero usa uno: la ausencia ya la decide Chest Chance. |
| `Maximum Chests` | `3` | Máximo de cofres en esta room. Mantenerlo bajo protege ritmo, navegación y rendimiento. |

### Chest → Placement

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Wall Inset` | `120.0` | Separación del pivote respecto a la pared, en cm. |
| `Extra Distance From Wall` | `100.0` | Separación visual añadida al Wall Inset para que un cofre ancho no parezca pegado a la pared. |

### Chest → Safety

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Chest Clearance` | `180.0` | Espacio vacío alrededor de cada cofre. Las decoraciones HISM que entren aquí se retiran. |
| `Door Clearance` | `240.0` | Mantiene cofres fuera de puertas y pasillos. |

### Chest → Visual Adjustment

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Position Offset` | `0, 0, 0` | Desplazamiento visual local en centímetros para corregir el pivote. |
| `Rotation Offset` | `0°, 0°, 0°` | Corrección de orientación de la malla o actor en grados. |
| `Scale` | `1, 1, 1` | Escala explícita del actor de este estilo; 1,1,1 mantiene su escala de referencia. |

### Chest → Debug

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Draw Chest Debug` | `Desactivado` | Dibuja posición, dirección, estilo y room durante la generación para encontrar reglas incorrectas. |
| `Debug Duration` | `12.0` | Duración del debug de cofres en el mundo. |

## FDungeonBlueprintForgeCorridorMeshAlignment


### Dungeon Blueprint Forge

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Rotation Offset` | `0°, 0°, 0°` | Corrección de orientación de la malla o actor en grados. |
| `Position Offset` | `0, 0, 0` | Desplazamiento visual local en centímetros para corregir el pivote. |
| `Size Multiplier` | `1, 1, 1` | Multiplicador del tamaño del módulo antes de colocarlo; 1,1,1 mantiene su tamaño de referencia. |

## UDungeonBlueprintForgeCorridorStyle


### 01 Meshes

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Floor Mesh` | `Sin asignar` | Malla modular del suelo del pasillo. Es obligatoria para construirlo. |
| `Wall Mesh` | `Sin asignar` | Malla de pared del pasillo. Se usa si alguna pared está activada. |
| `Ceiling Mesh` | `Sin asignar` | Malla opcional del techo del pasillo; se usa si Generate Ceiling está activo. |

### 02 Materials

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Floor Material` | `Sin asignar` | Material opcional del suelo; vacío conserva los materiales de Floor Mesh. |
| `Wall Material` | `Sin asignar` | Material opcional de las paredes; vacío conserva los materiales de Wall Mesh. |
| `Ceiling Material` | `Sin asignar` | Material opcional del techo; vacío conserva los materiales de Ceiling Mesh. |

### 03 Alignment

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Floor Alignment` | `Sin asignar` | Orientación, posición y tamaño del módulo de suelo del pasillo. |
| `Left Wall Alignment` | `Sin asignar` | Orientación, posición y tamaño de la pared izquierda del pasillo. |
| `Right Wall Alignment` | `Sin asignar` | Orientación, posición y tamaño de la pared derecha del pasillo. |
| `Ceiling Alignment` | `Sin asignar` | Orientación, posición y tamaño del techo del pasillo. |

### 04 Geometry And Collision

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Generate Ceiling` | `Activado` | Construye el techo con el módulo asignado. |
| `Generate Left Wall` | `Activado` | Construye la pared izquierda del pasillo. |
| `Generate Right Wall` | `Activado` | Construye la pared derecha del pasillo. |
| `Enable Collision` | `Activado` | Activa la colisión de la geometría generada. |
| `Enable Physics Collision` | `Desactivado` | Añade colisión física además de consultas. Requiere Enable Collision. |
| `Affect Navigation` | `Desactivado` | Permite que la geometría con colisión participe en la NavMesh. |

### 05 Fill Lights

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Enable Corridor Fill Lights` | `Desactivado` | Crea luces de relleno sin sombras en el pasillo. |

### 05 Fill Lights → Channels

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Isolate Corridor Local Lights From Player` | `Activado` | Usa canal de luz 1 para el relleno del pasillo; un jugador en canal 0 no lo recibe. |

### 05 Fill Lights

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Corridor Fill Light Intensity` | `350.0` | Intensidad de las luces del pasillo en lúmenes. |
| `Corridor Fill Light Color` | `(0.32, 0.42, 0.70, 1.0)` | Color de las luces de relleno del pasillo. |
| `Corridor Fill Light Height From Floor` | `180.0` | Altura de las luces sobre el suelo del pasillo, en cm. |
| `Corridor Fill Light Spacing` | `900.0` | Separación objetivo entre luces de pasillo, en cm. |
| `Max Corridor Fill Lights` | `3` | Máximo de luces de relleno por pasillo. |

### 05 Fill Lights → Performance

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Corridor Fill Light Attenuation Radius` | `750.0` | Radio de influencia de cada luz del pasillo, en cm. |
| `Corridor Fill Light Max Draw Distance` | `2500.0` | Distancia máxima de dibujado de las luces del pasillo, en cm. |
| `Corridor Fill Light Fade Range` | `400.0` | Rango de desaparición gradual del relleno del pasillo, en cm. |

## UDungeonBlueprintForgeDoorFrameStyle


### Door Frame

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Door Frame Mesh` | `Sin asignar` | Malla del marco generado. Su pivote debe estar centrado en la abertura. |

### Door Frame → Materials

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Material Overrides Per Slot` | `Lista vacía` | Un material por slot del marco. Los elementos vacíos conservan su material original. |

### Door Frame → Placement

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Rotation Offset` | `0°, 0°, 0°` | Corrección de orientación de la malla o actor en grados. |
| `Position Offset` | `0, 0, 0` | Desplazamiento visual local en centímetros para corregir el pivote. |
| `Scale` | `1, 1, 1` | Escala explícita del actor de este estilo; 1,1,1 mantiene su escala de referencia. |

### Door Frame

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Enable Frame Collision` | `Desactivado` | Activa la colisión de la geometría generada. |
| `Frame Affects Navigation` | `Desactivado` | Permite que el marco con colisión participe en la NavMesh. |

## UDungeonBlueprintForgeGenerationConfig


### 01 Rooms → Required

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Start Rooms` | `Lista vacía` | Candidatas de inicio: se elige una Room Definition de categoría Start según su peso. |

### 01 Rooms → Normal

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Normal Rooms` | `Lista vacía` | Candidatas normales: se distribuyen entre la ruta principal y las ramas según el presupuesto de normales. |

### 01 Rooms → Optional Roles

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Hub Rooms` | `Lista vacía` | Candidatas con varias salidas para bifurcaciones. Vacío reutiliza salas normales compatibles. |
| `Reward Rooms` | `Lista vacía` | Candidatas terminales de recompensa. Vacío desactiva las salas Reward. |

### 01 Rooms → Required

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Key Rooms` | `Lista vacía` | Candidatas de objetivo o llave: se elige una Room Definition de categoría Key. |
| `Boss Rooms` | `Lista vacía` | Candidatas de final: se elige una Room Definition de categoría Boss. |

### 02 Layout

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Default Normal Room Count` | `10` | Número de salas normales solicitado por los nodos que no reciben un Request. Rango soportado: 0 a 69; no incluye Start, Key, Boss ni Hubs. |
| `Maximum Placement Attempts` | `100` | Máximo de intentos de colocar una candidata. Aumentarlo permite buscar más posiciones y tarda más. |
| `Maximum Generation Attempts` | `20` | Número de layouts completos que se intentan para la misma seed antes de declarar un fallo. No cambia la seed del jugador. |
| `Maximum Local Backtrack Steps` | `2` | Número de salas recientes que se pueden retirar para recuperar una conexión. Cero desactiva el retroceso local. |

### 07 Debug

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Print Generation Retry Debug` | `Desactivado` | Muestra mensajes de reintentos y tiempos. Úsalo solo para diagnóstico. |

### 06 Staged Presentation

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Staged Generation Time Budget` | `6.0` | Presupuesto aproximado de presentación por frame, en milisegundos. Un único elemento pesado puede superarlo; la planificación sigue siendo síncrona. |
| `Maximum Staged Items Per Frame` | `4` | Límite adicional de elementos presentados por frame. Se combina con el presupuesto en milisegundos. |
| `Preload Staged Generation Assets` | `Activado` | Precarga las referencias visuales antes de planificar la generación Staged para reducir carga síncrona. |

### 03 Adaptive Floors

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Generation Expansion Mode` | `FreeExpansion` | Free Expansion crece sin límite XY. Adaptive Floors limita cada planta y usa escaleras. Automatic elige según el número de normales. |
| `Automatic Adaptive Floor Threshold` | `40` | Solo en Automatic: hasta este número de normales usa expansión libre; por encima usa plantas adaptativas. |
| `Stairwell Room Definitions` | `Lista vacía` | Definitions Normal de DBF Stairwell Room que conectan plantas. Obligatorio cuando se resuelve Adaptive Floors. |
| `Default Footprint Size` | `(30000.0, 30000.0)` | Anchura X y profundidad Y de cada planta, en cm, centradas en el Generator. Se usa cuando Randomize Footprint Size está apagado. |
| `Randomize Footprint Size` | `Desactivado` | Elige por seed una huella XY dentro del rango mínimo y máximo. |
| `Minimum Footprint Size` | `(22000.0, 22000.0)` | Menor anchura X y profundidad Y para la huella aleatoria, en centímetros. |
| `Maximum Footprint Size` | `(36000.0, 36000.0)` | Mayor anchura X y profundidad Y para la huella aleatoria, en centímetros. |
| `Stairwell Direction` | `Either` | Permite ascender, descender o ambos sentidos al usar una Stairwell; su altura física se configura en la sala escalera. |
| `Maximum Adaptive Stairwell Transitions` | `6` | Salvaguarda del número de transiciones verticales. No es un número objetivo de plantas. Se muestra en opciones avanzadas. |

### 07 Debug → Adaptive Floors

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Draw Adaptive Floor Footprint` | `Desactivado` | Dibuja el límite horizontal de las plantas adaptativas; no crea colisión. Se muestra en opciones avanzadas. |
| `Adaptive Floor Debug Half Height` | `5000.0` | Semialtura visual de la caja de diagnóstico. No cambia la altura de las salas. Se muestra en opciones avanzadas. |
| `Adaptive Floor Debug Duration` | `60.0` | Duración de la caja de diagnóstico, en segundos. Cero persiste hasta limpiar el mundo. Se muestra en opciones avanzadas. |

### 02 Layout → Connections

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Randomize Connection Gap` | `Desactivado` | Elige por seed la separación entre salidas dentro del rango configurado; solo afecta conexiones con pasillo. |
| `Connection Gap` | `300.0` | Separación fija entre salidas, en cm, para conexiones con pasillo. |
| `Minimum Connection Gap` | `200.0` | Separación mínima entre salidas para pasillos con distancia aleatoria, en cm. |
| `Maximum Connection Gap` | `600.0` | Separación máxima entre salidas para pasillos con distancia aleatoria, en cm. |
| `Overlap Tolerance` | `1.0` | Margen pequeño para permitir que dos bounds se toquen. No permite atravesar otras salas; empieza con 1 cm. |
| `Connection Mode` | `Corridor` | Corridor separa las rooms con pasillo. Direct Contact une sus salidas. Random elige por seed. |

### 04 Corridors

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Generate Straight Corridors` | `Desactivado` | Construye la geometría de los pasillos reservados. Desactivado puede dejar separaciones sin suelo. |
| `Straight Corridor Style` | `Sin asignar` | Estilo de mallas, materiales, colisión y luz de los pasillos. |

### 05 Door Frames

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Generate Door Frames` | `Desactivado` | Añade marcos a las salidas utilizadas, salvo las marcadas Already Has Door Frame. No modifica el arte integrado de la room. |
| `Door Frame Style` | `Sin asignar` | Data Asset con la malla, materiales y colocación de los marcos generados. |

### 02 Layout → Key Placement

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Minimum Start To Key Graph Distance` | `3` | Distancia mínima de conexiones desde Start para ubicar la Key en una rama; no es una distancia en centímetros. |

## ADungeonBlueprintForgeGenerator


### Configuration

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Configuration` | `Sin asignar` | Generation Config que contiene las salas candidatas y las reglas de esta mazmorra. |

### Preview

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Preview Seed` | `1` | Seed del botón de vista previa. Repite el mismo valor para comparar cambios. |

## FDungeonBlueprintForgeRoomSurfaceModule


### Dungeon Blueprint Forge

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Mesh` | `Sin asignar` | Malla que se utiliza para este módulo o regla de decoración. |
| `Material Override` | `Sin asignar` | Material opcional para todos los slots de esta malla. Vacío conserva los materiales del asset. |
| `Auto Orient Mesh` | `Activado` | Deduce la orientación del módulo a partir de sus dimensiones. Desactiva para orientar una malla manualmente. |
| `Rotation Offset` | `0°, 0°, 0°` | Corrección de orientación de la malla o actor en grados. |
| `Size Multiplier` | `1, 1, 1` | Multiplicador del tamaño del módulo antes de colocarlo; 1,1,1 mantiene su tamaño de referencia. |
| `Position Offset` | `0, 0, 0` | Desplazamiento visual local en centímetros para corregir el pivote. |

## FDungeonBlueprintForgeDecorationRule


### Decorations → Content

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Decoration Name` | `("Decoration")` | Nombre legible de esta regla; no cambia su colocación. |
| `Mesh` | `Sin asignar` | Malla que se utiliza para este módulo o regla de decoración. |
| `Material Override` | `Sin asignar` | Material opcional para todos los slots de esta malla. Vacío conserva los materiales del asset. |
| `Enabled` | `Activado` | Permite utilizar esta regla o salida. Desactivar conserva sus ajustes. |

### Decorations → Advanced

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Enable Collision` | `Activado` | Activa la colisión de la geometría generada. Se muestra en opciones avanzadas. |
| `Affect Navigation` | `Desactivado` | Permite que la geometría con colisión participe en la NavMesh. Se muestra en opciones avanzadas. |
| `Enable Physics Collision` | `Desactivado` | Añade colisión física además de consultas. Requiere Enable Collision. Se muestra en opciones avanzadas. |

### Decorations → Placement

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Placement Surface` | `Floor` | Floor coloca props sobre suelo; Wall sobre paredes. |
| `Wall Layout` | `SmartCentered` | Smart Centered reparte props centrados entre paredes distintas. Centered no prioriza paredes distintas. Random Legacy conserva el reparto antiguo. |
| `Floor Placement Zone` | `Edges` | Zona permitida: toda la sala, bordes, esquinas o centro. |

### Decorations → Quantity

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Decoration Layer` | `Minor` | Orden de colocación: Structural, Major, Minor. Las piezas previas reservan espacio. |
| `Max Instances` | `4` | Máximo de props de esta regla; cero la deja sin instancias. |
| `Minimum Instances` | `0` | Objetivo mínimo de esta regla si hay espacio seguro. No garantiza forzar props que no caben. |

### Decorations → Safety

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Can Use Combat Area` | `Desactivado` | Permite a esta regla de suelo ocupar la zona central reservada. Se muestra en opciones avanzadas. |

### Decorations → Quantity

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Selection Weight` | `1.0` | Peso relativo de selección. Cero excluye esta candidata o regla. Se muestra en opciones avanzadas. |

### Decorations → Safety

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Min Spacing` | `120.0` | Separación mínima entre props, en cm. |
| `Wall Side Margin` | `100.0` | Separación respecto a paredes para reglas de suelo, en cm. |
| `Corner Inset` | `80.0` | Separación interior respecto a esquinas, en cm. Se muestra en opciones avanzadas. |
| `Door Clearance` | `160.0` | Espacio libre alrededor de salidas para esta decoración, en cm. |

### Decorations → Placement

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Distance From Wall` | `5.0` | Separación del pivote respecto a la pared, en cm. |

### Decorations → Visual Adjustment

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Uniform Scale Range` | `(0.9, 1.1)` | Escala uniforme mínima y máxima de los props. La seed resuelve un valor entre ambas. Se muestra en opciones avanzadas. |
| `Rotation Range` | `(0.0, 360.0)` | Rango de giro horizontal de los props, en grados. Se muestra en opciones avanzadas. |
| `Position Adjustment` | `0, 0, 0` | Desplazamiento visual local en centímetros para corregir el pivote. Se muestra en opciones avanzadas. |

## ADungeonBlueprintForgeModularRoom


### 01 Room Layout

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Room Size` | `(800.0, 800.0, 300.0)` | Tamaño interior en cm: X anchura, Y profundidad y Z altura de pared. |
| `Randomize Room Size` | `Desactivado` | Elige un tamaño de sala reproducible por seed entre Minimum y Maximum Room Size. |
| `Room Shape` | `Rectangle` | Forma de la sala. Random elige L Left, L Right o T Shape; no incluye Rectangle ni Stairwell. |

### 01 Room Layout → L Shape

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `L Arm Size` | `(400.0, 400.0)` | Anchura de los dos brazos de una L, en cm. Solo se usa en L Left, L Right o Random. |

### 01 Room Layout → T Shape

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `T Arm Size` | `(300.0, 300.0)` | X es anchura del brazo central y Y profundidad de la barra superior de la T, en cm. |

### 01 Room Layout → Stairwell

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Stair Mesh Direction` | `Auto` | Auto lee la pendiente de la malla para orientar la subida. Elige otro sentido solo si la geometría no permite detectarla. |
| `Stair Repeat Count` | `1` | Copias completas de la escalera encadenadas rectas a su tamaño nativo. Aumenta la subida sin estirar la malla. |
| `Lower Landing Depth` | `200.0` | Longitud del descansillo inferior, en cm. |
| `Upper Landing Depth` | `200.0` | Longitud del descansillo superior, en cm. |
| `Upper Clearance` | `300.0` | Altura libre mínima sobre el descansillo superior, en cm. |
| `Allow Chest On Lower Landing` | `Activado` | Permite un cofre en el descansillo inferior si cabe. No coloca cofres sobre peldaños. |
| `Minimum Lower Landing Depth For Chest` | `500.0` | Profundidad mínima del descansillo para intentar su cofre, en cm; todavía debe superar las comprobaciones de espacio. |

### 01 Room Layout → Random Size

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Minimum Room Size` | `(600.0, 600.0, 300.0)` | Menor tamaño X/Y/Z para la sala aleatoria, en cm. |
| `Maximum Room Size` | `(1200.0, 1200.0, 500.0)` | Mayor tamaño X/Y/Z para la sala aleatoria, en cm. |
| `Auto Detect Grid Step` | `Activado` | Calcula incrementos válidos desde los módulos de suelo y pared. Recomendado para piezas modulares. Se muestra en opciones avanzadas. |
| `Manual Grid Step` | `(100.0, 100.0, 100.0)` | Incremento X/Y/Z de tamaño, en cm, cuando la detección del paso está desactivada. Se muestra en opciones avanzadas. |

### 02 Surface Modules

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Floor Module` | `Sin asignar` | Malla y ajustes del suelo. En Stairwell construye los descansillos planos. |
| `Wall Module` | `Sin asignar` | Malla y ajustes de paredes; también es el respaldo para pilares sin malla propia. |
| `Ceiling Module` | `Sin asignar` | Malla y ajustes del techo cuando Generate Ceiling está activo. |
| `Stair Module` | `Sin asignar` | Malla de escalera completa a tamaño nativo. Su geometría determina anchura, recorrido y subida. |

### 02 Surface Modules → Ceiling

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Generate Ceiling` | `Activado` | Construye el techo con el módulo asignado. |
| `Ceiling Overhang` | `5.0` | Margen horizontal extra del techo para cubrir el borde de la pared, en cm. |
| `Ceiling Height Offset` | `-5.0` | Corrección vertical del techo, en cm. Un valor negativo lo baja. Se muestra en opciones avanzadas. |

### 03 Structure → Pillars

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Generate Structural Pillars` | `Activado` | Genera pilares estructurales en las esquinas interiores de salas L y T. |
| `Pillar Size` | `(120.0, 120.0)` | Anchura y profundidad de los pilares, en cm. |
| `Pillar Corner Offset` | `(15.0, 15.0)` | Corrección tangencial e interior de la posición de los pilares, en cm. |
| `Pillar Mesh Override` | `Sin asignar` | Malla opcional de pilar. Vacío utiliza Wall Module. |
| `Pillar Material Override (All Slots)` | `Sin asignar` | Material para todos los slots del pilar. Solo se usa si la lista por slots está vacía. |
| `Pillar Material Overrides (Per Slot)` | `Lista vacía` | Material por slot del pilar. Un elemento vacío conserva el original del asset. |
| `Generate Rectangle Pillars` | `Activado` | Genera pilares de esquina y, según el reparto elegido, pilares intermedios en salas rectangulares. |
| `Rectangle Pillar Layout` | `Automatic` | Corners Only coloca esquinas. Corners And Evenly Spaced añade intermedios. Automatic resuelve el reparto de forma determinista. |
| `Target Pillar Spacing` | `600.0` | Separación objetivo entre pilares intermedios, en cm. |

### 03 Generation Options

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Enable Collision` | `Activado` | Activa la colisión de la geometría generada. Se muestra en opciones avanzadas. |
| `Enable Physics Collision` | `Desactivado` | Añade colisión física además de consultas. Requiere Enable Collision. Se muestra en opciones avanzadas. |
| `Affect Navigation` | `Desactivado` | Permite que la geometría con colisión participe en la NavMesh. Se muestra en opciones avanzadas. |

### 04 Connections

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Snap Connections To Walls` | `Activado` | Alinea las salidas con las paredes al cambiar el tamaño de la sala. |
| `Close Unused Connections` | `Activado` | Cierra con pared las salidas procedurales que la mazmorra no utiliza. |

### 04 Connections → Automatic

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Use Automatic Connections` | `Desactivado` | Configura las cuatro conexiones cardinales desde estos ajustes. Apágalo para usar componentes manuales. |
| `Enable North Connection` | `Activado` | Habilita la salida automática North, eje local +X. |
| `Enable East Connection` | `Activado` | Habilita la salida automática East, eje local +Y. En Stairwell es la puerta alta. |
| `Enable South Connection` | `Activado` | Habilita la salida automática South, eje local -X. |
| `Enable West Connection` | `Activado` | Habilita la salida automática West, eje local -Y. En Stairwell es la puerta baja. |
| `Connection Type` | `("Passage")` | Etiqueta de compatibilidad de las salidas automáticas. Passage y el nombre antiguo Door son compatibles. |
| `Opening Size` | `(200.0, 250.0)` | Anchura y altura libres de las puertas automáticas, en cm. |
| `Opening Center Height` | `125.0` | Altura del centro de la abertura sobre el suelo, en cm; normalmente es la mitad de Opening Size Y. |

### 05 Decorations

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Enable Decorations` | `Desactivado` | Genera las reglas decorativas después de aceptar la sala. |
| `Decoration Density` | `Standard` | Densidad de props: Minimal usa menos, Standard es intermedia y Detailed usa más. |
| `Maximum Props Per Room` | `16` | Máximo conjunto de props de todas las reglas en una sala. |

### 05 Decorations → Safety

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Pillar Clearance` | `50.0` | Espacio adicional libre alrededor de los pilares estructurales, en cm. |
| `Reserve Combat Area` | `Activado` | Reserva una zona central sin props de suelo para tránsito o combate. |
| `Combat Area Radius` | `300.0` | Radio de la zona central protegida, en cm. |

### 05 Decorations → Advanced

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Placement Attempts Per Rule` | `16` | Máximo de intentos por regla. Si no cabe, se omite para continuar con las demás. Se muestra en opciones avanzadas. |

### 05 Decorations

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Decoration Rules` | `Lista vacía` | Una regla por tipo de prop: malla, superficie, reparto, cantidad y espacio libre. |

### 06 Torches

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Enable Procedural Torches` | `Desactivado` | Coloca actores de antorcha del proyecto después de aceptar la sala. |
| `Torch Actor Class` | `Sin asignar` | Blueprint Actor de antorcha del proyecto host; controla su propio arte. |
| `Maximum Torches Per Room` | `2` | Máximo de antorchas por sala; cero no coloca ninguna. |

### 06 Torches → Placement

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Preferred Wall Spacing` | `500.0` | Separación preferida entre antorchas de una pared, en cm. |
| `Height From Floor` | `180.0` | Altura del ancla de antorcha sobre el suelo, en cm. |
| `Door Clearance` | `220.0` | Espacio libre alrededor de puertas para colocar antorchas, en cm. |
| `Distance From Wall` | `75.0` | Distancia del ancla desde la pared hacia el interior, en cm. |
| `Torch Minimum Spacing` | `350.0` | Distancia mínima entre antorchas, en cm. |
| `Structural Pillar Clearance` | `100.0` | Distancia libre alrededor de pilares al colocar antorchas, en cm. |

### 06 Torches → Advanced

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Torch Forward Faces Room` | `Activado` | Actívalo si +X del Blueprint de antorcha apunta desde la pared hacia el interior. |

### 06 Torches → Performance

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Light Attenuation Radius` | `800.0` | Radio de influencia de las luces de la antorcha, en cm. |
| `Light Max Draw Distance` | `2000.0` | Distancia máxima de dibujado de las luces de antorcha, en cm. |
| `Light Fade Range` | `400.0` | Rango de desaparición gradual de las luces de antorcha, en cm. |

### 07 Room Fill Light

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Enable Room Fill Light` | `Activado` | Crea una luz de relleno sin sombras para reducir zonas completamente negras. |

### 07 Room Fill Light → Optimization

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Isolate Room Local Lights From Player` | `Activado` | Usa canal de luz 1 para luces locales de la sala y antorchas; un jugador en canal 0 no las recibe. |

### 07 Room Fill Light

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Intensity` | `1800.0` | Intensidad del relleno en lúmenes. |
| `Color` | `(0.55, 0.65, 1.0)` | Color de la luz de relleno. |
| `Height Below Ceiling` | `120.0` | Distancia de la luz bajo el techo, en cm. |
| `Local Offset` | `0, 0` | Corrección X/Y desde el centro de la luz, en cm. |

### 07 Room Fill Light → Performance

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Fill Light Attenuation Radius` | `1400.0` | Radio de influencia del relleno, en cm. |
| `Fill Light Max Draw Distance` | `3000.0` | Distancia máxima de dibujado del relleno, en cm. |
| `Fill Light Fade Range` | `500.0` | Rango de desaparición gradual del relleno, en cm. |

### 08 Preview

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Preview Seed` | `1` | Seed del botón de vista previa. Repite el mismo valor para comparar cambios. |

## ADungeonBlueprintForgePackedRoom


### 01 Packed Room

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Packed Level Actor Class` | `Sin asignar` | Blueprint Packed Level Actor que contiene el arte prehecho de esta sala. |
| `Number Of Exits` | `1` | Cantidad de salidas nativas activas, de Exit1 a Exit4. Gobierna Enabled y los IDs de esas flechas. |

### 04 Connections → Detection

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Auto Center Exits` | `Desactivado` | En editor, busca aberturas en la geometría Packed y coloca sus flechas. Requiere Bounds Mode Automatic; revisa y guarda el resultado. |

### 03 Unused Exits

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Unused Exit Door Class` | `Sin asignar` | Actor del proyecto para cerrar una salida sin conexión. Conserva su escala y centra sus Static Mesh visibles en el hueco. |
| `Unused Exit Door Height Offset` | `0.0` | Corrección vertical explícita del centro del actor de cierre, en cm. Positivo sube. |
| `Unused Exit Door Forward Offset` | `0.0` | Corrección del cierre en el eje +X de la salida, en cm. Positivo va hacia fuera. |
| `Unused Exit Door Right Offset` | `0.0` | Corrección del cierre en el eje +Y de la salida, en cm. |

### 02 Packed Bounds

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Bounds Mode` | `Manual` | Manual usa componentes Bounds de tu Blueprint. Automatic aproxima la geometría Packed mediante varias cajas. |
| `Automatic Bounds Cell Size` | `25.0` | Tamaño de celda de la aproximación XY, en cm. Menor es más preciso y cuesta más calcular. |
| `Automatic Bounds Margin` | `0.0` | Margen añadido a cada caja automática, en cm. |
| `Automatic Bounds XYInset` | `0.0` | Contracción horizontal de las cajas, en cm. Valores grandes pueden permitir solapes visibles. |
| `Maximum Automatic Bounds` | `16` | Máximo de cajas automáticas. Si se necesitan más, se fusionan regiones cercanas. |

## ADungeonBlueprintForgeRoomBase


### Room

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Allowed Rotations` | `0°, 90°, 180°, 270° (constructor)` | Rotaciones de la sala que el planificador puede probar. Mantén al menos una. |

## UDungeonBlueprintForgeConnectionComponent


### Connection

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Enabled` | `Activado` | Permite utilizar esta regla o salida. Desactivar conserva sus ajustes. |
| `Connection Id` | `("Door")` | Identificador único de esta salida dentro de la sala. Las salidas nativas automáticas lo asignan por sí mismas. |
| `Connection Type` | `("Door")` | Tipo lógico compatible entre puertas. Door y Passage son equivalentes; otros nombres exigen coincidencia. |
| `Opening Size` | `(200.0, 250.0)` | Anchura y altura libres del hueco, en cm. No redimensiona el actor de cierre Packed. |
| `Already Has Door Frame` | `Desactivado` | Marca una salida cuyo arte ya contiene marco. Omite el marco generado en ese punto y en el contacto directo compartido. |

## UDungeonBlueprintForgeBoundsComponent


### Bounds

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Bounds Id` | `("MainBounds")` | Identificador de este volumen de ocupación dentro de la sala. |

## UDungeonBlueprintForgeMarkerComponent


### Marker

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Marker Type` | `("Marker")` | Etiqueta que el juego host consulta para situar contenido; el componente no crea gameplay. |

## UDungeonBlueprintForgeRoomDefinition


### 01 Room

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Room Class` | `Sin asignar` | Blueprint hijo de Room Base, Modular Room o Packed Room que presenta esta candidata. |

### 05 Preload

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Additional Preload Assets` | `Lista vacía` | Assets visuales extra que la generación Staged debe precargar para esta definición. Se muestra en opciones avanzadas. |

### 01 Room

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Category` | `Normal` | Papel de la candidata: Start, Normal, Key, Boss, Hub o Reward. Special está reservado para extensiones futuras. |

### 03 Gameplay

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Gameplay Zone` | `Safe` | Metadato para el juego host: Safe, Combat, Special o MiniBoss. No genera enemigos. |

### 02 Selection

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Selection Weight` | `1.0` | Peso relativo de selección. Cero excluye esta candidata o regla. |
| `Enabled` | `Activado` | Permite utilizar esta regla o salida. Desactivar conserva sus ajustes. |

### 04 Chests

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Chest Spawn Style` | `Sin asignar` | Estilo opcional de cofres de esta definición. Vacío no genera cofres. |

## FDungeonBlueprintForgeGenerationRequest


### Dungeon Blueprint Forge

| Campo en Unreal | Valor inicial | Qué hace |
|---|---|---|
| `Seed` | `1` | Entero de 32 bits para reproducir el layout; admite valores negativos. |
| `Use Random Seed` | `Desactivado` | Ignora Seed y resuelve un entero aleatorio; consulta Resolved Seed para repetirlo. |
| `Normal Room Count` | `10` | Presupuesto de salas normales solicitado; rango soportado 0 a 69. No cuenta los papeles especiales. |

## Campos históricos

`Stair Rise`, `Stair Step Count` y `Stair Step Depth` se conservan serializados
para no romper assets antiguos, pero permanecen ocultos. La escalera actual utiliza
su malla completa y `Stair Repeat Count`. `Connector` y el papel futuro `Special`
no se ofrecen como categorías nuevas de Room Definition.

El actor de cierre Packed conserva su tamaño y solo se centra. No existen ajustes
de autoescalado ni un fondo generado por el plugin.

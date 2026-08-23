# Referencia de Data Assets

Los Data Assets separan reglas y contenido del código. Crea los tres desde el
Content Browser con `Add > Miscellaneous > Data Asset`; asigna siempre arte y
Blueprints de tu proyecto, no dentro de `Source/` del plugin.

## 1. Dungeon Blueprint Forge Generation Config

Es el Asset que se asigna en **Configuration** del `DungeonBlueprintForgeGenerator`.
Define qué salas son candidatas y cómo se construye el mapa; no guarda el
progreso de una partida.

### Rooms

| Campo | Qué hace | Regla práctica |
|---|---|---|
| `Start Rooms` | Definitions posibles para el inicio. | Debe haber al menos una válida. |
| `Normal Rooms` | Presupuesto de salas normales. | Debe existir si `Normal Room Count` es mayor que cero. |
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

### Corridors y Special Rooms

| Campo | Significado |
|---|---|
| `Generate Straight Corridors` | Activa la geometría visual del corredor. Requiere un Style válido. |
| `Straight Corridor Style` | Data Asset visual de suelo, paredes, techo, alineación y colisión. |
| `Minimum Start To Key Graph Distance` | Mínimo de pasos lógicos desde Start antes de colocar Key. No es distancia en centímetros. |
| `bDrawDebug` | Campo obsoleto conservado por compatibilidad; no cambia la generación actual. |

## 2. Dungeon Blueprint Forge Room Definition

Representa una sala seleccionable. Crea una Definition por rol aunque dos roles
apunten temporalmente al mismo Blueprint.

| Campo | Significado |
|---|---|
| `Room Class` | Blueprint hijo de `DungeonBlueprintForgeRoomBase` o `DungeonBlueprintForgeModularRoom`. |
| `Category` | Rol: Start, Normal, Hub, Reward, Key o Boss. Debe coincidir con la lista donde se añade. |
| `Selection Weight` | Probabilidad relativa entre candidatas compatibles. `0` evita que se seleccione. |
| `Enabled` | Activa/desactiva sin borrar el Asset. |

Una Definition no crea geometría: solo define cómo puede participar un Blueprint
de sala ya válido.

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

## Validación rápida de los tres Assets

1. Todas las listas obligatorias tienen al menos una Definition `Enabled`.
2. Cada Definition apunta a una Room Class válida y su Category coincide.
3. Si Connection Mode puede crear pasillos, el Style tiene mallas válidas.
4. Repite una seed fija antes de cambiar pesos, tamaños o conexiones: es la
   manera más rápida de saber qué ajuste cambió el resultado.

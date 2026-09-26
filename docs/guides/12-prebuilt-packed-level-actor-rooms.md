# Rooms prehechas con Packed Level Actor

Cada Room utiliza un único Blueprint contenedor:

1. Crea el Blueprint de `Packed Level Actor` desde tu geometría estática.
2. Crea un Blueprint hijo de `Dungeon Blueprint Forge Packed Room`.
3. Asigna `Packed Level Actor Class` en Class Defaults.
4. Elige `Bounds Mode`: `Automatic` calcula varias cajas desde las mallas;
   `Manual` usa `Main Room Volume` y los componentes Bounds que coloques tú.
   Si eliges Manual para una L o T, usa varias cajas y deja libre el hueco.
5. Selecciona `Number Of Exits` entre 1 y 4, coloca las flechas en las puertas y
   orienta `+X` hacia fuera.
6. Si hay más de una salida, asigna en `Unused Exit Door Class` un Blueprint de
   puerta del proyecto host, con `+X` hacia fuera. El plugin centra sus Static
   Mesh visibles en el hueco aunque el pivote del Blueprint esté desplazado.
7. Configura `Allowed Rotations`; compila y guarda el Blueprint contenedor.
   Coloca una instancia temporal en un nivel. Si usas Automatic, pulsa
   `Rebuild Automatic Bounds` en Details y comprueba las cajas azules.
8. Pulsa `Validate Room And Log` en esa instancia y lee Output Log. Corrige
   cualquier error antes de crear su Room Definition.

No hay actor Layout, Bake ni Presentation Mode. Los IDs se asignan
automáticamente. Durante la planificación el contenedor es ligero; la geometría
Packed aparece al aceptar el layout y se crea dentro del flujo staged.

Packed Level Actor está orientado principalmente a geometría estática. Puertas,
enemigos, loot y encuentros siguen perteneciendo al proyecto host.

El generador puede entrar por cualquier salida compatible. Las salidas que no
conecte con otra Room reciben una instancia de `Unused Exit Door Class` al
aceptar la distribución; `Clear Dungeon` la elimina. Las Packed Rooms ya
creadas con más de una salida deben asignar esa clase antes de generar.

## Añadirla al generador

1. Crea una `Dungeon Blueprint Forge Room Definition` y asigna el Blueprint
   contenedor en `Room Class`.
2. Para usarla entre otras Rooms, elige `Category = Normal`, activa `Enabled`,
   pon `Selection Weight` mayor que `0` y añade la Definition a `Normal Rooms`
   del Generation Config. Necesita al menos dos salidas válidas: una para
   conectarse con la Room anterior y otra para continuar.
3. Genera con una seed fija, preferiblemente con
   `Generate Dungeon From Seed Staged`. Confirma que la Room aparece como
   intermedia, que no se solapa y que cada salida libre recibe su puerta.
   Guarda `Resolved Seed` y `Last Result > Message` si falla.

La Room puede conectarse por cualquier salida compatible; `Exit1` no es una
entrada obligatoria. Una Room con una sola salida puede terminar una rama.

## Bounds automáticos

En `Class Defaults`, `Bounds Mode = Automatic` calcula varias cajas a partir de la geometría Packed. Los valores predeterminados para nuevas Packed Rooms son `Automatic Bounds Cell Size = 25 cm`, `Automatic Bounds Margin = 0 cm` y `Maximum Automatic Bounds = 16`. En Rooms grandes, el tamaño efectivo de celda puede aumentar para cubrir toda la geometría.

Los Blueprints existentes pueden conservar valores anteriores. Revísalos en
`Class Defaults`; después compila y guarda el Blueprint. Coloca una instancia
temporal en un nivel, ejecuta `Rebuild Automatic Bounds` desde Details y
comprueba visualmente las cajas antes de usar la Room con una seed fija. El
botón de la instancia actualiza esa instancia y marca el nivel; no guarda por
sí solo los Class Defaults del Blueprint.

Para una Room L/T, varias cajas azules deben seguir la geometría sin ocupar todo
el rectángulo exterior. El cálculo preciso analiza triángulos XY en el editor,
rellena el interior y usa hasta 16 cajas. El valor efectivo de la celda puede
crecer en Rooms grandes. Compila y guarda el Blueprint contenedor con los
bounds correctos antes de distribuir: una build empaquetada conserva esas
cajas guardadas y no dispone necesariamente de los vértices CPU para repetir
el cálculo preciso. Si la instancia muestra cajas distintas, vuelve al
Blueprint contenedor, revisa sus Class Defaults, compila, guarda y ábrelo otra
vez para confirmar los bounds que se distribuirán.

## Puertas de salidas libres

La flecha Exit debe estar en el centro del hueco, a la altura del centro de la
puerta. En `Class Defaults > Dungeon Blueprint Forge > Packed Room > Unused Exits`
puedes corregir cada Blueprint de Room sin modificar la puerta del proyecto:

| Ajuste | Efecto |
|---|---|
| `Unused Exit Door Height Offset` | Sube o baja la puerta en cm. |
| `Unused Exit Door Forward Offset` | Mueve en el eje `+X` de la flecha: hacia fuera si es positivo. |
| `Unused Exit Door Right Offset` | Mueve en el eje `+Y` local de la flecha. |
| `Unused Exit Door Scale` | Escala local X grosor, Y anchura y Z altura; `1,1,1` conserva el tamaño. |

El plugin aplica la escala y vuelve a centrar las Static Mesh visibles antes de
añadir los offsets. Usa una seed fija, cambia un ajuste cada vez y comprueba
el encaje de cada Exit libre. La prueba visual del 2026-09-26 confirmó el uso
de Packed Room y una mejora del ajuste de puertas; las nuevas variantes y una
build empaquetada siguen pendientes de la prueba del usuario.

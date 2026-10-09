# Ajustes de salas procedurales — UE 5.8

Configura el Blueprint hijo de `DungeonBlueprintForgeModularRoom` en **Class
Defaults**. Los nombres siguientes coinciden con el editor; sus ayudas están
en español. Para consultar todos los campos, abre [el catálogo de opciones](../reference/editor-options.md).

## 01 Room Layout: forma y tamaño

| Campo | Qué haces con él |
|---|---|
| `Room Shape` | Empieza con Rectangle. L Left, L Right y T Shape añaden brazos. Random elige entre esas L/T; puede resolver Rectangle si un brazo no cabe. |
| `Room Size` | X anchura, Y profundidad y Z altura interior, en cm. Usa múltiplos de tus tiles. |
| `Randomize Room Size` | Apágalo mientras ajustas el arte; actívalo después para variar por seed. |
| `Minimum / Maximum Room Size` | Rango del tamaño aleatorio. |
| `Auto Detect Grid Step` | Detecta los incrementos desde las mallas. Manténlo activo para módulos regulares. |
| `Manual Grid Step` | Incrementos X/Y/Z cuando desactivas la detección. |
| `L Arm Size` / `T Arm Size` | Dimensiones de los brazos de L/T. También puedes configurarlos con Room Shape Random. |

Para subir entre alturas utiliza [DBF Stairwell Room](09-procedural-stairwell.md).
La subida real depende de la malla completa y `Stair Repeat Count`.

## 02 Surface Modules: arte de la sala

Asigna `Floor Module` y `Wall Module`. `Ceiling Module` se utiliza con
`Generate Ceiling`; `Stair Module` pertenece a Stairwell.

| Campo dentro de un módulo | Qué hace |
|---|---|
| `Mesh` | Malla que se repite. |
| `Material Override` | Material para todos sus slots. Vacío conserva los materiales del asset. |
| `Auto Orient Mesh` | Deduce la orientación desde las dimensiones. Desactívalo para orientar manualmente. |
| `Rotation Offset` | Corrección de giro de una malla importada en otro eje. |
| `Size Multiplier` | Multiplicador previo al teselado. Empieza con 1,1,1. |
| `Position Offset` | Corrección local del pivote, en cm. |

Si aparece material gris en HISM, comprueba **Used with Instanced Static
Meshes** en el material padre, aplica y guarda. Revisa primero esa compatibilidad.

`Ceiling Overhang` añade margen horizontal; `Ceiling Height Offset` desplaza
verticalmente el techo. Un valor negativo lo baja.

## 03 Structure: pilares y colisión

Los pilares de L/T y los de Rectangle comparten tamaño, mesh y materiales,
pero tienen interruptores propios. En Rectangle empieza con `Corners Only`;
`Corners And Evenly Spaced` añade intermedios. `Target Pillar Spacing` controla
la separación deseada. Las reglas decorativas y antorchas respetan su espacio.

`Pillar Material Override (All Slots)` se usa si la lista por slots está vacía.
Para mallas con varios materiales utiliza la lista por slots; un elemento vacío
conserva su original.

`Enable Collision` activa consultas. `Enable Physics Collision` añade física.
`Affect Navigation` permite participar en la NavMesh si la colisión está activa;
todavía necesitas el NavMesh Bounds Volume del nivel y una prueba con tu IA.

## 04 Connections: salidas

Activa `Use Automatic Connections` para configurar las salidas cardinales
desde North, East, South y West. Los grupos individuales muestran sus valores;
los campos gobernados por el modo automático quedan de solo lectura.

| Campo | Uso |
|---|---|
| `Snap Connections To Walls` | Mantiene las flechas alineadas con las paredes al variar el tamaño. |
| `Close Unused Connections` | Cierra con pared las aberturas que no se usan. |
| `Connection Type` | Tipo compatible; Passage y Door son equivalentes. |
| `Opening Size` | Ancho y alto del hueco, en cm. |
| `Opening Center Height` | Altura del centro del hueco sobre el suelo. Normalmente la mitad de su altura. |
| `Already Has Door Frame` | El arte de esa salida ya tiene marco. El generador no añade otro en ese punto. |

Para puertas artísticas, apaga las automáticas y usa componentes Connection
manuales. Evita dos flechas en el mismo lugar. En el editor completo selecciona
la flecha para mover su Transform; +X apunta hacia fuera.

## 05 Decorations: props

Activa `Enable Decorations` y añade una regla por tipo de prop en
`Decoration Rules`. Define malla, superficie, reparto, cantidad y espacio libre.
`Maximum Props Per Room` limita todas las reglas combinadas. Empieza con 8–12.
`Minimal`, `Standard` y `Detailed` cambian la densidad.

Para cajas o barriles usa `Placement Surface = Floor` y `Floor Placement Zone`
Edges o Corners. `Reserve Combat Area` protege el centro; una regla puede
permitirse entrar mediante `Can Use Combat Area`.

Para un banner o cuadro prueba:

| Campo | Valor inicial de prueba |
|---|---|
| Placement Surface | Wall |
| Wall Layout | Smart Centered (Recommended) |
| Minimum / Maximum Instances | 1 / 1 |
| Minimum Spacing | 300 cm |
| Door Clearance | 250 cm |
| Distance From Wall | 5–10 cm |
| Rotation Range | 0 / 0; 180 / 180 si el asset mira hacia la pared |

Smart Centered centra el prop y prioriza paredes aún no usadas. Centered no
prioriza paredes diferentes. Random Legacy conserva el reparto antiguo.
Los mínimos son objetivos sujetos a que exista espacio seguro.

## 06 Torches: actores del proyecto

Activa `Enable Procedural Torches`, asigna `Torch Actor Class` y empieza con
dos antorchas. Configura altura, distancia de pared y separación. `Door
Clearance` y `Structural Pillar Clearance` protegen puertas y pilares.

Si +X del Blueprint apunta desde la pared hacia el interior, deja activo
`Torch Forward Faces Room`. Los controles de radio, distancia de dibujado y
fade están en **06 Torches → Performance**.

## 07 Room Fill Light: relleno

`Enable Room Fill Light` crea una Point Light sin sombras. Ajusta `Intensity`
en lúmenes y `Color`. Para bajarla aumenta `Height Below Ceiling`; `Local
Offset` solo cambia X/Y. Radio, dibujado y fade están en Performance.

`Isolate Room Local Lights From Player` usa canal de luz 1 para el relleno y
las antorchas. La geometría generada recibe canales 0 y 1; un jugador por
defecto en canal 0 no recibe esas luces locales.

## 08 Preview: comprobar la sala

Compila el Blueprint, coloca una instancia y usa `Preview Seed` más `Rebuild
Preview`. Repite una seed al comparar cambios. Comprueba primero geometría y
puertas; añade props y luces después. Los componentes internos siguen en el
árbol Components del editor completo, agrupados una sola vez en el panel.

# Ajustes de una sala procedural, explicados

Esta guía es para el Blueprint hijo de `DungeonBlueprintForgeModularRoom`. Los
nombres de las opciones se mantienen exactamente como aparecen en Unreal, pero
las explicaciones indican qué tocar primero y qué puedes dejar por defecto.

## Orden sencillo para configurar una sala

1. Define forma y tamaño en **01 Room Layout**.
2. Asigna los tres meshes en **02 Surface Modules**.
3. Configura las puertas en **04 Connections**.
4. Pulsa **Rebuild Preview** y deja la sala base correcta antes de añadir props.
5. Añade decoración, antorchas y luz de relleno al final.

No hace falta cambiar todas las opciones: los valores por defecto están pensados
para una sala rectangular válida cuando los módulos de mesh están asignados.

## 01 Room Layout

| Opción | Qué cambia | Punto de partida |
|---|---|---|
| `Room Shape` | Plano de la sala: Rectangle, L o T. | `Rectangle` para la primera sala. |
| `Room Size` | Anchura interior X/Y y altura del techo. | Múltiplos del tamaño de tu tile de suelo. |
| `Randomize Dimensions` | La seed escoge el tamaño. | Apagado al crear la sala; actívalo tras comprobar el preview. |
| `Minimum Room Size` / `Maximum Room Size` | Límites del tamaño aleatorio. | Compatibles con puertas y módulos. |
| `Auto Detect Dimension Step` | Detecta el tamaño de los módulos para teselar bien. | Activo con assets modulares normales. |
| `Dimension Step` | Paso manual si desactivas la detección. | Igual al tile de suelo X/Y y a la altura de pared. |

En salas L y T, los ajustes de brazos cambian el suelo caminable real, no son
controles decorativos.

## 02 Surface Modules

`Floor Module`, `Wall Module` y `Ceiling Module` definen los meshes repetidos
de la sala.

| Opción del módulo | Uso |
|---|---|
| `Mesh` | Asset modular que se repite. Es obligatorio. |
| `Material Override` | Opcional. Vacío mantiene el material que ya tiene el mesh. |
| `Rotation Offset` | Corrige un asset importado en otro eje. |
| `Size Multiplier` | Ajuste fino; normalmente 1.0. |
| `Position Offset` | Corrige un pivote o una pequeña separación sin editar el mesh. |

Si un mesh instanciado sale gris, abre su **material padre**, activa **Used with
Instanced Static Meshes**, pulsa Apply y Save. Es un ajuste del material de
Unreal; no necesitas forzar un `Material Override`.

## 03 Structure

Aquí están techo y pilares estructurales. Activa el techo solo si el arte lo
necesita. Empieza los pilares en esquinas y aumenta la densidad poco a poco;
decoración y antorchas respetan el espacio reservado por pilares.

## 04 Connections

| Opción | Uso |
|---|---|
| `Snap Connections To Generated Walls` | Mantiene puertas en la pared real si cambia el tamaño. Déjalo activo. |
| `Close Unused Connections` | Cierra huecos no usados por el generador. Déjalo activo. |
| `Use Automatic Connections` | Crea puertas cardinales sin añadir Components manuales. |
| `North`, `East`, `South`, `West` | Elige qué puertas automáticas existen. |
| Tipo, tamaño y altura automáticos | Se aplican a esas puertas automáticas. |

Usa Components manuales cuando una puerta deba estar en una posición artística
especial. No pongas una puerta automática y otra manual en el mismo sitio.

## 05 Decorations

Activa `Enable Decorations`, escoge `Decoration Density` y añade reglas en
`Decoration Rules`. Cada regla describe **un tipo de prop**, no una instancia
colocada a mano.

### Controles de la sala

| Opción | Uso |
|---|---|
| `Decoration Density` | `Minimal` genera menos; `Standard` es normal; `Detailed` añade detalle visual. |
| `Maximum Props Per Room` | Límite total de props. Empieza por 8–12. |
| `Structural Pillar Clearance` | Espacio libre alrededor de pilares. Súbelo para props grandes. |
| `Reserve Combat Area` / `Combat Area Radius` | Reserva el centro para combate. Recomendado en salas de pelea. |
| `Placement Attempts Per Rule` | Límite de seguridad. Déjalo por defecto salvo que una regla no quepa repetidamente. |

### Regla de decoración: orden para configurarla

1. **Content**: asigna `Mesh`; deja `Material Override` vacío salvo que quieras
   otro material de forma deliberada.
2. **Placement**: elige `Placement Surface` (`Floor` o `Wall`).
3. **Quantity**: `Minimum Instances`, `Maximum Instances` y `Selection Weight`.
4. **Safety**: clearances para no bloquear pilares ni puertas.
5. **Visual Adjustment**: escala, rotación o posición solo si el asset lo necesita.

### Decoración de pared: banners, cuadros y apliques

Para un banner, selecciona `Placement Surface = Wall` y empieza así:

| Opción | Valor inicial | Por qué |
|---|---:|---|
| `Wall Layout` | `Smart Centered (Recommended)` | Centra el item en su pared y prefiere paredes válidas no usadas antes de repetir. |
| `Minimum Instances` / `Maximum Instances` | `1 / 1` | Un banner por regla. Sube el número solo tras mirar el preview. |
| `Minimum Spacing` | `300 cm` | Evita que props de pared se amontonen. |
| `Door Clearance` | `250 cm` | Mantiene las puertas claras. |
| `Distance From Wall` | `5–10 cm` | Mete el mesh hacia la sala. Súbelo solo si entra en la pared. |
| `Rotation Range` | `0 / 0` | Conserva la orientación del asset. Usa `180 / 180` solo si mira hacia la pared. |
| `Position Adjustment` | `0, 0, 0` | Z sube/baja; los otros ejes son correcciones propias del asset. |

`Smart Centered` es la opción normal: reparte props de pared por paredes válidas
antes de reutilizar una y los centra visualmente. `Centered` también centra pero
no prioriza una pared no usada. `Random (Legacy)` conserva el comportamiento
antiguo aleatorio de bordes/esquinas.

Para props de suelo, `Floor Placement Zone` elige el área. Usa `Edges` o
`Corners` para cajas/barriles y deja `Center` para un prop focal único.

## 06 Torches

Las antorchas son Actors del proyecto que usa el plugin. Asigna el Blueprint
propio en `Torch Actor Class`; el plugin no incluye tu arte de antorcha.

| Opción | Uso |
|---|---|
| `Maximum Torches Per Room` | Empieza con 2 en una sala normal. |
| `Preferred Wall Spacing` / `Torch Minimum Spacing` | Evitan antorchas cercanas. Más valor = más separación. |
| `Height From Floor` | Sube o baja el anclaje de la antorcha. |
| `Door Clearance` | Aleja fuego de las puertas. |
| `Distance From Wall` | Mueve la antorcha hacia la sala. Súbelo si entra en pared. |
| `Structural Pillar Clearance` | Evita que un pilar tape la antorcha. |
| `Torch Forward Faces Room` | Déjalo activo si el eje +X del Blueprint mira de pared a sala. |

El generador usa paredes distintas antes de repetir una cuando hay paredes
válidas suficientes. También descarta posiciones bloqueadas por pilares,
conexiones u otra antorcha.

Para rendimiento: radio de luz dentro de la sala, sombras apagadas y draw
distance/fade para que luces lejanas dejen de renderizarse.

## 07 Room Fill Light

Una Point Light opcional sin sombras que evita zonas completamente negras. Es
luz de apoyo, no sustituye el tono cálido de las antorchas.

| Opción | Uso |
|---|---|
| `Intensity` | Baja; súbela solo hasta leer siluetas. |
| `Color` | Azul-gris suave para separarla de antorchas naranjas. |
| `Height Below Ceiling` | Aumenta este valor para bajar la luz. |
| `Local Offset` | X/Y la mueven desde el centro procedural; no cambia altura. |
| Rendimiento | Atenuación dentro de sala y draw distance/fade para salas lejanas. |

## 08 Preview

Asigna `Preview Seed` y pulsa `Rebuild Preview` para comprobar una sola sala
antes de generar toda la mazmorra. Con seed fija puedes comparar: cambia un
ajuste, reconstruye la misma seed y decide si mantenerlo.

## Preset seguro inicial

Sala Rectangle, tamaño fijo, techo si tu arte lo necesita, conexiones automáticas,
decoración `Standard`, máximo 8–12 props, un banner `Smart Centered`, dos
antorchas y una fill light baja sin sombras. Añade detalle solo cuando eso se
vea limpio.

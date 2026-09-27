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
botón copia las cajas calculadas a los valores predeterminados del Blueprint
contenedor. Guarda ese Blueprint después de comprobar la instancia.

Para una Room L/T, varias cajas azules deben seguir la geometría sin ocupar todo
el rectángulo exterior. El cálculo preciso analiza triángulos XY en el editor,
rellena el interior y usa hasta 16 cajas. El valor efectivo de la celda puede
crecer en Rooms grandes. Compila y guarda el Blueprint contenedor con los
bounds correctos antes de distribuir: una build empaquetada conserva esas
cajas guardadas y no dispone necesariamente de los vértices CPU para repetir
el cálculo preciso. Si la instancia muestra cajas distintas, vuelve al
Blueprint contenedor, revisa sus Class Defaults, compila, guarda y ábrelo otra
vez para confirmar los bounds que se distribuirán.

### Error: «La habitación necesita al menos un componente Dungeon Blueprint Forge Bounds»

Una Packed Room nueva necesita sus **propios** Bounds guardados, aunque otra
Packed Room ya funcione. El error no depende de que tenga dos o más salidas:

1. Abre la `Room Definition` citada en el mensaje. Confirma que `Room Class`
   apunta al Blueprint contenedor de esa Room, no al Packed Level Actor ni a
   otro Blueprint de prueba.
2. En el Blueprint contenedor, confirma `Packed Level Actor Class` y `Bounds
   Mode = Automatic`. Compila y guarda.
3. Coloca una instancia temporal de ese mismo Blueprint en un nivel. Pulsa
   `Rebuild Automatic Bounds` en sus Details y comprueba que aparecen cajas
   azules con tamaño real. Guarda de nuevo el Blueprint.
4. Ejecuta `Validate Room And Log` en la instancia, revisa Output Log y repite
   la generación con la misma seed.

Si la instancia muestra cajas, pero el generador sigue informando que faltan
Bounds, conserva capturas de `Room Class`, los componentes del Blueprint, el
resultado de validación y `Resolved Seed`. Puede existir una diferencia entre
la instancia visible y los Bounds guardados para la candidata.

## Puertas de salidas libres

La flecha Exit debe estar en el centro del hueco, a la altura del centro de la
puerta. En `Class Defaults > Dungeon Blueprint Forge > Packed Room > Unused Exits`
puedes corregir cada Blueprint de Room sin modificar la puerta del proyecto:

| Ajuste | Efecto |
|---|---|
| `Unused Exit Door Height Offset` | Sube o baja la puerta en cm. |
| `Unused Exit Door Forward Offset` | Mueve en el eje `+X` de la flecha: hacia fuera si es positivo. |
| `Unused Exit Door Right Offset` | Mueve en el eje `+Y` local de la flecha. |
| `Fit Unused Exit Door To Opening` | Ajusta automáticamente anchura y altura al hueco detectado de cada Exit; en modo Manual usa `Opening Size`. |
| `Unused Exit Door Scale` | Multiplicador posterior en los ejes locales del Actor de puerta; `1,1,1` conserva el ajuste automático. |

La Room guarda `Detected Exit Opening Sizes` para Exit1..Exit4 al buscar los
huecos con `Auto Center Exits`. La búsqueda admite puertas mucho mayores que
el `Opening Size` lógico y prefiere el contorno más amplio cuando se solapan
medidas del mismo vano. Tras actualizar el plugin, vuelve a activar `Auto
Center Exits` en una instancia y guarda el Blueprint para renovar medidas
antiguas. El detector comprueba la intersección real de los triángulos del muro
con la cuadrícula para evitar que un rectángulo envolvente invada el vano.
Revisa que no haya seleccionado un hueco decorativo. El plugin mide las Static
Mesh visibles de la puerta y comprueba qué ejes locales del Actor modifican
realmente su ancho y alto en el plano del Exit. Después aplica `Unused Exit Door
Scale` y vuelve a centrar
la geometría antes de añadir los offsets. Si el hueco es más alto que `Opening
Size`, corrige la altura del centro para mantener la base en el umbral. Compila y guarda el Blueprint Packed
después de detectar los huecos. Si `Auto Center Exits` estaba desactivado antes
de esta mejora, actívalo una vez para medirlos y guarda la Room; después puedes
desactivarlo para retocar las flechas manualmente. Usa una seed fija, cambia un ajuste cada vez y
comprueba el encaje de cada Exit libre. Si la puerta y el hueco tienen siluetas
distintas, puede hacer falta un marco o una puerta apropiada: la escala no
cambia la forma. La prueba visual de esta adaptación y una build empaquetada
siguen pendientes.

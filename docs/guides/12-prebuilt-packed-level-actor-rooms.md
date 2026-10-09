# Rooms prehechas con Packed Level Actor

Cada Room prehecha se configura en un único Blueprint contenedor. No necesita
mapa runtime, actor Layout, Bake ni Presentation Mode.

## Crear la Room

1. Construye la geometría en un mapa de trabajo y crea su Blueprint de
   `Packed Level Actor`, conservando el pivote en el origen.
2. Crea un Blueprint hijo de `Dungeon Blueprint Forge Packed Room`, por ejemplo
   `BP_DBF_PackedRoom_Castle01`.
3. En Class Defaults asigna el Packed Blueprint en `Packed Level Actor Class`.
   La vista previa aparece dentro del actor solo en editor.
4. Elige `Bounds Mode`: `Automatic` calcula varias cajas desde las mallas;
   `Manual` usa `Main Room Volume` y los componentes Bounds que coloques tú.
   Si eliges Manual para una L o T, usa varias cajas y deja libre el hueco.
5. Configura `Number Of Exits` entre 1 y 4. Mueve las flechas activas al centro
   de cada puerta y orienta su eje `+X` hacia fuera.
6. Si usas más de una salida, asigna un Blueprint de puerta del proyecto host en
   `Unused Exit Door Class`. Orienta su eje `+X` hacia fuera y configura la
   geometría y colisión deseadas.
7. Configura `Allowed Rotations`; compila y guarda el Blueprint contenedor.
   Coloca una instancia en el nivel. Si usas Automatic, pulsa `Rebuild
   Automatic Bounds` en Details y comprueba las cajas azules.
8. Ejecuta `Validate Room And Log` en esa instancia y comprueba Output Log.

### Centrado automático de salidas

En `Class Defaults → Dungeon Blueprint Forge → Packed Room → Exits`, activa `Auto Center Exits` si usas `Bounds Mode = Automatic`. No hay botón adicional: al construir la Room en el editor, el plugin examina todas las caras exteriores de los Bounds, busca huecos cerrados en los triángulos de las Static Mesh y asigna un hueco distinto a cada flecha activa. Coloca las flechas en el centro y orienta su `+X` hacia fuera. `Number Of Exits` indica cuántos huecos debe encontrar; la detección no decide por sí sola cuántas puertas tiene la Room. Si encuentra menos huecos que salidas activas, no mueve ninguna flecha y escribe el recuento en Output Log. Guarda el Blueprint después de comprobar las flechas. Las Rooms existentes mantienen el modo manual hasta que actives la opción.

La búsqueda admite portones bastante mayores que el `Opening Size` lógico y,
si varias caras de la geometría presentan contornos del mismo hueco, conserva
la medida de mayor superficie. Tras actualizar el plugin, activa de nuevo
`Auto Center Exits` en una instancia y guarda el Blueprint para sustituir
medidas antiguas de `Detected Exit Opening Sizes`. Revisa visualmente que no
haya confundido un vano decorativo con una puerta.

La detección se basa en geometría, no en etiquetas semánticas de puerta. Si no encuentra un hueco fiable, deja la flecha donde estaba y escribe un aviso en Output Log. Una ventana, abertura decorativa, muro sin triángulos legibles o geometría muy abierta puede impedir la detección; en esos casos desactiva `Auto Center Exits` y ajusta la flecha manualmente. Ejecuta `Validate Room And Log` antes de generar. El análisis se realiza solo en editor; las posiciones guardadas se usan en runtime.

La flecha se centra horizontalmente en el hueco y se coloca verticalmente a media altura de `Opening Size` sobre el umbral detectado. Así, una puerta o arco más alto que `Opening Size` no eleva el suelo de la Room procedural o del pasillo conectado. Comprueba el empalme con una seed fija: los dos suelos deben quedar a la misma altura. Si el Packed Actor no contiene una superficie horizontal legible en el umbral, el análisis usa el borde inferior aproximado de la cuadrícula y puede requerir ajuste manual de la altura de la flecha.

Los IDs de las cuatro flechas y de los volúmenes se asignan automáticamente.
No deben editarse manualmente.

La flecha heredada `GameplayAnchor` es independiente de las salidas `Exit`.
Puedes moverla a un lugar navegable de esta Room para que el proyecto host
coloque allí su controlador de enemigos usando `Gameplay Anchor World Transform`
de `Result.Rooms`. Los puntos donde aparecen los enemigos siguen estando en el
Blueprint de ese controlador; revisa que sus posiciones relativas queden dentro
del suelo de esta Packed Room.

La puerta se crea con la orientación de la flecha `Exit` libre. La flecha queda
centrada horizontalmente en la abertura y a media altura del `Opening Size`
lógico sobre el umbral. Si el vano real es más alto, el plugin eleva la puerta
visual sin elevar la Room conectada. Al crearla, desplaza el Actor para centrar el conjunto
de sus Static Mesh visibles en ese punto; esto admite Blueprints cuyo pivote
está separado de la geometría. La puerta debe tener mallas visibles registradas
y dimensiones apropiadas para el hueco. Si no tiene Static Mesh, se conserva
el pivote del Blueprint como referencia.

Para ajustar la altura de todas las puertas libres de una Packed Room concreta,
abre su Blueprint y cambia `Unused Exit Door Height Offset` en **Class Defaults
→ Dungeon Blueprint Forge → Packed Room → Unused Exits**. El valor es en
centímetros: `+20` sube la puerta 20 cm y `-20` la baja 20 cm. El valor inicial
es `0`. Después, regenera con la misma seed y comprueba la puerta de perfil.
Cada Blueprint de Packed Room puede tener un valor diferente.

En el mismo grupo están `Unused Exit Door Forward Offset` y `Unused Exit Door
Right Offset`: positivos mueven la puerta hacia el `+X` y `+Y` de la flecha;
negativos, en sentido contrario. Estos ejes giran con cada salida. `Unused
Exit Door Scale` multiplica los ejes **locales del Actor de puerta** después
del ajuste automático. Según cómo esté orientada la Static Mesh dentro de su
Blueprint, el ancho puede depender de `X` o de `Y`. Con `Fit Unused Exit Door
To Opening` activado (valor inicial), la Room mide cuál de los ejes locales
cambia realmente el ancho y la altura visibles y ajusta el Actor completo.
En modo
Manual, o si no se detectó un hueco, usa `Opening Size` de la flecha. `1,1,1`
conserva el ajuste automático; desactiva `Fit Unused Exit Door To Opening` si
quieres conservar el tamaño original de la puerta. `Detected Exit Opening
Sizes` muestra las medidas guardadas de Exit1..Exit4. La geometría se vuelve a
centrar después de escalarla; si el hueco detectado es más alto que `Opening
Size`, la puerta sube lo necesario para mantener su base junto al umbral.
El detector comprueba la intersección real de cada triángulo del muro con la
cuadrícula del vano. Así evita que el rectángulo envolvente de un triángulo
marque como pared una parte vacía del hueco. El ajuste escala el Actor completo.
Output Log indica el vano objetivo, el tamaño de las mallas antes y después
de escalar y la escala final; compara estas cifras con el hueco visible si
queda una franja abierta.
Si desactivaste `Auto Center Exits` antes de incorporar esta mejora, actívalo
una vez para medir y guardar las aberturas, y después desactívalo de nuevo si
quieres retocar manualmente las flechas.
Compila y guarda el Blueprint de la Packed Room
tras detectar los huecos, y compara la misma seed. Una puerta con silueta
distinta del hueco (por ejemplo, rectangular frente a arco) puede necesitar
un marco o un Actor de puerta apropiado; la escala no cambia su forma.

## Añadirla al generador

1. Crea una `Dungeon Blueprint Forge Room Definition`.
2. Asigna `BP_DBF_PackedRoom_Castle01` en `Room Class`.
3. Para usarla como intermedia, pon `Category = Normal`, `Enabled = true`,
   `Selection Weight > 0` y al menos dos salidas válidas en su Blueprint.
4. Añade la Definition a `Normal Rooms` del Generation Config. `Gameplay Zone`
   solo comunica el tipo de encuentro al proyecto host.
5. Genera con una seed fija, preferiblemente con
   `Generate Dungeon From Seed Staged`. Comprueba conexiones, solapamientos y
   puertas en salidas libres; conserva `Resolved Seed` y `Last Result > Message`
   si falla.

Durante la búsqueda solo existe el contenedor ligero con sus cajas y flechas.
El Packed Level Actor se crea después de aceptar la topología y su clase se
precarga mediante el flujo staged. `Clear Dungeon` destruye también la
presentación Packed.

El generador puede conectar cualquier salida válida; no reserva `Exit1` como
entrada obligatoria. Al finalizar la distribución, la Packed Room coloca una
instancia de `Unused Exit Door Class` en cada salida que haya quedado sin
conexión. La clase se precarga en staged y esas instancias se destruyen con la
Room. Una puerta Blueprint replicada se crea solo en el servidor; una no
replicada se crea localmente en cada cliente. Los Blueprints Packed existentes
con más de una salida necesitan asignar esta clase antes de generar.

Packed Level Actor está pensado principalmente para geometría estática. Puertas
interactivas, enemigos, loot y encuentros permanecen en el proyecto host.
## Volúmenes manuales y automáticos

En el Blueprint contenedor, `Bounds Mode` permite elegir cómo se describe la ocupación de la Room:

- `Manual`: utiliza `Main Room Volume` y cualquier componente `Dungeon Blueprint Forge Bounds` añadido por el diseñador. Es la opción de mayor control.
- `Automatic`: analiza las mallas del `Packed Level Actor`, rellena los interiores cerrados y aproxima formas L, T o curvas mediante varias cajas azules.

El modo automático ofrece `Automatic Bounds Cell Size`, `Automatic Bounds Margin` y `Maximum Automatic Bounds`. Los valores predeterminados para nuevas Packed Rooms son `25 cm`, margen `0 cm` y máximo `16` cajas. Una celda más pequeña sigue mejor las curvas, pero puede necesitar más cajas. En Rooms grandes, el tamaño efectivo de celda puede aumentar para cubrir toda la geometría dentro de la cuadrícula.

`Automatic Bounds XY Inset` (por defecto `0 cm`) contrae cada caja automática en X e Y sin cambiar su altura. Si el borde azul sobresale de la pared, prueba primero `5 cm`, reconstruye los bounds en una instancia y aumenta poco a poco. El ajuste se aplica a cada caja, por lo que valores grandes pueden abrir separaciones entre cajas vecinas o dejar paredes sin cubrir. `Automatic Bounds Margin` sigue expandiendo las cajas; déjalo en `0 cm` al corregir un borde que sobresale. Ejecuta `Validate Room And Log` después de cada ajuste y comprueba también los solapamientos con una seed fija.

Los Blueprints ya creados pueden conservar valores anteriores. Compruébalos en `Class Defaults` y ejecuta `Rebuild Automatic Bounds` en una instancia colocada después de cambiarlos.

Si cambias el Packed Level Actor o no ves las cajas al cambiar el modo, ejecuta `Rebuild Automatic Bounds` en el Details Panel de una instancia del actor. El botón copia las cajas calculadas a los valores predeterminados del Blueprint de la Packed Room. Comprueba el resultado, guarda ese Blueprint y vuelve a abrirlo antes de generar. Si cambias `Cell Size`, `Margin`, `XY Inset` o el máximo, vuelve a pulsar el botón para actualizar la copia guardada.

En el editor, los volúmenes automáticos se recalculan al construir el actor y forman parte del descriptor lógico usado por el planificador. En una build empaquetada se conservan las cajas precisas guardadas con el Blueprint; si no hay cajas guardadas, se recurre a bounds de malla menos precisos. Antes de distribuir, compila y guarda el Blueprint, vuelve a abrirlo y comprueba sus cajas; prueba también una build empaquetada. Estos volúmenes no generan colisión física ni se recalculan cada frame.

La selección entre volúmenes Manual y Automatic se basa en los componentes nativos, no en el texto editable de `Bounds Id`. Si `Validate Room And Log` informa que no existe ningún Bounds pese a ver cajas azules, conserva el Blueprint y el Output Log para diagnosticar si las cajas automáticas llegaron con un tamaño válido a la instancia generada.

### Error: «La habitación necesita al menos un componente Dungeon Blueprint Forge Bounds»

Este mensaje puede aparecer al generar una **segunda Packed Room** aunque la
primera funcione: cada Blueprint de Room necesita sus propios Bounds guardados.
El número de salidas no causa este error. Revisa en este orden:

1. Abre la `Room Definition` nombrada en el mensaje y comprueba que `Room Class`
   apunta al **Blueprint contenedor correcto** de esta Room, no al Packed Level
   Actor ni a otro Blueprint usado en una prueba anterior.
2. En ese Blueprint, confirma `Packed Level Actor Class` y elige `Bounds Mode =
   Automatic` si quieres calcular las cajas. Compila y guarda.
3. Coloca una instancia temporal del **mismo Blueprint contenedor** en un nivel.
   En sus Details pulsa `Rebuild Automatic Bounds` y confirma que aparecen cajas
   azules con tamaño real. El botón copia los resultados a los valores
   predeterminados del Blueprint; guárdalo de nuevo.
4. Ejecuta `Validate Room And Log` en esa instancia y revisa Output Log. Genera
   otra vez con la misma seed.

Si las cajas son visibles en la instancia pero el generador sigue dando el
mismo error, conserva capturas de `Room Class`, los componentes del Blueprint,
el resultado de validación y `Resolved Seed`: puede haber una diferencia entre
la instancia visible y los Bounds guardados que usa la candidata.

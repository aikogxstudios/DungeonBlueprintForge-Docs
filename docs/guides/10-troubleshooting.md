# Diagnóstico y errores frecuentes

| Síntoma | Primera comprobación |
|---|---|
| El plugin no aparece | Debe estar en `TuProyecto/Plugins/DungeonBlueprintForge/`, con `.uplugin` y `Source/`, y compilado para la versión de Unreal de ese proyecto. La copia 5.8 está en validación. |
| No se genera nada | Revisa `Configuration`, `Last Result > Message` y `Resolved Seed`. |
| Falta Start, Key o Boss | La lista necesita una Definition válida, habilitada y con categoría correcta. |
| No hay pasillos | Asigna un `Corridor Style` válido con meshes. |
| No Compatible Placement | Revisa Bounds, conexiones, categorías, pesos, rotaciones y `Maximum Placement Attempts`. Conserva la seed. |
| La Stairwell no aparece | Debe estar en `Normal Rooms`, con `Category = Normal`; sube temporalmente su peso para probarla. |
| Una Packed Room no aparece como intermedia | Revisa `Category = Normal`, `Enabled`, `Selection Weight > 0`, la lista `Normal Rooms` y al menos dos salidas válidas. Conserva la seed y consulta la [guía Packed](12-prebuilt-packed-level-actor-rooms.md). |
| «La habitación necesita al menos un componente Dungeon Blueprint Forge Bounds» en otra Packed Room | Comprueba que la `Room Class` del Data Asset apunta a su Blueprint contenedor. En una instancia de ese Blueprint, ejecuta `Rebuild Automatic Bounds`, confirma cajas azules, guarda el Blueprint y prueba `Validate Room And Log`. Cada Room necesita sus propios Bounds guardados. Consulta la [guía Packed](12-prebuilt-packed-level-actor-rooms.md). |
| Una puerta de salida libre queda desplazada | Centra la flecha en el hueco y ajusta `Unused Exit Door Height/Forward/Right Offset` en Class Defaults del Blueprint Packed. Regenera con la misma seed. |
| Una puerta Packed deja una franja superior o lateral | Comprueba `Detected Exit Opening Sizes`, activa `Auto Center Exits`, reconstruye y guarda el Blueprint. El cierre conserva el tamaño de su Blueprint: solo se centra. Si su silueta o tamaño no cubre el vano, utiliza un actor de cierre adecuado en tu proyecto; Opening Size no lo escala. |
| `CookAll` de la copia 5.8 falla con `CR_Mannequin_Procedural` o `HeroAnin` | Revisa esos assets del proyecto host. `HeroAnin` informa que le falta Skeleton. Consulta el [estado de UE 5.8](../development/ue58-validation-2026-10-09.md); una compilación C++ correcta no resuelve errores de assets. |
| Las cajas automáticas forman un rectángulo demasiado grande | Revisa `Bounds Mode = Automatic`, los valores de celda/margen y las mallas Packed; reconstruye en una instancia, comprueba las cajas azules y compila/guarda el Blueprint contenedor. |
| Adaptive Floors no encuentra Stairwell | Añade una Definition válida a `Stairwell Room Definitions`, activa el debug de huella y comprueba conexiones libres, Bounds, rotaciones y dirección. |
| La generación parece congelarse | Puede estar probando hasta `Maximum Generation Attempts` layouts. Conserva la seed, revisa `Last Result` y compara primero con menos Rooms o una huella mayor. |
| Un suelo o techo atraviesa otra planta | Los bounds actuales incluyen su grosor real. Guarda seed, configuración y captura: sería una regresión que debe reproducirse. |
| Una Room invade verticalmente un pasillo | La auditoría actual compara el ancho y la altura completos del corredor. Guarda la seed y una vista lateral si vuelve a suceder. |
| La auditoría final rechaza un pasillo | Es una protección: encontró un cruce o una entrada lateral a una Room y descartó ese layout. No la desactives. |
| Hub o Key se quedan sin salida | Deja `Maximum Local Backtrack Steps = 2` para que el planificador pueda retirar hasta dos normales recientes, liberar sus conexiones y probar otra combinación. |
| Quiero saber cuándo reintenta | Activa `Print Generation Retry Debug`: cian indica un nuevo layout y muestra los milisegundos del anterior; amarillo indica backtracking local. |
| Hay marcos duplicados en una puerta | Marca Already Has Door Frame en el Exit cuyo arte ya incluye marco; comprueba Direct Contact y regenera. |
| La salida alta no conecta | No modifiques conectores automáticos; prueba una seed fija y confirma la Room siguiente. |
| El cofre no sale | Revisa clase host, estilo asignado, bounds, puertas, pilares y `Door Clearance`. |
| Un cliente intenta generar | Genera solo en servidor o GameMode. |
| La IA no camina | Activa navegación donde corresponda y comprueba NavMesh real. |

La misma seed solo repite si conservas configuración, listas, pesos, clases y
Room Count. Para cambios de geometría, genera primero un caso pequeño, repite
con dos seeds nuevas y finalmente prueba rendimiento en juego.

La caja verde de `Draw Adaptive Floor Footprint` solo muestra el alcance XY;
no bloquea, no genera navegación y no sustituye una prueba manual de colisión
ni de recorrido entre plantas.

Se dibuja una sola vez después del resultado final. Con `Duration = 0` queda
visible como línea debug persistente. En un fallo, `Last Result` conserva el
modo, la huella y las transiciones resueltas para facilitar el diagnóstico.

Los pasillos candidatos desactivan colisión, navegación y luces durante la
búsqueda. Solo los pasillos del layout aceptado reconstruyen la presentación
completa. Compara siempre la misma seed con backtracking 0 y 2 antes de atribuir
una pausa al retroceso.

En el backtracking, los pasillos de Hub, Key y normales restauradas se prueban
como rutas lógicas sin Actor ni HISM temporal. Tras aceptar el layout se crean
una sola vez y se valida su bound real contra la huella.

Para generación durante gameplay consulta [Generación staged y rendimiento](11-staged-generation-and-performance.md).

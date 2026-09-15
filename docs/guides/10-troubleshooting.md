# Diagnóstico y errores frecuentes

| Síntoma | Primera comprobación |
|---|---|
| El plugin no aparece | Debe estar en `TuProyecto/Plugins/DungeonBlueprintForge/`, con `.uplugin` y `Source/`, y compilado para UE 5.4. |
| No se genera nada | Revisa `Configuration`, `Last Result > Message` y `Resolved Seed`. |
| Falta Start, Key o Boss | La lista necesita una Definition válida, habilitada y con categoría correcta. |
| No hay pasillos | Asigna un `Corridor Style` válido con meshes. |
| No Compatible Placement | Revisa Bounds, conexiones, categorías, pesos, rotaciones y `Maximum Placement Attempts`. Conserva la seed. |
| La Stairwell no aparece | Debe estar en `Normal Rooms`, con `Category = Normal`; sube temporalmente su peso para probarla. |
| La salida alta no conecta | No modifiques conectores automáticos; prueba una seed fija y confirma la Room siguiente. |
| El cofre no sale | Revisa clase host, estilo asignado, bounds, puertas, pilares y `Door Clearance`. |
| Un cliente intenta generar | Genera solo en servidor o GameMode. |
| La IA no camina | Activa navegación donde corresponda y comprueba NavMesh real. |

La misma seed solo repite si conservas configuración, listas, pesos, clases y
Room Count. Para cambios de geometría, genera primero un caso pequeño, repite
con dos seeds nuevas y finalmente prueba rendimiento en juego.

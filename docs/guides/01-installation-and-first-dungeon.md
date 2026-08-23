# Instalación y primera mazmorra

## 1. Instalar

1. Cierra Unreal Editor.
2. Copia la carpeta completa `DungeonBlueprintForge` en `TuProyecto/Plugins/DungeonBlueprintForge/`.
3. Comprueba que dentro existan `DungeonBlueprintForge.uplugin` y `Source/`.
4. Abre el `.uproject` con Unreal Engine **5.4** y acepta compilar si Unreal lo solicita.
5. Ve a `Edit > Plugins`, busca **Dungeon Blueprint Forge**, actívalo y reinicia si Unreal lo pide.

El plugin aparecerá en Plugins sin subir nada a Epic ni a Fab. Para distribuir una versión privada usa una carpeta empaquetada del plugin, no una copia de `Intermediate` o del proyecto de pruebas.

## 2. Los cuatro Assets mínimos

Necesitas crear estos Assets en el Content Browser de tu juego:

1. Un Blueprint de sala `Start`.
2. Un Blueprint de sala `Normal`.
3. Un Blueprint de sala `Key` y otro `Boss` (pueden compartir geometría al principio, pero son Definitions distintas).
4. Un `Dungeon Blueprint Forge Generation Config` que los referencia.

Para un primer test puedes usar salas prehechas. Añade a cada una un Bounds y dos o más Connection Components; la flecha `+X` de cada conexión apunta hacia fuera de la sala. Después crea una `Dungeon Blueprint Forge Room Definition` por categoría y rellena las listas del Generation Config.

Si usarás pasillos, crea también un `Dungeon Blueprint Forge Corridor Style`, asigna las mallas de suelo/pared y actívalo en el Generation Config.

## 3. Colocar el generador

1. Arrastra al mapa un Actor de clase `DungeonBlueprintForgeGenerator` o un Blueprint hijo suyo.
2. En Details, asigna tu `Generation Config` a **Configuration**.
3. Para una prueba desde editor, asigna **Preview Seed** y pulsa **Generate Preview**.
4. Para juego, sigue la [guía Blueprint](02-blueprint-implementation.md).

Si falla, consulta el campo **Last Result > Message** o el Output Log. La seed resuelta se conserva: úsala con `Generate Dungeon From Seed` para repetir el caso exacto.

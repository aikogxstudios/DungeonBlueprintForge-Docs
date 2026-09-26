# Instalación y primera mazmorra

Esta guía requiere una copia autorizada del plugin. El repositorio público de
documentación contiene guías e imágenes, pero no incluye el plugin instalable.
Si todavía no tienes esa copia, puedes leer el flujo, pero no realizar los
pasos en Unreal.

## 1. Instalar

1. Cierra Unreal Editor.
2. Copia la carpeta del plugin privado en `TuProyecto/Plugins/DungeonBlueprintForge/`.
3. Comprueba que contiene `DungeonBlueprintForge.uplugin` y los archivos de
   compilación correspondientes a tu entrega (`Source/` o `Binaries/`).
4. Abre el `.uproject` con Unreal Engine **5.4** y acepta compilar si Unreal lo solicita.
5. Ve a `Edit > Plugins`, busca **Dungeon Blueprint Forge**, actívalo y reinicia si Unreal lo pide.

El plugin aparecerá en Plugins sin subir nada a Epic ni a Fab. Para distribuir una versión privada usa una carpeta empaquetada del plugin, no una copia de `Intermediate` o del proyecto de pruebas.

## 2. Los cuatro Assets mínimos

Necesitas crear estos Assets en el Content Browser de tu juego:

1. Un Blueprint de sala `Start`.
2. Un Blueprint de sala `Normal`.
3. Un Blueprint de sala `Key` y otro `Boss` (pueden compartir geometría al principio, pero son Definitions distintas).
4. Un `Dungeon Blueprint Forge Generation Config` que los referencia.

Para un primer test puedes usar salas prehechas. Añade a cada una al menos un
Bounds. `Start` y `Boss` necesitan una salida válida; una `Normal` que deba
conectar dos Rooms necesita al menos dos. La flecha `+X` de cada Connection
Component apunta hacia fuera. Crea una `Dungeon Blueprint Forge Room
Definition` por categoría, habilítala y añádela a la lista correspondiente
del Generation Config. Sigue los pasos de [creación de salas](03-authoring-rooms.md)
y consulta la [referencia de Data Assets](../reference/data-assets.md) si no
encuentras un campo.

Si usarás pasillos, crea también un `Dungeon Blueprint Forge Corridor Style`, asigna las mallas de suelo/pared y actívalo en el Generation Config.

## 3. Colocar el generador

1. Arrastra al mapa un Actor de clase `DungeonBlueprintForgeGenerator` o un Blueprint hijo suyo.
2. En Details, asigna tu `Generation Config` a **Configuration**.
3. Para una prueba desde editor, asigna **Preview Seed** y pulsa **Generate Preview**.
4. Para juego, sigue la [guía Blueprint](02-blueprint-implementation.md).

Si falla, consulta el campo **Last Result > Message** o el Output Log. La seed resuelta se conserva: úsala con `Generate Dungeon From Seed` para repetir el caso exacto.

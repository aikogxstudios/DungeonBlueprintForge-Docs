# Estado de la migración a Unreal Engine 5.8 — 2026-10-09

La rama privada `agent/ue58-migration` del plugin se preparó para Unreal Engine
5.8.3 con el proyecto de pruebas `DungeonLab54` copiado a una carpeta separada.
El nombre interno del módulo sigue siendo `DungeonLab54` para conservar sus
referencias. Esta documentación pública no incluye el plugin ni el proyecto.

## Comprobado

- Compilan los targets Editor y Game Development del proyecto de pruebas.
- `BuildPlugin` Win64 terminó para Editor, Game Development y Game Shipping.
- El cook de paquetes referenciados terminó con 658 paquetes, sin errores ni avisos.
- El descriptor del plugin declara `EngineVersion = 5.8.0`; `VersionName` sigue
  en `0.10.1` hasta completar la aceptación funcional.

## Pendiente y problemas conocidos

- `CookAll` del laboratorio falló por `CR_Mannequin_Procedural` (RigVM no resuelve
  una propiedad de memoria) y `HeroAnin` (Animation Blueprint sin Skeleton).
  Son assets del proyecto de pruebas; no se ha demostrado que fallen en 5.4.
- Algunas texturas UDIM de `Stylised_Dungeon_Pack` avisan de que Virtual
  Texturing está desactivado y solo se cocinará el primer bloque.
- Falta abrir la copia 5.8 en el editor, compilar sus Blueprints y comprobar
  generación, Packed Rooms, puertas, iluminación, colisión y navegación.
- La puerta de salida libre mide las Static Mesh visibles y escala el Actor
  completo. El encaje visual de todos los huecos y siluetas sigue sin aceptar.
- Falta una build empaquetada completa y pruebas en hardware objetivo.

La compilación del plugin no equivale a validar todos los assets ni a declarar
una versión lista para distribución. La guía de [Rooms Packed](../guides/12-prebuilt-packed-level-actor-rooms.md)
explica cómo recalcular los huecos y probar una seed fija.

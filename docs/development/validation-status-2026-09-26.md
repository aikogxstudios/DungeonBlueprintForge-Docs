# Estado de validación — 2026-09-26

La rama privada de desarrollo mantiene el descriptor `0.10.1` para Unreal
Engine 5.4. Los targets `DungeonLab54Editor` y `DungeonLab54` Win64 Development
compilaron de nuevo el 2026-09-27 después de la última corrección C++.

El usuario confirmó visualmente en el laboratorio que una Packed Room se
genera y que `Automatic Bounds` puede seguir una forma irregular con varias
cajas. También observó mejor variedad de Rooms normales tras la preferencia
por una clase distinta de la anterior.

La revisión de código también corrigió los índices de Rooms opcionales: un
intento de colocación fallido o un retroceso local ya no deja huecos en
`LastResult.Rooms`.

La prueba manual posterior encontró un defecto reproducible: una puerta host
hecha con una sola Static Mesh deja una franja abierta sobre un vano Packed,
aun con `Auto Center Exits` y `Fit Unused Exit Door To Opening` activos. La
revisión incremental del 2026-09-27 atribuye el tamaño insuficiente al
rasterizado aproximado de triángulos del muro; la corrección geométrica y la
prueba visual siguen pendientes. No se debe considerar resuelto con un build.

La siguiente prueba manual también cubrirá Rooms Packed nuevas: dos o más
salidas como Room intermedia, puertas host en salidas libres, repetición
con seed fija y marcos con `Direct Contact` staged. Siguen pendientes una build
empaquetada en un proyecto limpio, NavMesh, tránsito, late join y mediciones
en hardware objetivo. Una compilación Development no verifica esos puntos.

Consulta la [guía Packed](../guides/12-prebuilt-packed-level-actor-rooms.md)
y la [lista de liberación](release-checklist.md) antes de llevar el plugin a
un proyecto de producción.

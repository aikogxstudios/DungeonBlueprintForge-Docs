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
rasterizado aproximado de triángulos del muro. Se sustituyó por una prueba de
intersección triángulo/celda y se retiraron pasadas redundantes; Editor y Game
Development compilan. **La prueba visual con la misma Room y seed sigue
pendiente**, por lo que no se considera resuelto todavía.

El Output Log permitió identificar otro fallo en el ajuste: una puerta de
`400 x 400 cm` destinada a un vano de `170 x 236 cm` quedaba en `400 x 236 cm`.
La escala cambiaba la altura, pero actuaba sobre el eje equivocado para el
ancho. El código ahora mide qué ejes locales del Actor controlan realmente
ancho y alto. Esta segunda corrección también requiere una prueba visual antes
de considerarse aceptada.

La siguiente prueba manual también cubrirá Rooms Packed nuevas: dos o más
salidas como Room intermedia, puertas host en salidas libres, repetición
con seed fija y marcos con `Direct Contact` staged. Siguen pendientes una build
empaquetada en un proyecto limpio, NavMesh, tránsito, late join y mediciones
en hardware objetivo. Una compilación Development no verifica esos puntos.

La copia privada de UE 5.8.3 tiene un [estado de validación separado](ue58-validation-2026-10-09.md):
compila el código y pasa el cook de paquetes referenciados, pero `CookAll`
encuentra errores en dos assets del laboratorio. No sustituye las pruebas
visuales y jugables pendientes de las puertas Packed.

Consulta la [guía Packed](../guides/12-prebuilt-packed-level-actor-rooms.md)
y la [lista de liberación](release-checklist.md) antes de llevar el plugin a
un proyecto de producción.

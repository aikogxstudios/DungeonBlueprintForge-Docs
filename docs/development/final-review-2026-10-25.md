# Revisión final y respaldo — 2026-10-25

Plugin **0.10.1**, versión interna **12**, Unreal Engine **5.8.3**.
Esta revisión prepara una copia de seguridad y una primera prueba controlada
en otro proyecto; no declara una versión de producción certificada.

## Correcciones y rendimiento

Se conservan la alineación por umbral, la detección del borde real del suelo
Packed, el preview seguro para undo y la actualización de flechas al ejecutar
`Rebuild Automatic Bounds` con `Auto Center Exits` activo. El usuario confirmó
los empalmes de la base del laboratorio después de reconstruir y guardar.

La caché de AABB por colocación mantiene el cálculo anterior e invalida al
cambiar el transform. Se conservan los arreglos de repetición y sustitución
Staged, limpieza del generador y selección de pesos finitos. Las 12 firmas
comparadas el 18 de octubre no cambiaron; no se repitió ese benchmark hoy.
Los tiempos de editor NullRHI no deben presentarse como FPS ni como garantía
para todas las seeds. La seed 42 con 75 salas todavía necesita perfilado.

En el repaso final se eliminaron dos parámetros sin uso del chequeo de
penetración del pasillo. Además, si desaparecía `Configuration` durante las
primeras fases de precarga, los callbacks salían sin finalizar la petición.
Ahora las tres fases producen un fallo terminal `InvalidConfiguration`,
conservan la seed y liberan callbacks y actividad pendiente. Se comprueba
también el estado después del evento inicial de progreso.

## Validación del 25 de octubre

- Editor Win64 Development: compilación y enlace correctos.
- Game Win64 Development: compilación y enlace correctos.
- BuildPlugin Win64 aislado: Editor, Game Development y Game Shipping correctos.
  Paquete generado con código, binarios y documentación; no incluye arte del host.
- Automation: **15 pruebas superadas, 0 fallos, 0 avisos en las pruebas**.
- Nueva prueba: pérdida de configuración en las tres fases de precarga.
- El test de determinismo conserva comparaciones normal/normal/Staged para
  2, 10 y 30 salas, incluyendo generaciones consecutivas.
- La validación usa NullRHI y fixtures nativos: no acredita apariencia visual.

El arranque del laboratorio conserva avisos de exposición Python por nombres
coincidentes entre flags y componentes `AutoConnection`. No se han renombrado
propiedades serializadas o Blueprint para silenciarlos. No son fallos de las
15 pruebas, pero tampoco se afirma que todo el log esté libre de avisos.

## Límites y siguiente prueba

La planificación sigue siendo síncrona; Staged distribuye presentación.
Quedan la limpieza estructural de los archivos grandes y la duplicación de
rutas de colocación. No se hace una refactorización amplia junto con el respaldo.
No se certifica ausencia de todos los fallos ni reentrancia arbitraria desde
eventos Blueprint. Evita cambiar Config o lanzar otra generación desde progreso.

Antes de adoptar en producción: instalar en una copia aislada del host UE 5.8.3,
probar una seed fija con pocas salas, recorrer conexiones y colisiones, repetir
Staged dos veces, sustituir/cancelar y borrar el generador. Después comprobar
NavMesh, red si procede y una build empaquetada del juego. Los fallos históricos
de `CookAll` en `CR_Mannequin_Procedural` y `HeroAnin` son del contenido del
laboratorio y siguen pendientes. No se ha modificado el juego real.

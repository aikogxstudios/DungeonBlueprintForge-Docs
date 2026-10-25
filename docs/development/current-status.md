# Versión y estado actual — 2026-10-25

- **Dungeon Blueprint Forge 0.10.1**, versión interna **12**.
- Motor de trabajo y validación: **Unreal Engine 5.8.3**.
- El descriptor declara `EngineVersion: 5.8.0`; no es la versión del plugin.
- Base funcional de empalmes confirmada por el usuario en el laboratorio.
- Editor y Game Development correctos; **15 pruebas automáticas superadas**.
- Últimas optimizaciones pendientes de aceptación visual y jugable en el host.

Consulta la [revisión final del 25 de octubre](final-review-2026-10-25.md) para
correcciones, alcance de las pruebas, avisos conocidos y primera integración.
El respaldo de mantenimiento no cambia el descriptor ni equivale a una versión
de producción. El código y los paquetes se conservan en el repositorio privado;
la documentación pública contiene únicamente guías y resultados.

## Empalmes y uso

Después de cambiar el arte Packed, ejecuta `Rebuild Automatic Bounds` con
`Auto Center Exits` activo, revisa cajas/flechas, compila y guarda el Blueprint.
La detección considera el umbral y el borde real del suelo. Consulta la
[guía Packed](../guides/12-prebuilt-packed-level-actor-rooms.md) y el
[diagnóstico](../guides/10-troubleshooting.md).

## Pendientes

Recorrido visual y colisiones, NavMesh, red/late join y empaquetado del host;
perfilado de casos grandes y refactorización conservadora. Las pruebas técnicas
no sustituyen estas comprobaciones. Los resultados anteriores conservan su fecha
y no deben interpretarse como una validación actual de producción.

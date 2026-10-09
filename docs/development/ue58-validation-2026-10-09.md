# Estado de UE 5.8 — 2026-10-09

El plugin activo utiliza **Unreal Engine 5.8.3**. El descriptor declara
EngineVersion 5.8.0 y el plugin conserva VersionName 0.10.1. La versión 5.4
corresponde a checkpoints históricos, no a las instrucciones actuales.

## Evidencia anterior a la revisión de opciones

- Editor y Game Development compilaron en UE 5.8.
- BuildPlugin Win64 pasó para Editor, Game Development y Game Shipping.
- Cook de paquetes referenciados: 658 paquetes, cero errores y avisos.
- El usuario confirmó el funcionamiento en Unreal y confirmó el cierre Packed
  sin autoescalado y la exclusión de marcos ya presentes en las salidas.

## Revisión actual

Se agrupan opciones y conexiones, se añaden ayudas en español y se corrigen
validación de medidas no finitas, seed negativa en replicación, diagnósticos
de cancelación y medidas Packed obsoletas. Consulta [la revisión actual](maintenance-review-2026-10-09.md)
para builds, pruebas automáticas y comprobaciones visuales pendientes.

## Límite de las afirmaciones

El CookAll anterior procesó 2594 paquetes y falló en CR_Mannequin_Procedural
(RigVM) y HeroAnin (Animation Blueprint sin Skeleton). No se repitió ni se
modificaron esos assets en esta revisión. Una build empaquetada completa,
NavMesh, late join y rendimiento en hardware objetivo requieren pruebas propias.

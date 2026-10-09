# Revisión de mantenimiento — 2026-10-09, UE 5.8.3

## Alcance

Se inventarió el módulo Runtime original (26 archivos C++/Build.cs), sus
propiedades reflejadas, descriptor y configuración. Se revisaron las rutas de
planificación, presentación síncrona/Staged, replicación, limpieza, conexiones,
Packed Rooms, geometría modular, pasillos, marcos y la documentación privada
y pública. Graphify se utilizó como orientación y se contrastaron los cambios
en Source. Es una revisión estática con builds y pruebas acotadas; no certifica
que todos los casos de gameplay o todos los posibles bugs estén cubiertos.

## Cambios de organización

- 225 opciones editables tienen ayudas en español. El catálogo se genera desde
  esas mismas ayudas para reducir divergencias con la documentación.
- Generation Config se organiza en Rooms, Layout, Adaptive Floors, Corridors,
  Door Frames, Staged Presentation y Debug.
- Pilares, antorchas y luz de relleno utilizan una sola jerarquía de categorías;
  los ajustes de rendimiento permanecen dentro de su función.
- El modo Random de forma permite configurar brazos L/T; antes se mostraban
  inactivos aunque el runtime sí los utilizaba.
- Los campos inactivos por interruptor se muestran deshabilitados: tamaño fijo
  frente a aleatorio, distancias en Direct Contact, techo y navegación sin
  colisión. Default Normal Room Count muestra el límite real de 69.
- El panel de room tiene un grupo por Connection Component. Se retiran de ese
  panel las referencias inline repetidas a componentes internos; los objetos
  originales siguen en Components del editor completo y accesibles a Blueprint.
- En salidas nativas, los valores que genera Number Of Exits o Use Automatic
  Connections se muestran de solo lectura. Already Has Door Frame es editable
  por salida. La edición múltiple conserva el comportamiento normal de Unreal.
- Connector y el papel futuro Special se conservan serializados y ocultos en
  la lista de nuevos roles. Special de Gameplay Zone sigue disponible.
- Stair Rise, Stair Step Count y Stair Step Depth permanecen ocultos por
  compatibilidad. El ajuste actual usa la malla y Stair Repeat Count.

## Bugs corregidos

| Problema confirmado en Source | Corrección | Límite de comprobación |
|---|---|---|
| OnRep descartaba RoomSeed = -1, aunque GetUnsignedInt puede resolver ese int32 | El flag de presentación preparada decide si reconstruir; no se reserva una seed legal como sentinel | Revisión y compilación; falta prueba de red de ese valor exacto |
| Medidas NaN/infinito no se rechazaban por comparaciones con cero | Validación explícita de ancho/alto finitos y de transforms en conexiones/pasillos | Tests de entradas y builds; no genera un caso visual inválido |
| Una detección Packed fallida conservaba la altura de otra geometría | Se limpian las medidas antiguas si no hay fuente o aberturas suficientes; las flechas conservan su autoría | Revisión y build; probar sustitución de arte en editor |
| Cancelar Staged limpiaba LastResult y perdía seed/diagnósticos | Conserva identidad y métricas, elimina referencias a actors destruidos y notifica progreso Cancelled | Revisión y build; comprobar una cancelación en gameplay |

La reserva de marcos ya presentes se comparte mediante una utilidad entre
generación normal y Staged; se usa la misma clave de ubicación para evitar
duplicados de Direct Contact. Con pasillo, el extremo separado conserva su marco.

## Documentación

Guía principal nueva en español, guía procedural reescrita, catálogo completo,
índices reorganizados y diagnóstico actualizado. La instalación actual utiliza
UE 5.8. Las referencias a 5.4 se conservan solo como checkpoints históricos.
Se retiran instrucciones del autoescalado y fondo Packed eliminados por
petición del usuario. La documentación pública recibe guías y referencias;
no recibe código, binarios ni assets del host.

## Validación de esta revisión

- Editor Win64 Development: compilación y enlace correctos, incluidos Runtime
  y el nuevo módulo Editor. Game Win64 Development: correcto.
- Automation DungeonBlueprintForge: **4 tests correctos, 0 fallos**.
  Cubren marcos existentes usados/no usados y extremos separados, medidas
  finitas y compatibilidad Door/Passage, seeds negativas/límites/entrada inválida.
  El cuarto construye un panel de propiedades separado: exactamente cuatro
  filas Already Has Door Frame, una por Exit, y cero referencias inline Exit1–Exit4.
- El informe JSON de Automation y su log permanecen en Saved del laboratorio.
  Se ejecutó con NullRHI: no es una prueba visual del panel ni del layout.
- BuildPlugin Win64 de la versión final: correcto para Editor, Game Development
  y Game Shipping; los targets Game no incluyen el módulo Editor.
- Enlaces Markdown: 71 documentos privados y 22 públicos, cero enlaces locales rotos.
- 352 declaraciones UPROPERTY existentes conservan sus nombres y valores iniciales.
- git diff --check pasa; no se han editado assets del proyecto host.

## Comprobación manual del usuario

1. Abrir Room_3 en UE 5.8: confirmar que Connections muestra una sola lista de
   Exit1–Exit4 y que los controles administrados no se pueden editar ahí.
2. Confirmar que las opciones originales, mallas y tamaños de sus Blueprints
   conservan sus valores; probar una seed conocida.
3. Marcar Already Has Door Frame en una salida artística, compilar y guardar;
   comprobar contacto directo y los dos extremos de un pasillo.
4. Cambiar un Packed Actor con Auto Center Exits activo por arte sin aberturas
   detectables: comprobar que no se utilizan medidas guardadas anteriores.
5. Cancelar una generación Staged y comprobar Phase, Failure Reason y seed.

La confirmación visual anterior del usuario corresponde al cierre Packed sin
escalado y a la exclusión de marcos previa a esta reorganización. El nuevo
panel aún necesita comprobación visual. CookAll, late join, NavMesh, Lumen,
rendimiento y empaquetado completo del host conservan sus pruebas propias.

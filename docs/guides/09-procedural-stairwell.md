# DBF Stairwell Room: subir una planta

`DBF Stairwell Room` es el actor especializado para conectar una zona baja con
otra superior. Genera paredes, techo, descansillo bajo, escalones de
una escalera completa de `Stair Module` y descansillo alto.

El actor crea automáticamente sus dos conexiones `Passage`: una baja y otra
alta. No añadas `Connection Components`, no edites sus transforms y no actives
las cuatro direcciones normales. El generador ya alinea transformaciones
completas, por lo que la conexión alta coloca la siguiente Room en la altura
correcta.

## Crear la Room

1. Crea un Blueprint hijo de `DBF Stairwell Room`, por ejemplo
   `BP_DBF_Stairwell`.
2. No modifiques `Room Shape` ni las conexiones. El actor trae una configuración
   inicial transitable:
   - `Room Size = (600, 1600, 700)`
   - descansillo bajo = `200 cm`
3. En **02 Surface Modules**, asigna cuatro módulos de tu pack:
   - `Floor Module`: un tile de suelo plano. Solo construye los descansillos
     bajo y alto; no pongas aquí una malla de escalera.
   - `Stair Module`: una malla de escalera completa, por ejemplo
     `SM_Moss_Stairs`. El plugin la trata como una escalera completa: la coloca
     una sola vez, mide su inicio y fin reales y adapta la Room a ese tramo.
     Nunca estira ni repite una escalera completa.
   - `Wall Module` y `Ceiling Module`: los módulos habituales de la sala.
4. Dentro de `Stair Module`, deja `Rotation Offset` en `(0, 0, 0)`. No uses
   `Auto Orient Mesh` para girar una escalera: la orientación la resuelve
   `Stair Mesh Direction`.
5. Deja **01 Room Layout > Stairwell > Stair Mesh Direction** en
   `Auto From Mesh Geometry`. El plugin lee los vértices bajos y altos del
   mesh y alinea la subida hacia la puerta alta. No escribas ángulos. Las otras
   direcciones son solo un respaldo para un asset cuya pendiente no pueda leerse.
6. `Stair Repeat Count` decide cuántas copias completas habrá en la subida:
   - `1`: una escalera.
   - `2`: la segunda comienza justo donde termina la primera.
   - `3`: tres escaleras rectas encadenadas, y así sucesivamente.
   No hay giros, columnas laterales ni descansillos entre copias. El plugin usa
   el inicio y final reales del mesh, por lo que conserva los peldaños sin
   escalarlo. La Room aumenta automáticamente su largo y altura.
   Para cambiar el **ancho**, elige un mesh completo más ancho.
7. `Lower Landing Depth` es el suelo plano después de la puerta baja y antes
   del primer tramo de escaleras. Déjalo en `200 cm` para la configuración
   inicial, o ajústalo al tamaño de un tile de tu `Floor Module`.
8. El actor mantiene pilares, decoraciones y antorchas desactivados por defecto
   para no invadir los peldaños.

El resultado esperado es: puerta baja → descansillo plano → una o más escaleras
rectas unidas por sus extremos → descansillo alto → puerta alta.

## Cofre y antorchas en la Stairwell

- Si el `Room Definition` tiene un estilo de cofre asignado, la Stairwell puede
  crear **un solo cofre** en el suelo del `Lower Landing`.
- `Allow Chest On Lower Landing` lo activa o desactiva. Solo se intenta cuando
  `Lower Landing Depth` alcanza `Minimum Lower Landing Depth For Chest`
  (por defecto `500 cm`). Aun así, los bounds reales del cofre, la puerta y el
  inicio de los escalones deben caber; si no caben, no se genera.
- Para las antorchas, activa `Enable Procedural Torches`, asigna tu `Torch Actor
  Class` y deja `Maximum Torches Per Room` en `2`. La Stairwell las coloca en
  las dos paredes largas, a media subida y a la altura del peldaño de esa zona.
  Nunca usa las puertas, los descansillos ni los escalones como punto de anclaje.

Al generarse, aparecerán internamente `Auto_Stair_Lower` y
`Auto_Stair_Upper`. Son los conectores de entrada y salida; no requieren
configuración del usuario.

## Integración en la mazmorra

Crea un `Room Definition` que apunte a `BP_DBF_Stairwell`, usa `Category =
Normal` y añádelo a `Normal Rooms`. La Stairwell participa como una candidata
normal: al entrar por su puerta baja, la puerta alta queda libre y la siguiente
Room o corredor horizontal continúa en esa planta. No hay una ruta vertical
forzada ni un segundo generador.

Usa `Selection Weight` para su frecuencia. Si tus Rooms normales tienen peso
`1`, empieza con `0.20` para Stairwell: puede aparecer sin transformar todas
las ramas en una subida. Sube ese peso cuando quieras más plantas. Haz primero
una generación corta con una seed fija y comprueba que el pasillo o Room de la
salida superior queda en la planta alta sin solapar la inferior.

Los corredores siguen siendo rectos y horizontales; la transición vertical la
realiza exclusivamente la Stairwell. Las comprobaciones distinguen corredores
de distintas alturas, de modo que una planta superior puede reutilizar X/Y de
la inferior solo cuando los volúmenes reales no se cruzan.

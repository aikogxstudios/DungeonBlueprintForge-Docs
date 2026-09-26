# DBF Stairwell Room: subir una planta

`DBF Stairwell Room` conecta una zona baja con otra superior.
Genera paredes, techo, descansillo bajo, escalera y descansillo alto.
Sus dos conexiones `Passage` son automáticas.
No añadas Connection Components ni edites sus transforms.

## Crear la Room

1. Crea un Blueprint hijo de `DBF Stairwell Room`.
2. En `Floor Module`, usa un tile plano para los descansillos.
3. En `Stair Module`, asigna una malla de escalera completa.
4. Asigna paredes y techo habituales.
5. Deja `Rotation Offset = (0, 0, 0)`.
6. Deja `Stair Mesh Direction = Auto From Mesh Geometry`.
7. Usa `Stair Repeat Count` para aumentar altura.

Cada copia comienza donde termina la anterior.
El mesh no se estira y no se insertan giros.
Resultado: puerta baja -> descansillo -> escalera(s) -> descansillo -> puerta alta.

Para más ancho, elige una malla completa más ancha.
Escalar una malla estrecha suele deformar peldaños y colisión.

## Integrarla en la generación

Crea una `Room Definition` que apunte al Blueprint.
Usa `Category = Normal` y añádela a `Normal Rooms`.
Si las normales tienen peso `1`, empieza con `Selection Weight = 0.20`.
La puerta alta queda libre para continuar en la planta superior.

La Stairwell puede generar un cofre solo en `Lower Landing` con espacio real.
Las antorchas opcionales se colocan en paredes largas, nunca en escalones ni puertas.
Prueba una seed fija con 20 normales antes de mapas de 75 Rooms.

## Plantas adaptativas

El `Generation Config` tiene tres modos de expansión:

- `Free Expansion`: conserva la generación horizontal sin huella XY.
- `Adaptive Floors`: limita cada planta a `Default Footprint Size` y puede usar
  una Stairwell reservada para continuar en otra altura.
- `Automatic`: cambia a Adaptive Floors cuando `Normal Room Count` supera
  `Automatic Adaptive Floor Threshold` (40 por defecto).

Añade la Definition de la Stairwell a `Stairwell Room Definitions`, mantén
`Category = Normal` y configura `Adaptive Floor Direction` como `Either` para
la primera prueba. Cuando una normal ya no puede continuar, el runtime intenta
una conexión libre compatible de una Room `Start`, `Normal` o `Hub` ya colocada.
La altura continúa dependiendo de `Stair Repeat Count` y del resto de ajustes
del actor Stairwell.

Para ver el alcance horizontal, activa `Draw Adaptive Floor Footprint`. La caja
verde centrada en el Generator es únicamente debug: no bloquea al jugador ni
crea NavMesh. Repite una seed fija y comprueba desde arriba y desde un lateral
que Rooms y pasillos permanecen dentro de la huella.

## Generación compacta y reintentos

Cuando una normal o Hub obligatorio no cabe en el extremo actual, Adaptive
Floors puede conservarlo y probar otras conexiones libres de Start, Normal o
Hub. La Key sigue prefiriendo una rama lateral de Hub, pero puede usar una Room
normal si no existe un Hub válido.

Los bounds de una Room modular incluyen el grosor real de suelo y techo. Esto
evita aceptar dos plantas cuyas superficies se atraviesen aunque sus volúmenes
interiores parezcan separados. Si el problema reaparece, guarda la seed y una
captura del actor afectado.

El perímetro verde se dibuja una sola vez al terminar la solicitud, aunque haya
reintentos. `Duration = 0` lo mantiene visible hasta limpiar las líneas debug.

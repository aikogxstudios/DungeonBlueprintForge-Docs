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

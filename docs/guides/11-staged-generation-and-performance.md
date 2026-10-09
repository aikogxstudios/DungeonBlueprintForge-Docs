# Generación staged y rendimiento

La generación staged mantiene la misma topología determinista, pero distribuye
la construcción visual entre frames. Es la opción recomendada durante gameplay
cuando una mazmorra grande no debe producir una pausa larga.

## Nodos Blueprint

| Nodo | Uso |
|---|---|
| `Generate Dungeon From Seed Staged` | Partida reproducible usando el Room Count predeterminado del Config. |
| `Generate Dungeon Staged` | Request completo: seed, seed aleatoria y Normal Room Count. |
| `Cancel Staged Generation` | Cancela una solicitud staged activa. |
| `Clear Dungeon` | Cancela también la precarga pendiente y limpia el resultado actual. |

Para una partida aleatoria, crea un
`DungeonBlueprintForgeGenerationRequest`, activa `Use Random Seed` y pásalo a
`Generate Dungeon Staged`. El wrapper separado `Generate Random Dungeon Staged`
ya no forma parte de la API para evitar dos caminos equivalentes.

La planificación sigue ejecutándose de forma síncrona. El presupuesto por
frame se aplica a la presentación, y un elemento pesado puede superarlo.
Cancelar conserva la seed y los diagnósticos; no conserva actors destruidos.

## Eventos

Enlaza los eventos antes de llamar al nodo:

- `On Start Area Ready`: Start y su primera conexión jugable ya tienen
  estructura. Puede utilizarse para retirar una pantalla de carga inicial.
- `On Generation Progress`: entrega progreso 0--1 y la fase actual.
- `On Generation Finished`: único resultado final. Comprueba `Success` antes de
  registrar encuentros, objetivos o lógica del proyecto host.

Las fases actuales separan precarga, planificación, Start, Rooms conectadas,
pasillos restantes, marcos, contenido diferido y gameplay. Una Room pesada no
comparte frame con otra operación pesada.

## Ajustes iniciales

| Campo | Valor inicial | Efecto |
|---|---:|---|
| `Preload Staged Generation Assets` | Activado | Precarga referencias blandas antes de presentar Rooms. |
| `Staged Generation Time Budget` | `6 ms` | Presupuesto aproximado por frame. |
| `Maximum Staged Items Per Frame` | `4` | Límite adicional de operaciones por frame. |

Un presupuesto menor suaviza cada frame, pero aumenta el tiempo total. Un
presupuesto mayor termina antes, pero puede crear un pico visible en hardware
modesto. Mide antes de cambiar los valores predeterminados.

## Métricas

Después de `On Generation Finished`, abre `Performance Stats` dentro del
resultado. Incluye tiempos de preload, planning, presentación, Rooms,
pasillos, marcos, contenido diferido, gameplay y `Slowest Staged Item`.

Compara siempre:

1. La misma seed.
2. El mismo Room Count y Data Assets.
3. Una ejecución de calentamiento antes de medir.
4. `stat unit` para Game/Draw/GPU y, si procede, Unreal Insights.

El tiempo total staged incluye frames de espera. Para localizar un tirón suele
ser más útil `Slowest Staged Item` junto con los tiempos por fase.

## Validación actual

El proyecto de laboratorio completó una barrida visual satisfactoria de 5 a 70
Rooms el 2026-09-19 en el laboratorio anterior de UE 5.4. También se reforzó la auditoría tridimensional para que una
Room de otra planta no invada el volumen vertical de un pasillo.
El cleanup correspondiente completó UHT, compilación C++ y enlace de
`DungeonLab54Editor Win64 Development` sin errores.

Esta validación no sustituye pruebas específicas de NavMesh, Lumen, late join,
cofres replicados ni hardware de gama baja. Conserva la seed y una captura si
encuentras una regresión.

## Packed Level Actor Rooms

Las Rooms prehechas utilizan un Blueprint hijo de
`Dungeon Blueprint Forge Packed Room`. Ese contenedor mantiene cajas y flechas
ligeras; runtime precarga su clase Packed y la crea dentro del presupuesto
staged. Consulta
[Rooms prehechas con Packed Level Actor](12-prebuilt-packed-level-actor-rooms.md).

# Generación multijugador

Dungeon Blueprint Forge es **server-authoritative**: el servidor elige la seed,
calcula la distribución y genera la mazmorra. Los clientes reconstruyen la
presentación replicada a partir de datos compactos.

## Uso desde Blueprint

1. Llama a la generación desde `GameMode`, un evento `Run on Server` o una ruta
   que se ejecute únicamente en el servidor.
2. No generes desde un cliente: el resultado devuelve `Not Server Authority`.
3. Prueba primero como `Listen Server` con dos jugadores antes de cambiar
   estilos, clases o assets.

| Elemento | Comportamiento |
|---|---|
| Generator | El servidor es la autoridad de la distribución. |
| Rooms y pasillos | Replican información determinista para reconstruir HISM y luces locales. |
| Marcos | Replican estilo y transform. |
| Cofres | Los crea el servidor como Actors replicados. |
| Enemigos | Son responsabilidad del juego host y deben crearse en servidor. |

Un Blueprint de cofre debe activar `Replicates`, guardar `Is Open` y el loot
como variables replicadas o RepNotify, y ejecutar la interacción con `Run on
Server`. Prueba late join y persistencia de cofres en tu proyecto: una
compilación no demuestra esos casos.

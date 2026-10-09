# Generación multijugador

Dungeon Blueprint Forge usa una red **server-authoritative**: solo el servidor
puede elegir la seed, calcular la distribución y crear la mazmorra. Los clientes
reciben el resultado y reconstruyen localmente las instancias HISM, luces y
antorchas a partir de los datos compactos replicados.

## Uso correcto desde Blueprint

1. Coloca `DungeonBlueprintForgeGenerator` en el mapa como siempre.
2. Llama a `Generate Dungeon`, `Generate Random Dungeon` o `Generate Dungeon
   From Seed` desde `GameMode`, una función marcada como `Run on Server`, o
   cualquier Blueprint que solo se ejecute en el servidor.
3. No llames al generador desde el cliente. El nodo devuelve
   `Not Server Authority` y no genera una copia local.
4. Prueba primero con `Number of Players = 2`, modo `Play As Listen Server`.
   La prueba inicial de salas, puertas, pasillos y marcos ya fue correcta; repite
   la comprobación cuando cambies estilos o assets.

## Qué se sincroniza

| Elemento | Estrategia |
|---|---|
| Generator | El servidor es la única autoridad para la distribución. |
| Rooms modulares | Actor replicado con seed y conexiones usadas; cada cliente reconstruye su HISM local. |
| Pasillos | Actor replicado con estilo, conexiones y tamaño; cada cliente reconstruye sus HISM y luces locales. |
| Marcos | Actor replicado con su estilo y transform de conexión. |
| Antorchas y fill lights | Presentación local determinista. No se envían Actors duplicados por red. |
| Cofres | Los crea el servidor como Actors replicados. |

## Preparar tu Blueprint de cofre

El plugin crea y replica el Actor, pero el contenido pertenece al juego host.
En el Blueprint de cofre:

1. Activa `Replicates`.
2. Mantén el estado persistente (`Is Open`, loot restante, bloqueo) como
   variables `Replicated` o `RepNotify`.
3. La interacción del jugador debe ser un evento `Run on Server`; el servidor
   valida distancia/estado y cambia el cofre.
4. Usa multicast solo para efectos breves como sonido o partículas; no para
   guardar el estado de abierto.

## Qué no hace todavía

- No incluye spawner ni IA de enemigos. El futuro spawner del proyecto host
  debe decidir y crear enemigos solo en el servidor.
- El marco es estático. Una futura puerta interactiva necesitará su propio
  estado replicado (`Open`, `Locked`, `Key Id`) y una interacción de servidor.
- La base de dos jugadores ya fue comprobada. Para cerrar esta fase faltan la
  prueba de late join y la validación del estado persistente del cofre host.

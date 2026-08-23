# Implementación Blueprint paso a paso

Esta es la integración mínima recomendada. El generador vive en el mapa; un Actor controlador o el Level Blueprint guarda su referencia y decide cuándo llamarlo. No generes cada frame ni desde un Widget sin una referencia válida.

## Paso 1: crear el controlador

1. Crea `BP_DungeonController` basado en `Actor` y colócalo en el mismo mapa que `DungeonBlueprintForgeGenerator`.
2. Añade una variable **Generator** de tipo `DungeonBlueprintForgeGenerator` (Object Reference). Márcala editable o asígnala desde el nivel.
3. Añade `Use Manual Seed` (Boolean) y `Manual Seed` (Integer) si quieres alternar entre una seed repetible y una aleatoria.

## Paso 2: enlazar antes de generar

En `Event BeginPlay`:

1. Usa `Is Valid (Generator)`. Si falla, imprime un error y no continúes.
2. Desde `Generator`, usa **Bind Event to On Generation Finished**.
3. Crea el Custom Event `Handle Generation Finished`, con el parámetro `Result` que Unreal crea al enlazarlo.
4. Solo después del Bind llama a la generación. Así nunca pierdes el resultado de una generación rápida.

![Flujo de BeginPlay, Bind y resultado.](../images/blueprint-session-test/Captura%20de%20pantalla%202026-08-09%20002925.png)

La captura muestra exactamente ese orden: `BeginPlay` valida la referencia, enlaza `On Generation Finished` y un Branch decide entre `Generate Dungeon From Seed` (manual) y `Generate Random Dungeon` (aleatoria). El evento de resultado usa `Break DungeonBlueprintForgeGenerationResult`: si `Success` es verdadero imprime la `Resolved Seed` y `Generated Room Count`; si es falso, imprime `Message` para diagnosticarlo.

## Paso 3: escoger el nodo correcto

| Necesidad | Nodo | Ajustes |
|---|---|---|
| Nueva partida aleatoria | `Generate Random Dungeon` | Usa `Default Normal Room Count` del Data Asset. |
| Repetir un bug o compartir mapa | `Generate Dungeon From Seed` | Introduce la seed exacta. |
| Cambiar seed y cantidad desde UI | `Generate Dungeon` | Construye `DungeonBlueprintForgeGenerationRequest`. |
| Repetir la última generación válida | `Regenerate Last Dungeon` | No modifica la seed. |

![Dos atajos y el Request completo.](../images/blueprint-session-test/Captura%20de%20pantalla%202026-08-09%20012607.png)

En la segunda captura, el bloque superior es el nodo avanzado `Generate Dungeon`: el struct Request acepta **Seed**, **Use Random Seed** y **Normal Room Count**. El bloque derecho es el atajo para seed manual; el inferior usa la configuración predeterminada y una seed aleatoria.

## Paso 4: responder al resultado

En `Handle Generation Finished`:

1. Haz `Break DungeonBlueprintForgeGenerationResult`.
2. Si **Success** es falso, muestra **Message** y guarda **Resolved Seed**.
3. Si es verdadero, usa **Generated Room Count** para UI y guarda **Resolved Seed** en tu SaveGame si deseas repetir el mapa.
4. Solo tras `Success` coloca jugador, enemigos o loot que dependan de markers.

Para buscar un Marker, llama a `Get Generated Marker Transforms` en Generator con un `Marker Type` como `PlayerSpawn`. El plugin devuelve transforms; tu juego decide qué Actor aparecer allí.

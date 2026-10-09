# Comprobaciones antes de distribuir — UE 5.8

Esta lista corresponde al plugin actual para UE 5.8.3. Los resultados
fechados de UE 5.4 son históricos. Consulta [la revisión actual](maintenance-review-2026-10-09.md).

## Verificación técnica

- [x] Editor y Game Win64 Development compilan en UE 5.8.3.
- [x] Automation DungeonBlueprintForge: 4 pruebas, sin fallos.
- [x] BuildPlugin final validado para Editor, Game Development y Game Shipping.
- [ ] Confirmar panel reorganizado, opciones existentes y una seed conocida en Unreal.
- [ ] Verificar reemplazo de geometría Packed y detección fallida de sus aberturas.
- [ ] Probar cancelación Staged y diagnóstico de seed.
- [ ] Probar marcos existentes en contacto directo y pasillos.
- [ ] Probar late join y el caso de RoomSeed negativa en red.
- [ ] Verificar NavMesh, Lumen, colisión, tránsito y rendimiento en hardware objetivo.

## Contenido y paquete

- [ ] Resolver los assets del host que fallaron en CookAll y repetir el cook completo.
- [ ] Probar una build empaquetada del host, incluida geometría Packed.
- [ ] Instalar el paquete privado en un proyecto limpio UE 5.8 y seguir la guía.
- [x] Los targets Game de BuildPlugin excluyen el módulo Editor.
- [ ] Revisar redirecciones, EngineVersion y archivos incluidos en el paquete.

## Documentación y GitHub

- [x] Enlaces relativos válidos en documentación privada y pública.
- [x] README, instalación y guía principal indican UE 5.8.
- [x] Catálogo coincide con las 225 opciones editables.
- [x] Rama, diff, nombres/valores de propiedades y archivos públicos revisados antes de publicar.

La documentación pública contiene solo guías e imágenes. La compilación no
equivale a aceptación visual ni empaquetado completo de los assets del juego.

# Nova client GUI for Fabric 1.21.11

Click GUI, HUD overlay and module system, matching the mockup. The modules only toggle on and off here. Hook your own behaviour into `Module.onEnable`, `onDisable` and `onTick`.

## Mappings

Written for **Mojang mappings**, which Fabric recommends for 1.21.11 (Yarn is frozen after this version). If your project uses Yarn, rename:

| Mojang | Yarn |
| --- | --- |
| `GuiGraphics` | `DrawContext` |
| `drawString(font, text, x, y, color, shadow)` | `drawText(textRenderer, text, x, y, color, shadow)` |
| `font` / `font.lineHeight` | `textRenderer` / `textRenderer.fontHeight` |
| `Component` | `Text` |
| `KeyMapping` | `KeyBinding` |
| `Minecraft` | `MinecraftClient` |
| `MouseButtonEvent` | `Click` |
| `KeyEvent` | `KeyInput` |
| `consumeClick()` | `wasPressed()` |

## Setup

1. Copy `src/client/java/com/nova` into your mod's client source set (`src/client/java`). If you use a single source set, put it under `src/main/java` instead.
2. Register the entrypoint in `fabric.mod.json`:

```json
"entrypoints": {
  "client": ["com.nova.NovaClient"]
}
```

3. Add a lang entry so the keybind shows a proper name, in `assets/nova/lang/en_us.json`:

```json
{
  "key.nova.open_gui": "Open Nova GUI",
  "key.category.nova.nova": "Nova"
}
```

4. Dependencies in `fabric.mod.json`: `"fabric-api": "*"` and `"minecraft": "~1.21.11"`.

## Notes

- Colours are ARGB with a full alpha byte (`0xFF...`). Since 1.21.6, colours without alpha can render invisible.
- Input events changed in 1.21.9: mouse and key handlers take `MouseButtonEvent` and `KeyEvent` objects rather than raw ints.
- Part most likely to need a small tweak if the compiler complains: the HUD file (`HudElementRegistry`, `mc.getFps()`, ping lookup), since those names moved around between 1.21.6 and 1.21.11.
- `ClickGuiScreen` overrides `renderBackground` with a plain dim. That avoids the vanilla blur and the "can only blur once per frame" error.

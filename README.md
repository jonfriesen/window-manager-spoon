# WindowManager

WindowManager is a [Hammerspoon](https://www.hammerspoon.org/) Spoon that moves and resizes the focused macOS window with keyboard shortcuts. It acts on the screen that contains the window.

## Install and configure

1. Put the entire `WindowManager.spoon` folder in the `Spoons/` directory under your Hammerspoon configuration (usually `~/.hammerspoon/Spoons/`).
2. Allow Hammerspoon under **System Settings → Privacy & Security → Accessibility** so it can move and resize other apps’ windows.
3. Add these bindings to your Hammerspoon `init.lua`, then reload the configuration:

```lua
local mods = { "ctrl", "alt", "cmd" }

hs.loadSpoon("WindowManager"):bindHotkeys({
    quarterTopLeft     = { mods, "u" },
    quarterTopRight    = { mods, "i" },
    quarterBottomLeft  = { mods, "j" },
    quarterBottomRight = { mods, "k" },
    cycleLeft          = { mods, "left" },
    cycleRight         = { mods, "right" },
    cycleTop           = { mods, "up" },
    cycleBottom        = { mods, "down" },
})
```

Only mapped actions get shortcuts. The Spoon logs a warning for each omitted action.

## Actions

| Mapping key | What it does |
| --- | --- |
| `quarterTopLeft` | Places the window in the top-left quarter. |
| `quarterTopRight` | Places the window in the top-right quarter. |
| `quarterBottomLeft` | Places the window in the bottom-left quarter. |
| `quarterBottomRight` | Places the window in the bottom-right quarter. |
| `cycleLeft` | Anchors the window to the left edge at full height, cycling its width. |
| `cycleRight` | Anchors the window to the right edge at full height, cycling its width. |
| `cycleTop` | On portrait screens, anchors to the top edge at full width and cycles the height. Otherwise, centers the window horizontally at full height and alternates between 75% and 100% width. |
| `cycleBottom` | On portrait screens, anchors to the bottom edge at full width and cycles the height. Otherwise, requests a centered 1024 × 768 point window. |

Left and right share a single width cycle: **1/2 → 2/3 → 1/3**, then repeat. On portrait screens, top and bottom share a separate height cycle in the same order. Switching sides advances the shared cycle rather than starting it over. A screen counts as portrait only when its usable frame is taller than it is wide; square screens follow the landscape behavior.

The percentages use the screen’s usable frame, excluding space reserved for the menu bar and Dock. A 100% window fills that frame without entering native macOS fullscreen. A shortcut only moves a window if one is focused. Apps that restrict window movement or enforce minimum or fixed sizes may not match the requested position or size.

## Build the install ZIP

From the repository root, run:

```sh
git archive --format=zip --prefix=WindowManager.spoon/ \
  -o ../WindowManager.spoon.zip HEAD init.lua README.md LICENSE
```

## License

Copyright 2024 Jon Friesen. Licensed under [Apache-2.0](LICENSE).

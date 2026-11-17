# Where the names come from

Every name in `config/symbols/` is one psx-tomba's Tomba! 1 owners or docs use, at an address our own RE placed:
the port's constants and override bindings (`titles/tomba1/game/`), its RE chain (`docs/re-frontier.md`) and its
state and issues. The map is hand-written; nothing is imported from another decomp's symbol files.

## Ours

Every name not listed below. Psy-Q library entries (`CdSync`, `VSync`, `DrawSync`, `SetGeomOffset`, ...) carry the
SDK's names: the port binds them to psxport's stock library owners by behaviour. Lower-case library internals
(`cdSyncInternal`, `gpuTimeoutBegin`, `cdReadyStatus`, ...) are our names.

## Names first read elsewhere, confirmed here

The address and the behaviour are ours; the name was first read in another project and kept because the port's
docs use it.

| name | address | from | confirmed |
|---|---|---|---|
| `main` | `0x800163B0` | hansbonini/psx_tomba (no licence, names only) | crt0 calls it; port `native_boot.cpp` |
| `dispatchTasks` | `0x80017024` | psx_tomba | Ghidra 2026-11-17: walks `taskTable` through `currentTask` |
| `gameplayStateDispatcher` | `0x8001AC00` | psx_tomba | re-frontier T1-07a |
| `areaTransitionHandler` | `0x8001CFCC` | psx_tomba | re-frontier T1-07a |
| `stopBgm` | `0x80020C00` | psx_tomba | re-frontier T1-07a; port `dev_warp.h` calls it |
| `ObjCullRegister` | `0x80022E44` | afsenovilla/Tombi-Decomp (MIT) | Ghidra 2026-11-17, body audited |
| `cullObjectToDrawList` | `0x80022C08` | Tombi-Decomp | Ghidra 2026-11-17, body audited |
| `cullObjectToDrawListMargin` | `0x80022D3C` | Tombi-Decomp | Ghidra 2026-11-17, body audited |
| `despawnObjectOffscreen` | `0x80023020` | Tombi-Decomp | Ghidra 2026-11-17, body audited |
| `isObjectInView` | `0x80022F6C` | Tombi-Decomp | Ghidra 2026-11-17 C read; the body audit did not pass |
| `isOnScreen` | `0x80022FD8` | Tombi-Decomp | the same |
| `despawnObjectOffscreenMargin` | `0x800230BC` | Tombi-Decomp | the same |
| `decreaseObjectTimer` | `0x800505B0` | Tombi-Decomp | the same; port `dev_life_loss.h` calls it |
| `playerHandleHazardContact` | `0x800FE970` (`x_shared`) | Tombi-Decomp | function entry with 7 in-image callers; port `dev_life_loss.h` |

## Not placed

Addresses the port's docs mention without a function entry or image to put them in:

| address | what | why not in the map |
|---|---|---|
| `0x80053DA0`, `0x80054618` | weapon collision against the category lists (Tombi-Decomp's `collideWeaponsWithCat2/4/5Objects`) | both are function entries; which category each walks is not read |
| `0x8011E4E4` | area 3's quad drawer | not a function entry in X03 |
| `0x80123940`, `0x80123B80` | area 5's quad drawers | past the end of X05 and INFO2; the image is not known |
| `0x80116308` | the area-12 branch of the 2D kind 4 | a label inside an X04 function |
| `0x1F800000`, `0x1F800060`..`0x1F80008C` | scratchpad temporaries (GTE results, anchors, cursors) | each function reuses them for its own values |
| `0x800A539A` | the painted-room drawer select bit (bit 1) | a field of `player` (+2) |

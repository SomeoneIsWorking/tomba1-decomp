# Tomba! 1 decomp

A decompilation of **Tomba!** (USA, `SCUS-94236`), set up with [splat](https://github.com/ethteck/splat). It holds
the build-matched split configuration for the boot executable and every code overlay on the disc, a symbol map from
our own reverse engineering, and the RE notes. No C is matched yet.

The PC port [psx-tomba](https://github.com/SomeoneIsWorking/psx-tomba) consumes this repository as a submodule and
reads its Tomba! 1 guest addresses from the symbol map.

## Bring your own disc

No game bytes are ever committed here: not the executable, not overlays, not the split assembly or data. Copy the
files from your own disc into `disc/` (gitignored), keeping the disc's layout:

```
disc/SCUS_942.36
disc/SYSTEM/X00.BIN .. X19.BIN  DSPSUB.BIN  DSPSUB2.BIN  DSPSUB3.BIN  GOVER.BIN  INFO.BIN  INFO2.BIN  INFO3.BIN  OPTSUB00.BIN
```

Each config carries the file's SHA-1 and splat refuses a mismatch. The images are listed in `docs/overlays.md`.

## Split

```
uv run --frozen splat split config/scus_942.36.yaml
uv run --frozen splat split config/overlays/x00.yaml     # one config per overlay
```

Output goes to `asm/<image>/` and `build/`, both gitignored. splat is pinned in `pyproject.toml`/`uv.lock`.

## Layout

| path | what |
|---|---|
| `config/scus_942.36.yaml`, `config/overlays/*.yaml` | splat configs, one per image |
| `config/symbols/<image>.txt` | the symbol map, splat `symbol_addrs` format; overlay configs also read `scus_942.36.txt`, the X area overlays `x_shared.txt` |
| `docs/overlays.md` | the image set, load address and segment boundaries |
| `docs/names.md` | where each name in the map comes from |
| `docs/re-frontier.md` | the ordered RE evidence chain (`re_frontier.py`) |
| `docs/prior-art.md` | other Tomba! 1 work, licences and what we take |

Notes cite psx-tomba paths (`titles/tomba1/game/...`, `titles/tomba1/docs/issues/...`) where a port owner or an
issue holds the evidence.

## Licence

MIT (`LICENSE`), covering this repository's own files only. Tomba! is © Whoopee Camp / Sony Computer
Entertainment; nothing of it is distributed here.

Built with AI assistance (Claude and other LLMs).

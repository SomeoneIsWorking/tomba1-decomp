# Images and overlays

The disc boots `SCUS_942.36`. Every other code image is a raw blob (no header) loaded at `0x800E7388`, the one code
slot: the load-list walker `appendLoadList` (`0x80021D70`) places a type-0xF0 entry there, and resident code calls
into it (`jal 0x800E7D5C` from `0x80019A7C` and `0x80019A94`). The base is checked per image by its own `jal`s:
at `0x800E7388` the in-image targets land on `addiu sp, sp, -N` prologues (X00 542 of 911, INFO 42 of 51, OPTSUB00
53 of 55); at `0x800E7488` almost none do (X00 4, INFO 0, OPTSUB00 0).

The disc holds each code image once under `SYSTEM/` and again in the `AREAnn/` folders that load it; the copies
are byte-identical (checked for AREA00/X00, AREA02/INFO, AREA04/X04, AREA05/INFO2, AREA07/X01, AREA19/X02). The
configs read `disc/SYSTEM/`.

## Executable

| image | load | gp | rodata | code | data from | SHA-1 |
|---|---|---|---|---|---|---|
| `SCUS_942.36` | `0x80010000` | `0x80097FA8` | `0x80010000` | `0x800163B0` (`main`) | `0x80076E38` | `81cbc79f0230aeb4252e058039f47ac95a777f5a` |

Code ends with the `jr ra` at `0x80076E30`; the next `jr ra` word is at `0x800905D4`, inside data. BSS is
`[0x8009AFB0, 0x800A3348)`, the heap starts at `0x800A3348`.

## X area overlays

`X00`..`X19` (no X07, X12 or X15: AREA07 carries X01, AREA12 X04, AREA19 X02 beside X19, and there is no AREA15). All carry the same first `0x2DE6C`
bytes, an entry table at `0x0` and the shared code at `0x1E60..0x2CA10` (`0x800E91E8..0x80113D98`, names in
`config/symbols/x_shared.txt`); the area part starts at `0x2DE6C` (`0x801151F4`) with its own table, code from
`0x2DE8C`. Code ends are the last `jr ra` plus its delay slot.

| image | size | area code end | SHA-1 |
|---|---|---|---|
| X00 | `0x53F38` | `0x503D8` | `2ba0d06824c940ab40c4c4031f3e3b8e9ead1b89` |
| X01 | `0x57F94` | `0x54164` | `b00c9552178738abacc73f94f17d65fa6e34250f` |
| X02 | `0x37D0C` | `0x34678` | `47013fa07760f5ee1d9348e090ea47bd7a9632ce` |
| X03 | `0x52A9C` | `0x4CF2C` | `919e0a0e29a2811edaa448f38b191e87047083aa` |
| X04 | `0x4DA90` | `0x4834C` | `84cf422f1e5b1fae0d4e2696547eb7fd2eb0633a` |
| X05 | `0x3276C` | `0x2F68C` | `8cc9a13885752f3da1264643abc791d6f8acce02` |
| X06 | `0x3B15C` | `0x36F60` | `869f84d42bd4af486ffac0f65a36e29260b66ac2` |
| X08 | `0x305E0` | `0x2DF54` | `dece9a61218eb0704f2d1cf5fe427cd4b4bbb8a7` |
| X09 | `0x49238` | `0x42774` | `0081fbea9136c4864e7f0e4f4121e5ec73b9236f` |
| X10 | `0x4AE78` | `0x46F8C` | `f22918863d400ed5ecf589a13a665086d7064522` |
| X11 | `0x346D4` | `0x31B84` | `60680209f06fb6e364737b05cb96b55764607c11` |
| X13 | `0x32D9C` | `0x300C0` | `6809e49b358fde84f35ccb99fef58dcec92824ea` |
| X14 | `0x427C8` | `0x3D5A0` | `b65ccce92f2d2ad0a033cf49da53ea7f89989e4c` |
| X16 | `0x33758` | `0x30C1C` | `9a8100ef44b36e4db16dabd2996228989bbbbb89` |
| X17 | `0x346CC` | `0x317D4` | `6955722682ac5664091a2ecb864e1fd645fbbd1e` |
| X18 | `0x34D80` | `0x32468` | `de2078cc6d271dbbb94460f6896b570613306fab` |
| X19 | `0x2E6D4` | `0x2DFCC` | `959eb400c62af4b5b0e1bc7424662587d40d6dad` |

## SYSTEM overlays

Each starts with a small id word (DSPSUB 0x31, DSPSUB2 0x32, DSPSUB3 0x33, OPTSUB00 0x34, GOVER 0x35, INFO 0x36,
INFO2 0x37, INFO3 0x38; the X images 0x1D) and a table, then code. INFO holds area 2's painted rooms and INFO2
area 5's (psx-tomba `titles/tomba1/game/core/state/overlay_images.h`).

| image | size | code | SHA-1 |
|---|---|---|---|
| DSPSUB | `0x28F9C` | `0x794..0x962C` | `bac75e42796b308f34ee7279ae5ffc8884c04bee` |
| DSPSUB2 | `0xEF58` | `0x20C..0x4E0C` | `631440078f95b983bcff158554b2e072df41a7e9` |
| DSPSUB3 | `0x18AF8` | `0x6EC..0x6820` | `8e5bc61c47b1e09a88c420ae0109535b13f0b05f` |
| GOVER | `0xCAC` | `0x1C..0xC6C` | `0dc13a1ef2dfca024f623b7ff3fc33a2d60912b9` |
| INFO | `0xD3D4` | `0x2D0..0x6730` | `e51dd1e335c95a7dc4cf1632ec0e9f647d3c8610` |
| INFO2 | `0x215E0` | `0xC08..0x16848` | `9e19f8b912ab0fd1ea4050e8a2c51e6b8e533c5b` |
| INFO3 | `0x9898` | `0x454..0x6C84` | `c0073331fc635e077734b19cfc78b99d4e330e3b` |
| OPTSUB00 | `0x300C` | `0x38..0x1FC0` | `50c4fc9ccc467e0d67c14eb8706d67c4f10fcd4c` |

Code start is the lowest in-image `j`/`jal` target, code end the last `jr ra` plus its delay slot. `SYS/LDAR*.BIN`
and `SYS/LDSYS.BIN` are not code and have no config.

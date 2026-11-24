# Title screen

## GAM files

A `.GAM` file is a byte LZ: `"GAM\0"`, the unpacked length (u32), then groups of a 16-bit flag word (little-endian)
and up to 16 items, bit 0 first. A clear bit is one literal byte; a set bit is two bytes, a distance and a length,
copying `length` bytes from `distance` bytes back in the output.

## The picture

`OPTSUB00.BIN` (loaded at `0x800E7388`) draws the title through `FUN_800E73C0(list, dx, dy, 0)`, which walks 16-byte
sprite records `{x, y: s16; u, v: u8; w, h, tpage, clut x, clut y: u16}` up to an x of -1. Its caller at
`0x800E7E64` draws the logo list `0x800E9E14` at dy = -[`0x800EA370`] (the slide-in), then the background list
`0x800E9E44`.

- Background: `/SYSTEM/A00010.GAM` unpacks to 256 x 256 halfwords at VRAM (0, 0): an 8-bit 320 x 240 picture through
  the palette `/SYSTEM/A00011.000` (VRAM (128, 511)). Its list draws it as 256 x 256 from tpage `0x80` and 64 x 256
  at x 256 from tpage `0x82`.
- Logo: `/SYSTEM/A00012.000` is 128 x 256 halfwords at VRAM (256, 0) (tpage `0x84`, 8-bit), palette
  `/SYSTEM/A00013.000` (VRAM (128, 510)). The list holds two raw sprites: 256 x 112 from uv (0, 0) at (4, 24), and
  72 x 88 from uv (8, 112) at (252, 48). Colour 0 is transparent.

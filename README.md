# luna-data

Streamed elevation tiles for the Luna app's deep zoom, served as static
files by GitHub Pages at `https://llanger137.github.io/luna-data/dem/`.

## Elevation (`dem/`)

Source: LRO LOLA GDR `LDEM_128` v3 (128 px/deg, ~237 m, 0.5 m per DN),
[PDS Geosciences Node](https://pds-geosciences.wustl.edu/missions/lro/lola.htm).
NASA data, public domain. Cut by `tools/make_dem_tiles.py` in the app.

- `dem/{L}/{row}/{col}.ldt`, L = 1, 2, 3 at 32, 64, 128 px/deg.
- Tiles are 360×360 px. Column 0 starts at 180° W, row 0 at 90° N.
- Poleward rows are decimated east–west by a power of two
  (≤ 1/cos of the row's equatorward edge): fewer, wider tiles, so ground
  spacing stays roughly square. `dem/index.json` lists each row's factor.
- `.ldt`: `'LDT1'`, u16 width, u16 height, u8 q, u8 0, then a zlib stream
  of two byte planes (low, then high) of zigzagged residuals. Heights are
  DN // q (q = 2, whole metres), predicted as left + up − upleft.
  Height in metres = value × q × 0.5 above the 1737.4 km sphere.

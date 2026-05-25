# Map Modding

Map work is high risk. Prefer history, area, trade node, or decision changes unless the feature truly needs new province geometry.

## Key Files

```txt
map/default.map
map/definition.csv
map/provinces.bmp
map/terrain.bmp
map/trees.bmp
map/rivers.bmp
map/heightmap.bmp
map/world_normal.bmp
map/area.txt
map/region.txt
map/superregion.txt
map/continent.txt
map/provincegroup.txt
map/positions.txt
map/terrain.txt
map/climate.txt
map/adjacencies.csv
map/trade_winds.txt
history/provinces/<id> - <name>.txt
common/tradenodes/*.txt
localisation/*_l_english.yml
```

## default.map

`default.map` declares map size, province limit, sea starts, lakes, random-only provinces, canal definitions, and filenames:

```txt
width = 5632
height = 2048
max_provinces = 4942

definitions = "definition.csv"
provinces = "provinces.bmp"
positions = "positions.txt"
terrain = "terrain.bmp"
rivers = "rivers.bmp"
terrain_definition = "terrain.txt"
heightmap = "heightmap.bmp"
```

`max_provinces` must be higher than the highest active province ID and must not exceed engine limits. The wiki notes crashes when it is not higher than the last ID or exceeds 32768.

## definition.csv

Maps province ID to RGB:

```csv
province;red;green;blue;x;x
1;128;34;64;Stockholm;x
```

Each province needs a unique RGB triplet and matching pixels in `provinces.bmp`.

## Bitmap Rules

From the wiki:

- `provinces.bmp`: province shapes, works with `definition.csv`, RGB mode, 24-bit BMP.
- No fully black `(0,0,0)` or fully white `(255,255,255)` pixels in `provinces.bmp`.
- `terrain.bmp`: indexed 8-bit BMP, palette order must remain compatible with `terrain.txt`.
- `trees.bmp`: indexed 8-bit BMP; at least a small amount of one tree type is required for load.
- `rivers.bmp`: indexed 8-bit BMP; rivers are one pixel thick and at least one river is required for load.
- River colours have special meanings for source, flow-in, water, and merging behavior.

Use an editor that preserves palette/indexed mode where required.

## Adding A Province

Checklist:

1. Pick unused province ID.
2. Pick unique RGB.
3. Add row to `definition.csv`.
4. Paint province in `provinces.bmp`.
5. Increase `max_provinces` if needed.
6. Create `history/provinces/<id> - Name.txt`.
7. Add province to `area.txt`.
8. Ensure area belongs to region, region to superregion, and continent if needed.
9. Add terrain/climate/positions/trade node data.
10. Add localisation.
11. Launch with `-debug` and inspect map load errors.

Touch-triggered cross-checks:

- If you change `definition.csv` or `provinces.bmp`, recheck `default.map`, province history, `area.txt`, `positions.txt`, and trade node membership.
- If you change `area.txt`, recheck `region.txt`, `superregion.txt`, missions/decisions/events that reference area keys, and colonial/trade company region logic.
- If you change `region.txt` or `superregion.txt`, recheck missions, decisions, AI logic, institutions, and any scripted triggers using those keys.
- If you change `terrain.bmp` or `terrain.txt`, recheck positions, climate assumptions, movement/combat expectations, and province visuals.
- If you change `adjacencies.csv`, recheck straits, canal logic, pathing, and whether both province IDs exist and remain sea/land compatible.
- If you change `common/tradenodes/*.txt`, recheck province assignment, outgoing edges, merchants, and scripted references to node keys.

Do not treat map files as isolated edits. A valid bitmap change can still break gameplay if area/region/trade/history references are stale.

## Grouping Chain

```txt
province -> area -> region -> superregion -> continent
```

Missions, decisions, events, trade, institutions, colonial regions, and AI often reference area/region keys. Moving provinces can have broad effects.

## Common Crash Sources

- Province ID in map text file missing from `definition.csv`.
- `max_provinces` too low.
- Duplicate RGB colour.
- Incorrect BMP mode.
- Missing province history for active land province.
- Invalid adjacency province ID.
- Area/region references to missing provinces.

# Mod Structure

EU4 mods are separate folders that mirror game folders. A mod should contain only the files it adds or changes.

## Launcher Descriptor

The launcher creates or reads descriptor data and a `.mod` file. The important idea is: the `.mod` file tells EU4 where the mod folder is and what metadata to display.

Typical local descriptor:

```txt
name="My EU4 Mod"
path="mod/my_eu4_mod"
supported_version="1.37.*"
tags={
    "Gameplay"
}
```

Workshop descriptors may use archive/remote metadata instead of a simple local `path`.

## Folder Map

| Folder | Purpose |
| --- | --- |
| `common/` | Game databases: countries, ideas, religions, modifiers, governments, scripted triggers/effects, on_actions, units, trade, estates |
| `events/` | `country_event` and `province_event` files |
| `decisions/` | Country decision files wrapped in `country_decisions = {}` |
| `missions/` | Mission tree series and mission definitions |
| `history/` | Start-date and dated setup for countries, provinces, diplomacy, wars, advisors |
| `localisation/` | Visible strings in `*_l_<language>.yml` files |
| `localisation/replace/` | Replacement localisation keys that intentionally override existing keys |
| `customizable_localization/` | Dynamic text selectors built from `defined_text` blocks |
| `map/` | Province geometry, IDs, areas, regions, terrain, climate, positions, adjacencies |
| `interface/` | `.gui` layouts and `.gfx` sprite declarations |
| `gfx/` | Image assets: flags, event pictures, UI art, models, map assets |
| `music/` | Music files and song declarations |
| `sound/` | Sound assets and registries |

## Additive vs Override

Prefer additive files:

- New `events/mymod_events.txt`
- New `decisions/mymod_decisions.txt`
- New `missions/mymod_missions.txt`
- New `common/scripted_effects/mymod_effects.txt`
- New `localisation/mymod_l_english.yml`
- New `customizable_localization/mymod_dynamic_text.txt`

Use overrides intentionally:

- Reusing an existing decision/mission/modifier/religion key changes that object.
- Replacing localisation goes in `localisation/replace/`.
- Copying a whole vanilla file into a mod increases patch compatibility cost.

## File Ordering

EU4 commonly loads all files in a folder, but conflict behavior depends on the object type. Name custom files with a prefix such as `zzz_mymod_...` only when load order matters. Unique keys are safer than relying on file order.

## Directory Hygiene

- Keep mod files in the matching game folder path.
- Do not include unused copied vanilla files.
- Use one feature per file when that improves maintenance.
- Use stable prefixes: `mymod_`, `mymod.`, `GFX_mymod_`.
- Keep compatibility notes when overriding vanilla objects.

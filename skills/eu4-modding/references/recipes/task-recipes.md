# Task Recipes

Use these as starting procedures. Then open the deeper reference for the feature.

## Create A New Event Chain

Files:

- `events/mymod_events.txt`
- `localisation/mymod_l_english.yml`
- Optional: `common/on_actions/mymod_on_actions.txt`
- Optional: `common/scripted_triggers/mymod_triggers.txt`
- Optional: `common/scripted_effects/mymod_effects.txt`

Steps:

1. Choose a namespace, e.g. `namespace = mymod`.
2. Reserve event IDs in a range, e.g. `mymod.1` through `mymod.99`.
3. Decide root scope: `country_event` or `province_event`.
4. Use MTTH for organic events; use `is_triggered_only = yes` for events fired by decisions, missions, effects, or on_actions.
5. Put shared conditions in scripted triggers and shared rewards in scripted effects.
6. Localise title, desc, and every option name.
7. Test with console: `event mymod.1 TAG` or a province target where appropriate.

## Add A Country Decision

Files:

- `decisions/mymod_decisions.txt`
- `localisation/mymod_l_english.yml`
- Optional scripted helpers in `common/scripted_triggers` and `common/scripted_effects`

Steps:

1. Wrap all decisions in `country_decisions = { ... }`.
2. Use a unique decision key.
3. Put visibility in `potential`.
4. Put clickability in `allow`.
5. Put immediate consequences in `effect`.
6. Add `provinces_to_highlight` for province requirements that should be obvious in UI.
7. Add `ai_will_do` and possibly `ai_importance` if AI may use it.
8. Localise `<decision_key>_title` and `<decision_key>_desc`.

## Build A Mission Tree

Files:

- `missions/mymod_missions.txt`
- `localisation/mymod_l_english.yml`
- Optional: `interface/mymod_mission_icons.gfx`
- Optional: `gfx/interface/missions/mission_<icon>.dds`

Steps:

1. Define one or more mission series.
2. Set `slot` for the column and `generic = yes/no`.
3. Use `potential_on_load` and `potential` to select countries.
4. For each mission, set `icon`, `required_missions`, `position`, `trigger`, `effect`.
5. Use `provinces_to_highlight` for conquest, ownership, state, or claim missions.
6. Localise `<mission_key>_title` and `<mission_key>_desc`.
7. For tag-changing decisions, use or consider `swap_non_generic_missions = yes`.

## Create A New Country

Files:

- `common/country_tags/zz_mymod_countries.txt`
- `common/countries/<Country Name>.txt`
- `history/countries/ABC - <Country Name>.txt`
- `localisation/mymod_l_english.yml`
- `gfx/flags/ABC.tga`
- Optional: ideas, missions, decisions, events, country colors, dynasty colors.

Steps:

1. Pick an unused 3-letter uppercase tag.
2. Map the tag to a country file.
3. Define country color, graphical culture, unit names, monarch names, and ship/army names.
4. Define country history: government, tech group, religion, culture, capital, ruler, heir, ideas, estate setup.
5. Add localisation for tag name and adjective if needed.
6. Add flag asset.
7. Put the country on the map via province history or release/create it by event/decision.

## Add A Bookmark Or Historical Start

Files:

- `common/bookmarks/mymod_bookmark.txt`
- `history/countries/*.txt`
- `history/provinces/*.txt`
- Optional: `history/advisors/mymod_advisors.txt`
- Optional: `history/diplomacy/mymod_diplomacy.txt`
- Optional: `history/wars/mymod_wars.txt`
- `localisation/mymod_l_english.yml`

Steps:

1. Choose the bookmark date first; history files must support that exact date.
2. Add `bookmark = { name desc date country = TAG ... }` in `common/bookmarks`.
3. Ensure country and province history already establish the intended world state by that date.
4. Add advisor history if the scenario needs named advisors available at start.
5. Add diplomacy history for alliances, unions, vassals, marriages, truces, HRE, or EoC setup.
6. Add war history only when the scenario should begin during an active war.
7. Localise bookmark `name` and `desc`.
8. Test by launching that start date and checking countries, diplomacy, wars, and advisor pools in game.

## Add Dynamic Localisation

Files:

- `customizable_localization/mymod_dynamic_text.txt`
- `localisation/mymod_l_english.yml`
- Optional caller files: missions, events, decisions, tooltips, GUI localisation

Steps:

1. Define one `defined_text = { name = GetMymod... }` block per callable dynamic selector.
2. Add ordered `text = { localisation_key = ... trigger = { ... } }` branches from most specific to most general.
3. Add a final fallback branch without `trigger` unless blank output is intentional.
4. Create all referenced localisation keys in normal localisation files.
5. Call the selector from visible text with bracket syntax such as `[Root.GetMymodRulerTitle]`.
6. For mission-preview helpers or other hardcoded dynamic triggers, copy a current vanilla shape rather than improvising helper names.
7. Test in game for both expected branches and fallback output; raw bracket text usually means the callable name or localisation key is wrong.

## Add A Province

Files:

- `map/definition.csv`
- `map/provinces.bmp`
- `map/default.map`
- `history/provinces/<id> - <Province Name>.txt`
- `map/area.txt`, `map/region.txt`, `map/superregion.txt`, `map/continent.txt`
- Often: `map/positions.txt`, `map/terrain.txt`, `map/climate.txt`, `common/tradenodes`, localisation.

Steps:

1. Avoid this unless the mod genuinely needs new geometry.
2. Choose an unused province ID and unique RGB color.
3. Update `definition.csv` and paint exactly that RGB in `provinces.bmp`.
4. Ensure `default.map` `max_provinces` exceeds the highest province ID.
5. Add province history and map grouping.
6. Add trade node, climate, terrain, positions, and localisation.
7. Launch with `-debug`; map load errors often identify missing IDs.

## Debug A Crash Or Broken Mod

Steps:

1. Launch with only the target mod enabled.
2. Use `-debug`.
3. Inspect latest `Documents/Paradox Interactive/Europa Universalis IV/logs/error.log`.
4. If crash-to-desktop, inspect `exceptions.log`.
5. Isolate by folder: events/decisions/missions/interface are usually easier to remove temporarily; common/history/map need dependency awareness.
6. Reduce to the last changed object.
7. Compare object shape against a known-working vanilla or wiki example.

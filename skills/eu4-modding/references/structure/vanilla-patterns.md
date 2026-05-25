# Vanilla Reference Strategy

This skill is standalone. Current-version vanilla examples are optional but valuable. When available, inspect the installed EU4 game files before writing syntax-heavy code that depends on patch-specific schemas.

## Useful Vanilla Paths

| Feature | Vanilla path to inspect |
| --- | --- |
| Events | `events/*.txt` |
| Decisions | `decisions/*.txt` |
| Missions | `missions/*.txt` |
| Country tags | `common/country_tags/*.txt` |
| Country definitions | `common/countries/*.txt` |
| Country colors | `common/country_colors/*.txt` |
| Country history | `history/countries/*.txt` |
| Advisor history | `history/advisors/*.txt` |
| Diplomacy history | `history/diplomacy/*.txt` |
| War history | `history/wars/*.txt` |
| Dynasty colors | `common/dynasty_colors/*.txt` |
| Dynamic text | `customizable_localization/*.txt` |
| Graphical cultures | `common/graphicalculturetype.txt` |
| Historical lucky tags | `common/historial_lucky.txt` |
| Province history | `history/provinces/*.txt` |
| Scripted triggers | `common/scripted_triggers/*.txt` |
| Scripted effects | `common/scripted_effects/*.txt` |
| On actions | `common/on_actions/*.txt` |
| Modifiers | `common/event_modifiers/*.txt`, `common/static_modifiers/*.txt`, `common/triggered_modifiers/*.txt` |
| Ideas | `common/ideas/*.txt` |
| Religions | `common/religions/*.txt` |
| Government reforms | `common/government_reforms/*.txt` |
| Localisation | `localisation/*_l_english.yml` |
| Map | `map/default.map`, `map/definition.csv`, `map/*.txt`, `map/*.bmp` |
| Interface | `interface/*.gui`, `interface/*.gfx` |
| Graphics | `gfx/**` |
| Music/sound | `music/*`, `sound/*` |

## Patterns Observed In Vanilla

- Event files use `namespace = ...`, then `country_event` or `province_event` blocks.
- Events localise title/desc/option keys explicitly, often with namespaced keys.
- Formable nation decisions use `country_decisions`, `potential`, `provinces_to_highlight`, `allow`, `effect`, `ai_will_do`, and often set a flag to prevent repetition.
- Mission files define series with `slot`, `generic`, `ai`, `potential_on_load`, `potential`, and mission blocks with `icon`, `required_missions`, `position`, `trigger`, `effect`.
- Scripted triggers and effects use `$ARG$` macro replacement for reusable templates.
- On actions often call scripted effects rather than placing all logic directly in the on_action block.
- Country history sets government, reform, tech group, cultures, religion, capital, estates, rulers, leaders, and dated changes.
- Advisor history uses repeated `advisor = { ... }` blocks with dates, locations, and advisor types.
- Diplomacy history uses relation blocks such as `alliance`, `union`, `vassal`, and `royal_marriage`.
- War history files start with `name` and `war_goal`, then use dated participant add/remove blocks.
- Country color files key by tag with `color1`, `color2`, and `color3`.
- Dynasty color files are ordered `color = { ... }` pools.
- `customizable_localization/*.txt` defines `defined_text` blocks that choose localisation keys by trigger order.
- `graphicalculturetype.txt` is a flat registry of valid graphical culture keys.
- `historial_lucky.txt` is priority ordered; the first valid lucky-tag entry wins.
- Province history sets owner/controller, culture, religion, development, trade goods, cores, city status, modifiers, discovery, and dated changes.
- Localisation files begin with `l_english:` and use one leading space before keys.
- Interface `.gui` files define `guiTypes = { ... }`; `.gfx` files define `spriteTypes = { ... }`.
- Mission icons and event pictures are sprite keys, not image paths directly in gameplay scripts.
- `default.map` declares map file names and province limits; map text files must agree on province IDs.

## Search Patterns

```bash
rg "namespace =|country_event =|province_event =" events
rg "country_decisions =|potential =|allow =|ai_will_do" decisions
rg "slot =|potential_on_load|required_missions|provinces_to_highlight" missions
rg "advisor = |advisor_id = |type = " history/advisors
rg "alliance = |union = |vassal = |royal_marriage = " history/diplomacy
rg "war_goal = |add_attacker = |add_defender = " history/wars
rg "defined_text = |localisation_key = " customizable_localization
rg "custom_trigger_tooltip|hidden_effect|save_event_target_as|random_list" common events decisions missions
rg "spriteType|windowType|guiButtonType|eventPicture|mission_" interface
rg "l_english:|_title:|_desc:" localisation
```

## How To Copy Safely

1. Copy only the object shape, not a whole file.
2. Rename every public key with your mod prefix.
3. Replace vanilla-specific tags, province IDs, modifiers, flags, and localisation keys deliberately.
4. Preserve context shape: triggers stay triggers, effects stay effects.
5. Add localisation and assets.
6. Test in game and read logs.

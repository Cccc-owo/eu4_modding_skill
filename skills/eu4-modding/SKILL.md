---
name: eu4-modding
description: Use when creating, editing, reviewing, or debugging Europa Universalis IV mods, Clausewitz script, localisation, events, decisions, missions, countries, history, map, interface, graphics, audio, Steam/launcher descriptors, or EU4 mod compatibility.
---

# EU4 Modding

## Scope

Use this skill as a standalone EU4 modding knowledge base. It does not require external files. Use the embedded patterns and the EU4 Wiki pages listed in `references/validation/wiki-pages.md`; when the user provides current vanilla examples, treat them only as optional evidence for patch-specific details.

EU4 uses Clausewitz-style text data. Most mod work is matching the correct folder, object shape, scope context, localisation keys, asset declarations, and load/test workflow.

## Workflow

1. Classify the change:
   - Structure/setup: descriptor, `.mod`, folders.
   - Scripted gameplay: events, decisions, missions, on_actions, scripted triggers/effects.
   - Database content: countries, history, ideas, religions, reforms, modifiers, units, trade, estates.
   - Advanced systems: disasters, privileges/agendas, institutions, great projects, CBs, war goals, subject types, rebels, custom GUI.
   - Specialized systems: advisors, buildings, cultures, EoC/HRE, factions, mercenaries, personalities, technology, units, trade goods, bookmarks, historical scenario support, RNW, religious/government subsystems.
   - Presentation: localisation, customizable localisation, interface, graphics, audio.
   - Map: provinces, definition, areas, regions, terrain, trade nodes.
   - Debugging: logs, run files, checksum, load order, crashes.
2. Open `references/structure/mod-structure.md` when creating or locating files.
3. Open `references/examples/basic-syntax-examples.md` first for a copyable ordinary file shape.
4. Open `references/syntax/script-syntax-scopes.md` when writing triggers, effects, scopes, variables, flags, or scripted helpers.
5. Open `references/systems/advanced-systems.md` and `references/examples/advanced-syntax-examples.md` when the task touches complex `common/` systems.
6. Open `references/systems/specialized-systems.md` and `references/examples/specialized-syntax-examples.md` when the task touches a wiki-listed specialized topic.
7. Open the matching reference file below for rules and pitfalls.
8. Choose additive vs override:
   - Additive: new file and unique keys. Prefer this for events, decisions, missions, scripted helpers, localisation, most common objects.
   - Override: reuse a vanilla key or replace a file/object. Use only when intentionally changing existing behavior.
9. Prefix new public identifiers: namespace, event IDs, flags, variables, event targets, modifiers, missions, decisions, scripted triggers/effects, sprites, localisation keys.
10. Write script in the correct context:
   - Trigger context: `potential`, `allow`, `trigger`, `limit`, `provinces_to_highlight`.
   - Effect context: `effect`, `immediate`, event options, scripted effects, run files.
11. Add localisation for every visible key.
12. Test with static review, game logs, and console/run-file checks.

## Reference Loader

Open only what the task needs:

Structure:

- `references/structure/mod-structure.md`: launcher descriptors, folder layout, additive vs override strategy.
- `references/structure/vanilla-patterns.md`: distilled current-version comparison strategy and safe-copy patterns.

Syntax:

- `references/syntax/script-syntax-scopes.md`: Clausewitz syntax, scopes, triggers, effects, flags, variables, scripted helpers.
- `references/syntax/localisation.md`: localisation format, customizable localisation, dynamic text, formatting, replace keys.

Examples:

- `references/examples/basic-syntax-examples.md`: broad copyable EU4 file syntax examples. Read this first when writing ordinary files.
- `references/examples/advanced-syntax-examples.md`: copyable syntax for complex `common/` systems. Read with `systems/advanced-systems.md`.
- `references/examples/specialized-syntax-examples.md`: copyable syntax for specialized EU4 systems. Read with `systems/specialized-systems.md`.

Systems:

- `references/systems/events-decisions-missions.md`: events, decisions, missions, on_actions, mission UI basics.
- `references/systems/countries-history-common.md`: countries, province/country history, common databases.
- `references/systems/common-databases.md`: detailed `common/` folder systems and compatibility risk.
- `references/systems/advanced-systems.md`: disasters, estates/privileges/agendas, policies, ages, CBs, war goals, subjects, institutions, great projects, government mechanics, triggered modifiers, rebels, trade companies, holy orders, parliament, diplomatic actions, peace treaties, scripted functions, custom GUI.
- `references/systems/specialized-systems.md`: advisors, buildings, centers of trade, cultures, EoC/HRE/factions, mercenaries, ruler/AI personalities, technology, units, trade goods/prices, bookmarks, RNW, religious/government subsystems, graphics/fonts/models, external tools.
- `references/systems/map-modding.md`: map files, province additions, bitmap rules, area/region chains.
- `references/systems/interface-gfx-audio.md`: `.gui`, `.gfx`, DDS sprites, flags, music, sound.

Recipes And Validation:

- `references/recipes/task-recipes.md`: common end-to-end tasks.
- `references/validation/validation-troubleshooting.md`: logs, run files, checksum, debugging order.
- `references/validation/wiki-pages.md`: verified EU4 Wiki pages and Playwright browsing pattern.

## Hard Rules

- Do not edit game installation files directly. Create a mod folder that mirrors game paths.
- Do not leave visible strings unlocalised.
- Do not reuse event IDs or global object keys accidentally.
- Do not put effects in trigger blocks or triggers in effect blocks.
- Do not invent triggers, effects, scopes, or modifiers. Verify their names against the EU4 Wiki pages in `references/validation/wiki-pages.md`; use file examples only for folder-specific object shapes.
- Do not assume every folder merges the same way. Prefer new files with unique keys when possible.
- Do not make map bitmap changes casually; map errors often crash at load.
- Do not trust only syntax appearance. Launch with `-debug` and inspect `error.log` for real validation.

## Fast Static Checks

```bash
# Duplicate or conflicting custom identifiers
rg "\bmymod_|mymod\\." .

# Missing localisation candidates
rg "title =|desc =|name =|custom_tooltip|tooltip =" events decisions missions common

# Common scope mistakes
rg "potential =|allow =|trigger =|limit =" common events decisions missions
rg "add_|set_|clr_|remove_|country_event =|province_event =" common events decisions missions
```

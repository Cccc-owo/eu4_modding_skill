# Specialized Systems

Use this for specialized EU4 systems not covered by the core event/decision/mission/map workflow. These systems are real modding targets, but they are narrower, more UI-bound, or more patch-sensitive than ordinary script.

For copyable object shapes, see `../examples/specialized-syntax-examples.md`.

## Wiki Gap Checklist

The EU4 Wiki modding portal lists these specialized pages in addition to the core pages: Advisor, Building, Culture, Empire of China, Faction, Holy Roman Empire, Mercenaries, Ruler Personality, Technology, Trade Goods, Units, Scenario/Bookmarks, Random New World, Map Quick Reference, Model, Font, Unit Models, Customizable Localization, and The Validator.

When a task mentions any of those terms, load this file. If the user provides current vanilla examples, use them only to confirm patch-specific schema details.

Fast navigation:

- Dynamic strings or mission preview text: open `../syntax/localisation.md` and the customizable localisation wiki page.
- Scenario start setup or bookmark curation: open `../examples/specialized-syntax-examples.md` and then inspect `history/countries`, `history/provinces`, `history/diplomacy`, `history/wars`, and `history/advisors`.
- Hardcoded/global UI bridge files: open `common-databases.md` plus the matching specialized example block.

## Advisors

Folder: `common/advisortypes`

Advisor types are keyed by advisor type key such as `philosopher` or `mymod_reformist`. Common fields include `monarch_power = ADM|DIP|MIL`, direct modifiers, `skill_scaled_modifier`, `chance`, `ai_will_do`, and optional gender restrictions. Add localisation and male/female portrait icons.

Do not rename advisor type keys after use; they are referenced by events, history, decisions, and scripted logic.

## Buildings And Centers Of Trade

Folders: `common/buildings`, `common/centers_of_trade`

Buildings use province ROOT and owner FROM. Common fields include `cost`, `time`, `modifier`, `trigger`, `one_per_country`, `manufactory`, `onmap`, lifecycle effects, and `ai_will_do`. The AI evaluates yearly return but still multiplies by `ai_will_do`.

Centers of trade define `level`, `type`, optional `cost`/`development`, and province/state/global modifiers.

## Cultures

Folder: `common/cultures`

Culture groups contain graphical culture settings, name pools, and cultures. Cultures can define `primary`, `male_names`, `female_names`, and `dynasty_names`. Wiki notes that name lists affect custom nation and dynasty generation differently from pre-existing country names, which often come from `common/countries`.

Primary tags affect cultural independence rebel target selection. Keep culture and group keys stable.

## Empire Of China, HRE, Factions

Folders: `common/imperial_reforms`, `common/decrees`, `common/factions`, `common/imperial_incidents`

Empire of China reforms are imperial reform blocks with `empire = celestial_empire`, optional `trigger`, `member`, `emperor`, `on_effect`, and `off_effect`. Decrees use `cost`, `duration`, `potential`, `trigger`, `modifier`, `effect`, `removed_effect`, and `ai_will_do`.

HRE reforms use `empire = hre` plus `province`, `member`, `emperor`, `emperor_per_prince`, `on_effect`, `off_effect`, `required_reform`, and GUI fields. HRE setup also involves history, religion, electors, emperor, reform images, and imperial incidents. There can only be one HRE-like empire mechanically, though localisation can disguise it.

Factions use `monarch_power`, `always`, optional `allow`, and `modifier`. Faction influence is driven elsewhere by script and mechanics; adding factions normally also requires matching government and UI support.

Imperial incidents define an event, `default_option`, optional `can_stop`, and numbered AI option weights. Console can start incidents by key.

## Mercenaries

Folder: `common/mercenary_companies`

Mercenary companies can be local or tied to `home_province`. Common fields include `regiments_per_development`, cavalry/artillery weights and caps, `cost_modifier`, `trigger`, optional `modifier`, and home province. Files can contain multiple companies; verify in-game ordering against a current example rather than assuming file order.

## Ruler And AI Personalities

Folders: `common/ruler_personalities`, `common/ancestor_personalities`, `common/leader_personalities`, `common/ai_personalities`, `common/ai_attitudes`

Ruler personality blocks use `ruler_allow`, `heir_allow`, `consort_allow`, `allow`, `chance`, modifier keys, AI behavior flags, and nation designer cost. Localise names and descriptions.

Vanilla comments state AI personalities cannot be newly added in `common/ai_personalities`; do not rename existing AI personality names. AI attitudes are hardcoded, though scripted triggers/effects can reference the known attitude keys.

Treat AI personality and AI attitude names as closed sets unless a current vanilla example proves otherwise. Do not derive new keys from naming patterns.

## Technology, Units, Trade Goods

Folders: `common/technology.txt`, `common/technologies`, `common/units`, `common/units_display`, `common/tradegoods`, `common/prices`

`common/technology.txt` defines technology groups and table metadata. Vanilla comments state that groups must be defined before tables, new technology keys must be unique, and newly added technologies go after the last technology. Do not confuse this file with `common/technologies/adm.txt`, `dip.txt`, and `mil.txt`, which define the per-level rewards.

Technology files start with `monarch_power`, define `ahead_of_time`, then repeated `technology = { ... }` blocks with years, institution expectations, unlock flags, modifiers, commands, and optional effects. Tech groups and sprites require interface/localisation support.

Units are one-file-per-unit definitions. Land units use `type = infantry|cavalry|artillery`, `unit_type`, maneuver, morale/fire/shock stats. Ships use hull, cannons, blockade, speed, trade power, sailors, and sprite level. Technologies and country files must reference unit keys correctly. `common/units_display` controls display/model selection; changing stats alone does not create a new visual unit model.

Trade goods require `common/tradegoods` definitions and matching `common/prices` entries. Trade goods also need icons/sprites and localisation. When a trade-good chance block scopes to another country, verify FROM/ROOT meaning against a current example instead of assuming the colonising-country pattern applies.

## Bookmarks And History

Folders: `common/bookmarks`, `history/*`

Bookmarks use `bookmark = { name desc date country = TAG ... }`. A usable bookmark relies on history files for countries, provinces, diplomacy, wars, and advisors at that date. Bookmark localisation must be present.

History support files commonly use these shapes:

- `history/advisors/*.txt`: repeated `advisor = { advisor_id name location skill type date death_date ... }`
- `history/diplomacy/*.txt`: dated or direct relation blocks such as `alliance`, `union`, `vassal`, `royal_marriage`, `truce`
- `history/wars/*.txt`: one war file with `name`, `war_goal`, then dated `add_attacker`, `add_defender`, `rem_attacker`, `rem_defender`

If a bookmark date looks wrong in game, verify every linked history file actually has valid entries spanning that date.

## Random New World

Folder: `map/random`

RNW uses generation settings (`tweaks.lua`), base images, scenarios (`RNWScenarios.txt`), name lists, tile text files, and tile bitmap data. Scenario definitions control climate suitability, countries, natives, religions, tech groups, government, graphical culture, names, uniqueness, and chance. Tile files list RGB-coded province features, dimensions, sea/land counts, centers of trade, estuaries, and regions.

RNW modding is map modding: bitmap compatibility and exact data/image agreement matter.

## Religious And Government Subsystems

Folders include `common/church_aspects`, `common/fervor`, `common/fetishist_cults`, `common/personal_deities`, `common/golden_bulls`, `common/defender_of_faith`, `common/isolationism`, `common/incidents`, `common/religious_reforms`, `common/religious_conversions`, `common/state_edicts`, `common/naval_doctrines`, `common/trading_policies`, `common/professionalism`, `common/flagship_modifications`, `common/hegemons`, `common/federation_advancements`, and `common/natives`.

Most are keyed blocks with potential/trigger/can_select, modifiers, effects, AI weights, icons, and localisation. They are tied to a hardcoded mechanic or UI. Additive keys may do nothing unless a government, religion, reform, button, or UI list references them.

## Prices, Power Projection, Insults, Revolt Triggers

`common/prices` pairs trade good keys with `base_price` and `goldtype`. `common/powerprojection` contains static power projection reasons and values. `common/insults` contains triggered insult text keys. `common/revolt_triggers` maps cultural/nationalist rebel target tags to province triggers.

These files are global and compatibility-sensitive. Prefer adding unique keys only where the engine reads dynamic additions; otherwise treat as override territory.

## Global, Hardcoded, And Nation Designer Files

Files/folders include `common/achievements.txt`, `common/msgrdk_achievements.json`, `common/alerts.txt`, `common/ai_army`, `common/client_states`, `common/custom_country_colors`, `common/custom_ideas`, `customizable_localization/*.txt`, `common/estates_preload`, `common/government_names`, `common/government_ranks`, `common/graphicalculturetype.txt`, `common/historial_lucky.txt`, `common/opinion_modifiers`, `common/parliament_bribes`, `common/province_names`, `common/region_colors`, and `common/subject_type_upgrades`.

These are real data files, but many are global, hardcoded, platform-facing, or UI-facing:

- Achievements use `possible` and `happened` trigger blocks plus IDs/localisation; platform mappings such as `msgrdk_achievements.json` are not normal gameplay mod targets.
- Alerts classify built-in alert keys and default sound/icons. Treat new alert keys as engine/UI work, not as plain additive data.
- `custom_country_colors` and `custom_ideas` affect the nation designer palette and idea picker.
- `customizable_localization` defines callable dynamic text functions through `defined_text = { name ... text = { localisation_key trigger } }`; branch order matters because the first valid branch wins.
- `government_names`, `government_ranks`, `client_states`, and `province_names` are dynamic localisation selectors. Order matters when the file says the first valid entry wins.
- `graphicalculturetype.txt` is a key registry for graphical cultures; countries, cultures, RNW, assets, and unit displays must all reference valid keys.
- `historial_lucky.txt` is priority-ordered. Put specific replacements before broad always-valid entries.
- `opinion_modifiers` contains hardcoded names marked as unsafe to remove or rename. Additive opinion modifiers are safer than renaming vanilla keys.
- `estates_preload` preloads estate loyalty/influence/slot modifiers and must match real estate and modifier keys.
- `parliament_bribes`, `subject_type_upgrades`, `ai_army`, `region_colors`, and `units_display` are narrow data bridges for hardcoded screens or AI behavior. Copy a current vanilla shape before changing them.

## Graphics, Fonts, Models, Unit Models

Wiki separates model, font, and unit model modding from `.gfx` sprite registration:

- Fonts live under `gfx/fonts` and are used by interface text elements; new fonts require bitmap font assets and font definitions.
- Models live under `gfx/models` with mesh, animation, texture, and asset metadata.
- Unit models are hard to mod and often require asset, sprite pack, entity, DLC, and technology/culture mapping files.

Keep this skill's graphics guidance lightweight; for production 3D/unit model work, open the wiki page and inspect a working vanilla asset chain.

## Tools From Wiki

The wiki lists CWTools, EU4 Mod Editing Tool, JoroDox, Clausewitz Scenario Editor, and The Validator. These are optional external tools, but useful for large mods. The Validator is especially useful for locating obscure file errors and CTD sources, though old wiki pages may describe outdated versions.

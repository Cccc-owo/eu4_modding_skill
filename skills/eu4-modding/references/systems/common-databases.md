# Common Databases

Use this when adding or changing gameplay database objects under `common/`. For copyable advanced object shapes, also read `advanced-systems.md` and `../examples/advanced-syntax-examples.md`.

## High-Value Folders

| Folder | What it defines |
| --- | --- |
| `common/achievements.txt`, `msgrdk_achievements.json` | Achievement trigger definitions and platform achievement ID mapping; treat as platform-facing content, not ordinary gameplay data |
| `common/advisortypes` | Advisor types and modifiers |
| `common/ai_army` | AI province war/peace evaluation data; global behavior tuning |
| `common/ai_personalities`, `ai_attitudes` | AI personality weights and hardcoded attitude keys |
| `common/alerts.txt` | Alert sound/icon defaults and HIGH/MEDIUM/LOW/DISABLED alert categories |
| `common/bookmarks` | Scenario/bookmark start dates and country picks |
| `common/ages` | Ages, objectives, abilities |
| `common/buildings` | Buildings and native buildings |
| `common/cb_types` | Casus belli behavior |
| `common/centers_of_trade` | Center of trade levels and modifiers |
| `common/church_aspects`, `fervor`, `golden_bulls`, `personal_deities`, `fetishist_cults` | Religion-specific mechanics |
| `common/client_states` | Client state dynamic names by region, area, and government trigger |
| `common/colonial_regions` | Colonial region membership |
| `common/countries` | Country visual/name pools |
| `common/country_colors` | Per-tag unit/banner colour palettes using `color1`/`color2`/`color3` |
| `common/country_tags` | Tag-to-country-file mapping |
| `common/cultures` | Culture groups and cultures |
| `common/custom_country_colors` | Custom nation colour palette and symbol count |
| `common/custom_gui` | Script hooks for supported GUI elements |
| `common/custom_ideas` | Nation designer custom idea modifier choices, levels, costs, and AI chance |
| `common/defender_of_faith` | Defender of faith tier benefits and religion-group scaling |
| `common/defines` and `defines.lua` | Engine constants; high compatibility risk |
| `common/diplomatic_actions` | Extra conditions/effects for hardcoded diplomatic actions |
| `common/disasters` | Disaster progression and effects |
| `common/dynasty_colors` | Dynasty-name colour definitions used by UI and countries |
| `common/estate_agendas`, `estate_privileges`, `estate_crown_land`, `estates` | Estates, privileges, agendas, crown land, interactions, and estate-specific availability |
| `common/estates_preload` | Estate loyalty/influence/privilege slot modifier preload rules |
| `common/event_modifiers` | Country/province modifiers used by script |
| `common/factions` | Faction definitions and modifiers |
| `common/federation_advancements` | Native federation advancement costs, triggers, and effects |
| `common/flagship_modifications`, `naval_doctrines` | Naval special mechanics |
| `common/government_names`, `government_ranks` | Localised rank/ruler/heir title selection and rank definitions |
| `common/government_mechanics` | Custom government power bars and interactions |
| `common/governments`, `government_reforms` | Government types and reform tiers |
| `common/graphicalculturetype.txt` | Allowed graphical culture keys referenced by countries, cultures, RNW, units, and assets |
| `common/great_projects` | Monuments/great projects |
| `common/hegemons` | Hegemon allow/base/scale/max blocks |
| `common/historial_lucky.txt` | Historical lucky nation priority list and triggers |
| `common/holy_orders` | Holy order definitions and province effects |
| `common/ideas` | National idea groups and idea groups |
| `common/imperial_reforms`, `imperial_incidents`, `decrees` | HRE and Empire of China reforms/incidents/decrees |
| `common/institutions` | Institution spread and origin |
| `common/insults` | Insult text selection rules and hardcoded insult pools |
| `common/isolationism` | Isolationism incident/reward configuration |
| `common/mercenary_companies` | Mercenary company composition and availability |
| `common/natives` | Native advancement, reform, and migration-related data |
| `common/new_diplomatic_actions` | Fully scripted diplomatic actions |
| `common/on_actions` | Engine hooks to fire script |
| `common/opinion_modifiers` | Hardcoded and scripted opinion modifier values, caps, and decay |
| `common/parliament_bribes` | Parliament seat bribe options and AI weights |
| `common/parliament_issues` | Parliament debate issues and rewards |
| `common/peace_treaties` | Scripted peace treaty options |
| `common/policies` | Policy availability and modifiers |
| `common/powerprojection` | Power projection sources and values |
| `common/prices` | Trade good base prices and gold behavior |
| `common/professionalism` | Army professionalism thresholds and rewards |
| `common/province_triggered_modifiers` | Province-level triggered modifiers |
| `common/province_names` | Dynamic province naming by culture, tag, religion, and region/province trigger |
| `common/rebel_types` | Rebel behavior and demands |
| `common/region_colors` | Map region/superregion/area colour metadata for UI/map modes |
| `common/religious_conversions`, `religious_reforms` | Religion-specific conversion mechanics and reform progression |
| `common/religions` | Religion groups, religions, mechanics |
| `common/revolt_triggers` | Nationalist/cultural rebel target rules |
| `common/revolution` | Revolution target, spread, and mechanic configuration |
| `common/ruler_personalities`, `leader_personalities`, `ancestor_personalities` | Personalities and traits |
| `common/scripted_effects` | Reusable effect macros |
| `common/scripted_functions` | Conditions for exposed hardcoded actions |
| `common/scripted_triggers` | Reusable trigger macros |
| `common/static_modifiers` | Always-used modifier keys |
| `common/state_edicts`, `trading_policies` | State and trade-node selectable policies |
| `common/subject_types` | Subject rules and interactions |
| `common/subject_type_upgrades` | Subject upgrade definitions tied to subject-type mechanics |
| `common/technology.txt` | Technology groups and technology table metadata; distinct from `common/technologies/*.txt` |
| `common/technologies` | Tech groups and technologies |
| `common/timed_modifiers` | Timed modifier definitions applied by events and script |
| `common/trade_companies` | Trade company region province sets and names |
| `common/tradecompany_investments` | Trade company investment definitions |
| `common/tradegoods` | Trade goods and prices |
| `common/tradenodes` | Trade node graph and province membership |
| `common/triggered_modifiers` | Country-level triggered modifiers |
| `common/units` | Unit definitions |
| `common/units_display` | Unit model/display mapping by type, culture, tech, and country conditions |
| `common/wargoal_types` | War goal scoring, costs, and peace options |

## General Pattern

Most objects are keyed blocks:

```txt
mymod_object_key = {
    icon = my_icon
    modifier = {
        global_tax_modifier = 0.10
    }
}
```

Rules:

- Use unique keys unless intentionally overriding.
- Match the expected folder-specific shape.
- Add localisation for visible names/tooltips.
- Check every referenced key: modifiers, icons, sprites, religion/culture IDs, estate IDs, reform IDs, province IDs.
- Prefer scripted triggers/effects for complex repeated logic.

## Modifiers

Modifier blocks are just named sets of exported modifier keys:

```txt
mymod_reform_energy = {
    reform_progress_growth = 0.10
    global_unrest = -1
}
```

Apply them through ideas, religions, reforms, events, decisions, missions, triggered modifiers, or static modifiers.

## Ideas

Idea groups commonly contain:

- `start = { ... }`: traditions.
- Seven idea blocks.
- `bonus = { ... }`: ambition.
- `trigger = { ... }`: tag/culture/religion ownership of the ideas.
- `free = yes`: national ideas.

## Religions

Religion files are nested by group:

```txt
christian = {
    mymod_faith = {
        color = { 100 100 200 }
        icon = 20
        country = {
            tolerance_own = 1
        }
        province = {
            local_missionary_strength = -0.01
        }
    }
}
```

When changing religions, verify the paired UI, decisions, events, conversion rules, centers, icons, and localisation instead of treating the religion file as a self-contained edit.

## Government Reforms

Government reforms interact with government type, tier, attributes, locked reforms, estates, and mechanics. Avoid overriding existing reform keys unless you have checked every known script reference that targets them.

## Trade Nodes

Trade nodes define province membership and outgoing connections. Changing a trade node can affect:

- province trade power
- merchants and steering
- missions and decisions
- colonial and trade company behavior

## Defines

Defines are global constants. Treat them as override territory and prefer scripted changes or modifiers unless the behavior is impossible to achieve elsewhere.

## Compatibility Triage

Treat these as lower-blast-radius edits:

- New uniquely keyed scripted triggers/effects.
- New event modifiers.
- New events/decisions/missions.
- New localisation.

Treat these as shared-data edits that need broader reference checks:

- New countries, ideas, reforms, religions with unique keys.
- New assets and UI additions.

Treat these as override or global-risk edits:

- Overriding vanilla object keys.
- Defines changes.
- Map geometry.
- Whole-file interface replacements.

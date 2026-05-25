# Advanced Systems

Use this for larger `common/` systems that are easy to mis-shape: disasters, estates, policies, ages, CBs, war goals, subjects, institutions, great projects, government mechanics, rebels, trade companies, holy orders, parliament, diplomatic actions, peace treaties, scripted functions, and custom GUI script.

Read `../examples/advanced-syntax-examples.md` for copyable templates. This file explains the object contracts, scopes, and compatibility risks.

## General Rules

- Treat every `common/` folder as its own schema. A keyed block that works in one folder may be invalid in another.
- Prefer unique keys and additive files. Override vanilla keys only when intentionally replacing behavior.
- Patch-specific systems should be copied from the closest current vanilla object when available, then renamed and simplified.
- Localise visible keys: object names, descriptions, tooltips, custom trigger tooltips, disaster names, agenda names, rebel demands, CB names, war names, and GUI tooltips.
- Add AI weights deliberately. Many advanced objects can work for players while making AI behavior nonsensical.
- Scripted helper contracts are not inferred. If an effect or trigger name is not proven in current vanilla or a verified wiki page, do not use it.

## Disasters

Folder: `common/disasters/*.txt`

Country-scope disaster blocks commonly contain:

- `potential`: who can ever see the disaster.
- `can_start`: conditions to begin progress.
- `can_stop`: conditions that stop progress before firing.
- `progress`: monthly progress factors.
- `can_end`: conditions to end an active disaster.
- `modifier`: active country modifiers.
- `on_start`, `on_end`: event IDs fired by the disaster.
- `on_monthly`: monthly events and random events while active.
- Optional flags such as `ended_by_country_breaking_to_rebels`.

Pitfalls:

- Gate with `has_any_disaster = no` if simultaneous disasters are not intended.
- Set and clear durable flags in start/end events, not only in disaster progress.
- Avoid progress modifiers that can never be positive.
- Pair `on_start` and `on_end` events with `is_triggered_only = yes`.

## Estates, Privileges, Agendas, Crown Land

Folders: `common/estates`, `common/estate_privileges`, `common/estate_agendas`, `common/estate_crown_land`

Estate blocks are country-scope definitions for whether an estate exists and what its happy/neutral/angry modifiers, land behavior, loyalty, and influence do.

Privileges are country-scope. Common fields:

- `icon`
- `land_share`
- direct modifiers such as `max_absolutism`, `loyalty`, `influence`
- `is_valid`, `can_select`, `can_revoke`
- `on_granted`, `on_revoked`, `on_invalid`
- `benefits`, `penalties`
- `conditional_modifier`
- `loyalty_scaled_conditional_modifier`
- `influence_scaled_conditional_modifier`
- `modifier_by_land_ownership`
- `mechanics`
- `cooldown_years`, `on_cooldown_expires`
- `ai_will_do`

Agendas are mostly country-scope, except `provinces_to_highlight`, where ROOT is the country and the tested object is a province. Common fields:

- `max_days_active`
- `can_select`
- `selection_weight`
- `pre_effect`, `immediate_effect`
- `task_requirements`
- `task_completed_effect`
- `failing_effect`
- `fail_if`
- `invalid_trigger`, `on_invalid`
- `modifier`

Crown land interaction files use `interaction = { key = ... trigger = { } effect = { } ai_will_do = { } }`. Crown land bonuses use `bonus = { key = ... range_from = ... range_to = ... modifier = { } }`.

Pitfalls:

- Do not use pseudo scopes like `estate_nobles = { add_loyalty = 5 }` unless verified. Vanilla effects normally use `add_estate_loyalty = { estate = estate_nobles loyalty = 5 }`.
- Estate privilege values often use decimals for loyalty/influence modifiers, while effects such as `add_estate_loyalty` use larger percentage-like numbers.
- Revocation and invalidation need cleanup for flags, modifiers, event targets, and variables.
- Estate IDs are global keys. Prefix custom estates and privileges unless intentionally integrating with vanilla estates.

## Policies

Folder: `common/policies/*.txt`

Policies are keyed blocks with:

- `monarch_power = ADM|DIP|MIL`
- `potential`: in current vanilla, commonly the required idea groups.
- `allow`: in current vanilla, commonly the full-idea-group completion check.
- direct modifier keys.
- `effect`, `removed_effect`
- `ai_will_do`

Pitfalls:

- `potential` controls whether a policy can be considered; `allow` controls whether it can be enacted.
- Policy cost/category behavior is engine-driven; do not invent fields without checking a current vanilla object.

## Ages

Folder: `common/ages/*.txt`

Ages define:

- `start`
- `can_start`
- top-level age flags such as religious or papacy behavior
- `objectives = { objective_key = { triggers } }`
- `abilities = { ability_key = { effect = { } modifier = { } ai_will_do = { } } }`

Pitfalls:

- Age objectives are country trigger blocks. Use `custom_trigger_tooltip` when the real condition is complex.
- Ability effects often set flags or call helper effects so other script can detect the ability. If you do this, prove each helper name from a current example.
- Ability modifiers should normally include `num_of_age_rewards = 1`.

## CB Types And War Goals

Folders: `common/cb_types`, `common/wargoal_types`

CB files use ROOT as attacker and FROM as target in prerequisites. Common fields:

- `valid_for_subject`
- `is_triggered_only`
- `months`
- `prerequisites_self`
- `prerequisites`
- `war_goal`

War goals define a `type`, attacker and defender score/cost rules, allowed provinces, peace options, and `war_name`.

Pitfalls:

- `is_triggered_only = yes` CBs must be granted by effects/events/missions; `prerequisites` alone will not make them appear.
- The order of `peace_options` can affect AI peace priorities.
- War goal province filters use war-scope meanings for ROOT/FROM; keep attacker/defender semantics clear.

## Subject Types

Folder: `common/subject_types/*.txt`

Subject types can `copy_from = default` or another type, then override graphics, annexation, liberty desire, income, trade, war behavior, interactions, subject/overlord modifiers, restoration CBs, and opinion modifiers.

Pitfalls:

- Subject type changes affect diplomacy, war, economy, subject interactions, UI sprites, and AI. Test full diplomatic lifecycle: creation, war calls, liberty desire, annexation, breakage.
- If adding a new subject type, verify every referenced sprite and modifier exists.

## Institutions

Folder: `common/institutions/*.txt`

Institution blocks commonly contain:

- `bonus`: embraced country bonus.
- optional trade company or special modifiers.
- `history`: province triggers for start-date presence.
- `can_start`, `start_chance`, `on_start`: for institutions that originate during play.
- `can_embrace`: country trigger.
- `embracement_speed`: province spread modifiers; when using `scale = yes`, copy a current institution example instead of assuming the same scaling contract elsewhere.
- optional `ai_will_do`.

Pitfalls:

- Institution spread runs over many provinces. Keep triggers efficient and avoid expensive nested random logic.
- History, origin, spread, and embrace logic are different contexts.

## Great Projects

Folder: `common/great_projects/*.txt`

Great projects use province-centric setup:

- `start`: province ID.
- `date`
- `time`, `build_cost`, `starting_tier`, `type`
- `can_be_moved`, `move_days_per_unit_distance`
- `build_trigger`
- `on_built`, `on_destroyed`
- `can_use_modifiers_trigger`
- `can_upgrade_trigger`
- `keep_trigger`
- `tier_0` through `tier_3`, each with upgrade time, cost, province/area/country modifiers, and `on_upgraded`.

Pitfalls:

- `build_trigger`, use, upgrade, and keep triggers can run in different practical contexts. Follow a vanilla monument/canal closest to your use case.
- Visible monument art, ambient objects, and province IDs must agree with map and assets.

## Government Mechanics

Folder: `common/government_mechanics/*.txt`

Government mechanics define custom progress/power systems tied to government reforms or flags. Common fields:

- `alert_icon_gfx`, `alert_icon_index`
- `available`: country trigger for the whole mechanic.
- `powers`: one or more power bars, each with min/max/default, monthly growth, scaling modifiers, range modifiers, and max/min effects.
- `interactions`: clickable actions with GUI key, icon, cost, trigger, effect, cooldown, and AI chance.

The engine generates modifier names such as `monthly_<power_type_id>` and `<power_type_id>_gain_modifier`.

Pitfalls:

- Mechanics need compatible UI definitions; script alone may not be enough.
- Generated modifiers need localisation and icons.
- Government reforms or effects must enable the mechanic deliberately.

## Triggered Modifiers

Folders: `common/triggered_modifiers`, `common/province_triggered_modifiers`

Country triggered modifiers commonly contain `potential`, `trigger`, and direct country modifiers.

Province triggered modifiers contain province-scope `potential` and `trigger`, direct modifiers, and optional province-scope `on_activation`/`on_deactivation`. Many are applied by effect with `add_province_triggered_modifier`, but prove that wiring from a current example before relying on it.

Pitfalls:

- A triggered modifier's `potential` normally controls visibility/eligibility; `trigger` controls activation.
- Province triggered modifiers frequently scope into `owner = { ... }` for country checks, but verify owner-scope usage against a current example when adding custom activation logic.
- Do not use expensive global checks in high-frequency triggered modifiers unless needed.

## Rebels

Folder: `common/rebel_types/*.txt`

Rebel types define:

- map behavior: `area`, `defection`, `independence`, `defect_delay`, `unit_transfer`, `will_relocate`
- unit behavior: `resilient`, `reinforcing`, `general`, `smart`, infantry/cavalry/artillery ratios, morale
- player responses: `handle_action_*`
- `spawn_chance`
- optional `movement_evaluation`
- province-scope `siege_won_trigger` and `siege_won_effect`
- country-scope `can_negotiate_trigger`, `can_enforce_trigger`, `demands_enforced_effect`
- `demands_description`

Pitfalls:

- Rebel systems are global and AI-visible. A spawn factor mistake can flood the world.
- Demands localisation is separate from the rebel type name.
- Keep province-scope siege effects and country-scope demand effects separate.

## Trade Companies And Investments

Folders: `common/trade_companies`, `common/tradecompany_investments`

Trade company regions define `color`, province lists, and one or more `names` blocks, optionally with triggers.

Investments define `category`, `sprite`, optional `upgrades_to`, `cost`, modifiers such as `company_province_area_modifier`, `area_modifier`, `owner_modifier`, `allow`, and AI worth blocks.

Pitfalls:

- Trade company provinces must match real province IDs and area/region design.
- Investments require sprites and localisation.
- The investment `allow` trigger is evaluated with ROOT as investor country and province scope set to the first province in the area.

## Holy Orders

Folder: `common/holy_orders/*.txt`

Holy orders define:

- `icon`
- `trigger`
- `color`
- `cost`, `cost_type`
- `per_province_effect`
- `per_province_abandon_effect`
- `modifier`
- `ai_priority`
- `localization`

Pitfalls:

- Per-province effects must be reversible or guarded if abandonment should not break province values.
- Icons must exist in interface assets.

## Parliament Issues

Folder: `common/parliament_issues/*.txt`

Parliament issues commonly contain:

- `category`
- `allow`
- `effect`
- `modifier`
- `chance`
- `ai_will_do`

Pitfalls:

- Some vanilla helpers pass script fragments as strings; copy those patterns carefully.
- Issue categories influence proposal variety.

## Diplomatic Actions

Folders: `common/diplomatic_actions`, `common/new_diplomatic_actions`

Legacy diplomatic action overrides use the hardcoded action key and can add `condition` blocks with `tooltip`, `potential`, and `allow`, plus one `effect` block. ROOT is the actor and FROM is the target.

New diplomatic actions define static alert metadata and scripted actions with fields such as `category`, `alert_index`, `alert_tooltip`, `require_acceptance`, `is_visible`, `is_allowed`, `on_accept`, `on_decline`, `ai_acceptance`, and `ai_will_do`. In action script, ROOT is actor and FROM is recipient.

Pitfalls:

- AI may not send newly scripted diplomatic actions unless the system supports it and weights are meaningful.
- Alert icons use frame indexes; avoid colliding with existing static actions unless intentionally reusing an icon.
- Acceptance scoring often uses `ai_acceptance = { add_entry = { ... variable_name = ai_value ... } }`.

## Peace Treaties

Folder: `common/peace_treaties`

Scripted peace treaties are negotiated between THIS as taker and FROM as giver. They can define localisation keys, category, diplomatic cost, prestige, AE, war score cost, visibility, allowed conditions, effect, and AI weight.

Pitfalls:

- Scripted treaties cannot target arbitrary third parties or selected provinces unless the treaty system supports that target directly.
- `is_make_subject = yes` makes the treaty mutually exclusive with other subject-making or subject-freeing treaties.
- `requires_is_allowed = yes` hides the treaty unless explicitly allowed by the CB.

## Scripted Functions

Folder: `common/scripted_functions`

Scripted functions add extra `condition` checks to hardcoded actions such as bankruptcy, moving capital, province HRE actions, autonomy actions, and similar engine functions. A condition has `tooltip`, `potential`, and `allow`.

Pitfalls:

- Most functions are hardcoded and only expose specific hook points.
- Match the documented scope comments in the file; many functions use ROOT as country, but province actions use ROOT as province and FROM as country.

## Custom GUI Script

Folder: `common/custom_gui/*.txt`, paired with `interface/*.gui`

Scripted GUI objects include:

- `custom_button`
- `custom_text_box`
- `custom_icon`
- `custom_shield`
- `custom_window`

The matching `.gui` object must use `scripted = yes` and the same `name`. Supported windows have fixed ROOT/FROM scopes; for most country screens ROOT is the visible country or player country and FROM is the clicking country. Province view objects use the clicked province as ROOT.

Pitfalls:

- Custom GUI is a glue layer between hardcoded windows and script. It is not a general UI framework.
- If the button exists in script but not in `.gui`, nothing appears.
- If the `.gui` object is not inside a supported parent window, script may never run.

## Defines, Static Modifiers, Timed Modifiers

Folders: `common/defines`, `common/static_modifiers`, `common/timed_modifiers`

- Defines are global engine constants and high compatibility risk.
- Static modifiers are globally referenced modifier bundles; overriding them can alter the whole game.
- Timed/event modifiers are safer when uniquely keyed and applied by script.

Prefer events, decisions, scripted effects, and unique modifiers before touching defines or static modifiers.

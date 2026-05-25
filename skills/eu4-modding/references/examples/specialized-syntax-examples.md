# Specialized Syntax Examples

Copy these shapes for specialized EU4 systems, then verify against current vanilla files for the target patch.

## common/advisortypes/mymod_advisors.txt

```txt
mymod_reformist = {
    monarch_power = ADM

    reform_progress_growth = 0.05

    skill_scaled_modifier = {
        trigger = {
            owner = {
                stability = 1
            }
        }
        modifier = {
            global_tax_modifier = 0.02
        }
    }

    chance = {
        factor = 1
    }

    ai_will_do = {
        factor = 1
    }
}
```

## common/buildings/mymod_buildings.txt

```txt
mymod_school = {
    cost = 100
    time = 12

    trigger = {
        is_city = yes
    }

    modifier = {
        local_institution_spread = 0.10
    }

    on_built = {
        add_base_tax = 1
    }

    on_destroyed = {
        add_base_tax = -1
    }

    on_construction_started = {
    }

    on_construction_canceled = {
    }

    ai_will_do = {
        factor = 1
    }
}
```

## common/centers_of_trade/mymod_centers_of_trade.txt

```txt
mymod_world_market = {
    level = 3
    type = coastal
    development = 25
    cost = 1000
    province_modifiers = {
        province_trade_power_value = 25
        local_institution_spread = 0.20
    }
    state_modifiers = {
        local_development_cost = -0.10
    }
    global_modifiers = {
        global_trade_power = 0.02
    }
}
```

## common/cultures/mymod_cultures.txt

```txt
mymod_culture_group = {
    graphical_culture = westerngfx
    second_graphical_culture = muslimgfx

    mymod_culture = {
        primary = ABC
        male_names = { Adam William }
        female_names = { Anne Mary }
        dynasty_names = { "Example" "de Example" }
    }
}
```

## common/imperial_reforms/mymod_eoc_or_hre.txt

```txt
mymod_celestial_reform = {
    empire = celestial_empire
    trigger = {
        meritocracy = 60
    }
    member = {
        state_maintenance_modifier = -0.10
    }
    emperor = {
        imperial_mandate = 0.05
    }
    on_effect = {
        set_country_flag = mymod_celestial_reform_passed
    }
    off_effect = {
        clr_country_flag = mymod_celestial_reform_passed
    }
}

mymod_hre_reform = {
    empire = hre
    province = {
        local_development_cost = -0.05
    }
    member = {
        stability_cost_modifier = -0.05
    }
    emperor = {
        diplomatic_reputation = 1
    }
    required_reform = reichsreform
    gui_container = decentralization
}
```

## common/decrees/mymod_decrees.txt

```txt
mymod_expand_censors = {
    cost = 20
    duration = 3650
    potential = {
        is_emperor_of_china = yes
    }
    trigger = {
        meritocracy = 50
    }
    modifier = {
        yearly_corruption = -0.05
    }
    effect = {
    }
    removed_effect = {
    }
    ai_will_do = {
        factor = 10
    }
}
```

## common/factions/mymod_factions.txt

```txt
mymod_scholars_faction = {
    monarch_power = ADM
    always = yes
    modifier = {
        idea_cost = -0.05
        land_morale = -0.05
    }
}
```

## common/imperial_incidents/mymod_incidents.txt

```txt
mymod_imperial_petition = {
    event = mymod.300
    default_option = 0

    can_stop = {
        NOT = { exists = ABC }
    }

    0 = { factor = 1 }
    1 = {
        factor = 1
        modifier = {
            factor = 5
            ai_attitude = {
                who = ABC
                attitude = attitude_friendly
            }
        }
    }
}
```

## common/mercenary_companies/mymod_mercs.txt

```txt
mymod_free_company = {
    regiments_per_development = 0.05
    cavalry_weight = 0.10
    artillery_weight = 0.10
    cavalry_cap = 2
    cost_modifier = 0.75
    home_province = 236
    trigger = {
        total_development = 150
        is_allowed_to_recruit_mercenaries = yes
    }
    modifier = {
        land_morale = 0.05
    }
}
```

## common/ruler_personalities/mymod_personalities.txt

```txt
mymod_reformer_personality = {
    ruler_allow = {
        allow = {
            ADM = 3
        }
        chance = {
            modifier = {
                factor = 1
                ADM = 5
            }
        }
    }

    heir_allow = {
        allow = {
            heir_ADM = 3
        }
    }

    consort_allow = {
        allow = {
            consort_ADM = 3
        }
    }

    chance = {
        modifier = {
            factor = 2
            stability = 1
        }
    }

    reform_progress_growth = 0.05
    nation_designer_cost = 2
}
```

## common/technologies/mymod_adm.txt

```txt
monarch_power = ADM

ahead_of_time = {
    production_efficiency = 0.20
}

technology = {
    year = 1440
    expects_institution = {
        feudalism = 0.5
    }
    production_efficiency = 0.02
    may_support_rebels = yes
}
```

Technology files are sequence-sensitive. Avoid replacing vanilla tech tables unless the whole mod owns tech progression.

## common/technology.txt

```txt
groups = {
    mymod_group = {
        start_level = 3
        start_cost_modifier = 0.25
        nation_designer_unit_type = western
        nation_designer_cost = {
            trigger = {
                capital_scope = {
                    continent = europe
                }
            }
            value = 75
        }
    }
}
```

Groups must appear before table definitions. Add new technology keys after the last existing technology key and keep every key unique.

## common/units/mymod_unit.txt

```txt
type = infantry
unit_type = western

maneuver = 1
offensive_morale = 1
defensive_morale = 1
offensive_fire = 0
defensive_fire = 0
offensive_shock = 1
defensive_shock = 1
```

Ship example:

```txt
type = light_ship
hull_size = 8
base_cannons = 10
blockade = 10
sail_speed = 10
trade_power = 2.0
sailors = 50
sprite_level = 1
```

## common/tradegoods and common/prices

`common/tradegoods/mymod_tradegoods.txt`:

```txt
mymod_saltpetre = {
    color = { 0.65 0.60 0.45 }
    modifier = {
        fire_damage = 0.02
    }
    province = {
        local_regiment_cost = -0.05
    }
    chance = {
        factor = 2
        modifier = {
            factor = 0
            has_terrain = glacier
        }
    }
}
```

`common/prices/mymod_prices.txt`:

```txt
mymod_saltpetre = {
    base_price = 3
}
```

## common/bookmarks/mymod_bookmark.txt

```txt
bookmark = {
    name = "MYMOD_BOOKMARK_NAME"
    desc = "MYMOD_BOOKMARK_DESC"
    date = 1500.1.1

    country = ENG
    country = FRA
    country = HAB
}
```

`history/advisors/mymod_advisors.txt`:

```txt
advisor = {
    advisor_id = 900001
    name = "John Example"
    location = 236
    discount = yes
    skill = 2
    type = statesman
    date = 1490.1.1
    death_date = 1520.1.1
}
```

`history/diplomacy/mymod_diplomacy.txt`:

```txt
alliance = {
    first = ENG
    second = POR
    start_date = 1500.1.1
    end_date = 1510.1.1
}

royal_marriage = {
    first = ENG
    second = CAS
    start_date = 1500.1.1
    end_date = 1508.1.1
}
```

`history/wars/mymod_war.txt`:

```txt
name = "MYMOD_SAMPLE_WAR"
war_goal = {
    type = superiority_insult
    casus_belli = cb_dishonored_call
}

1501.1.1 = {
    add_attacker = ENG
    add_defender = FRA
}

1503.1.1 = {
    rem_attacker = ENG
    rem_defender = FRA
}
```

Bookmarks are only presentation selectors. The actual world state comes from dated history files for countries, provinces, diplomacy, wars, and advisors.

## map/random/RNWScenarios.txt and tiles

Scenario:

```txt
mymod_rnw_animist_tribes = {
    min_provinces = 30
    force_apart = yes
    minor_tags = yes
    temperate = yes
    arid = no
    arctic = no
    tropical = yes
    min_countries = 3
    max_countries = 8
    min_country_size = 1
    max_country_size = 3
    religion = animism
    technology_group = south_american
    government = native
    graphical_culture = northamericagfx
    unique = yes
    chance = 500
}
```

Tile text files use RGB triplets that must match tile bitmap data:

```txt
sea_province = { 93 164 236 }
lake_province = { 100 149 210 }
num_sea_provinces = 21
num_land_provinces = 185
size = { 7 7 }
continent = yes
weight = 50
empty = { 34 97 212 }
level_1_center_of_trade = { 31 217 106 }
regions = 2
region = { 145 203 116 }
```

## common/state_edicts/mymod_edicts.txt

```txt
mymod_reform_edict = {
    potential = {
        current_age = age_of_absolutism
    }
    allow = {
        stability = 1
    }
    modifier = {
        local_monthly_devastation = -0.10
    }
    color = { 116 198 240 }
    ai_will_do = {
        factor = 1
    }
}
```

## common/naval_doctrines/mymod_naval_doctrine.txt

```txt
mymod_convoy_system = {
    can_select = {
        is_primitive = no
    }
    cost = 0.1
    country_modifier = {
        global_ship_trade_power = 0.25
    }
    effect = {
    }
    removed_effect = {
    }
    button_gfx = 3
}
```

## common/trading_policies/mymod_trading_policies.txt

```txt
mymod_secure_routes = {
    potential = {
        NOT = { has_country_flag = disabled_mymod_secure_routes }
    }
    can_select = {
        FROM = {
            has_trader = ROOT
        }
    }
    trade_power = {
        duration = -1
        power_modifier = 0.05
        key = mymod_secure_routes
    }
    button_gfx = GFX_Trading_Policy_Max_Profit
    cooldown = no
}
```

## common/flagship_modifications/mymod_flagship.txt

```txt
mymod_signal_book = {
    trigger = {
        adm_tech = 10
    }
    modifier = {
        movement_speed_in_fleet_modifier = 1
        naval_maintenance_flagship_modifier = 0.5
    }
    ai_trade_score = { factor = 1 }
    ai_war_score = { factor = 1 }
}
```

## common/hegemons/mymod_hegemons.txt

```txt
mymod_economic_hegemon = {
    allow = {
        is_great_power = yes
        monthly_income = 1000
        NOT = { has_country_modifier = lost_hegemony }
    }
    base = {
        war_exhaustion = -0.1
    }
    scale = {
        global_trade_goods_size_modifier = 0.25
    }
    max = {
        governing_capacity_modifier = 0.20
    }
}
```

## common/custom_country_colors/mymod_colors.txt

```txt
num_symbols = 121

color = { 235 235 235 }
color = { 120 40 40 }
color = { 40 90 150 }
```

This file is a global palette for custom nations. Preserve `num_symbols` semantics and use RGB integers.

## common/country_colors/mymod_country_colors.txt

```txt
ABC = {
    color1= { 120 40 40 }
    color2= { 235 235 235 }
    color3= { 40 90 150 }
}
```

Country colors are keyed by tag and usually drive unit/banner palettes rather than map color.

## common/dynasty_colors/mymod_dynasty_colors.txt

```txt
color = { 248 232 115 }
color = { 18 28 55 }
color = { 229 127 132 }
```

Dynasty color files are simple ordered color pools. Preserve RGB integer format.

## common/custom_ideas/mymod_custom_ideas.txt

```txt
adm_idea_modifiers = {
    category = ADM

    mymod_custom_idea_reform = {
        reform_progress_growth = 0.05
        max_level = 2
        level_cost_1 = 5
        level_cost_2 = 20
        default = 1
        chance = {
            factor = 1
        }
    }
}
```

## common/graphicalculturetype.txt

```txt
westerngfx
mymod_westerngfx
```

This file is a flat key registry, not a keyed block. New graphical culture keys must match downstream country, culture, RNW, and unit-display references.

## common/government_names/mymod_government_names.txt

```txt
mymod_titles = {
    rank = {
        1 = MYMOD_DUCHY
        2 = MYMOD_KINGDOM
        3 = MYMOD_EMPIRE
    }
    ruler_male = {
        1 = MYMOD_DUKE
        2 = MYMOD_KING
        3 = MYMOD_EMPEROR
    }
    ruler_female = {
        1 = MYMOD_DUCHESS
        2 = MYMOD_QUEEN
        3 = MYMOD_EMPRESS
    }
    heir_male = {
        1 = HEIR
        2 = HEIR
        3 = HEIR
    }
    heir_female = {
        1 = HEIR_fem
        2 = HEIR_fem
        3 = HEIR_fem
    }
    trigger = {
        government = monarchy
        primary_culture = mymod_culture
    }
}
```

The first valid dynamic title can win, so keep specific entries above broad entries.

## common/client_states/mymod_client_states.txt

```txt
mymod_client_state = {
    region = north_german_region

    name = {
        name = "MYMOD_CLIENT_KINGDOM"
        trigger = {
            area = westphalia_area
            from = {
                government = monarchy
            }
        }
    }
}
```

## common/province_names/mymod_province_names.txt

```txt
236 = "MYMOD_LONDON_NAME"
237 = "MYMOD_OXFORD_NAME"
```

Some province-name files are simple province-ID-to-localisation mappings keyed by country/culture file. Other dynamic naming setups are more complex; compare the exact target format before mixing styles.

## common/government_ranks/mymod_government_ranks.txt

```txt
2 = {
    diplomats = 1
    governing_capacity = 200
    global_autonomy = -0.025
}

3 = {
    diplomats = 1
    governing_capacity = 400
    max_absolutism = 5
}
```

Government rank files are global. Avoid changing vanilla ranks unless the mod owns broad government balance.

## common/estates_preload/mymod_estates_preload.txt

```txt
mymod_estate_scholars = {
    modifier_definition = {
        type = loyalty
        key = mymod_scholars_loyalty_modifier
        trigger = {
            has_estate = mymod_estate_scholars
        }
    }
    modifier_definition = {
        type = influence
        key = mymod_scholars_influence_modifier
        trigger = {
            has_estate = mymod_estate_scholars
        }
    }
}
```

## common/opinion_modifiers/mymod_opinion.txt

```txt
mymod_saved_ally = {
    opinion = 40
    yearly_decay = 2
    max = 80
}
```

Do not remove or rename vanilla opinion keys marked as hardcoded.

## common/parliament_bribes/mymod_bribes.txt

```txt
mymod_administrative_support = {
    trigger = {
        has_reached_seat_threshold = no
    }

    effect = {
        adm_power_cost = 5
    }

    chance = {
        factor = 1
    }

    ai_will_do = {
        factor = 1
    }
}
```

Parliament bribes often call helper effects and may pass script fragments as strings. Keep the vanilla helper style when scaling costs by estate loyalty or debate state.

## common/subject_type_upgrades/mymod_subject_upgrades.txt

```txt
mymod_colonial_militia = {
    can_upgrade_trigger = {
        is_subject_of_type = crown_colony
        colonial_parent = {
            adm_power_cost = 25
        }
    }

    cost = 100

    effect = {
        colonial_parent = {
            adm_power_cost = 25
        }
    }

    modifier_overlord = {
        land_forcelimit = 5
    }

    modifier_subject = {
        liberty_desire = 10
        land_forcelimit = -5
    }
}
```

Subject-upgrade triggers are evaluated in subject scope with overlord as FROM; many colonial examples scope to `colonial_parent`.

## common/historial_lucky.txt

```txt
ABC = {
    NOT = {
        exists = XYZ
        is_year = 1700
    }
}

XYZ = {
    always = yes
}
```

This file is priority-ordered. Put specific replacements above broad always-valid entries.

## common/units_display/mymod_units_display.txt

```txt
infantry = {
    factor = 1
}

cavalry = {
    factor = 1
    modifier = {
        factor = 3
        tag = ABC
    }
}

artillery = {
    factor = 1
}
```

This influences display selection, not the combat stat definition in `common/units`.

## common/ai_army/mymod_ai_army.txt

```txt
province = {
    war = {
        active = {
            is_at_war = yes
        }
        eval_add = {
            factor = 0
            modifier = {
                factor = 100
                is_in_capital_area = yes
            }
        }
    }
}
```

Army AI chooses the province with the lowest value. Use `mapmode armyeval` and `ai_army_tick` for debugging.

## common/region_colors/mymod_region_colors.txt

```txt
color = { 181 72 106 }
color = { 220 138 57 }
color = { 63 125 214 }
```

Region colors are UI/map metadata. Preserve RGB integer format.

## common/alerts.txt

```txt
sound = {
    HIGH = "new_alert"
    MEDIUM = "new_alert"
    LOW = "new_alert"
}

icon = {
    HIGH = "GFX_alerticon_banner"
    MEDIUM = "GFX_alerticon_banner_med"
    LOW = "GFX_alerticon_banner_low"
}

alerts = {
    alert_low_maintenance = {
        category = HIGH
    }
}
```

Categories are `HIGH`, `MEDIUM`, `LOW`, or `DISABLED`. Adding a new alert key normally needs engine/UI support.

## common/achievements.txt

```txt
mymod_achievement_example = {
    id = 9999
    localization = MYMOD_ACHIEVEMENT_EXAMPLE

    possible = {
        ironman = yes
        start_date = 1444.11.11
    }

    happened = {
        tag = ABC
        owns = 236
    }
}
```

Achievements are checksum/platform-sensitive and usually unsuitable for ordinary gameplay mods.

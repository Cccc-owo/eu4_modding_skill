# Syntax Examples

This file is the first stop when writing EU4 mod files. Copy the closest example, then adapt keys, scopes, localisation, and asset paths.

## Mini Mod Skeleton

```txt
my_eu4_mod/
├── descriptor.mod
├── common/
│   ├── scripted_effects/mymod_effects.txt
│   ├── scripted_triggers/mymod_triggers.txt
│   ├── on_actions/mymod_on_actions.txt
│   ├── event_modifiers/mymod_modifiers.txt
│   ├── country_tags/mymod_countries.txt
│   ├── countries/My Country.txt
│   └── ideas/mymod_ideas.txt
├── decisions/mymod_decisions.txt
├── events/mymod_events.txt
├── missions/mymod_missions.txt
├── history/
│   ├── countries/ABC - My Country.txt
│   └── provinces/236 - London.txt
├── interface/mymod_sprites.gfx
├── gfx/interface/missions/mission_mymod.dds
├── gfx/flags/ABC.tga
└── localisation/mymod_l_english.yml
```

## descriptor.mod

```txt
name="My EU4 Mod"
path="mod/my_eu4_mod"
supported_version="1.37.*"
tags={
    "Gameplay"
    "Missions"
}
```

## events/mymod_events.txt

```txt
namespace = mymod

country_event = {
    id = mymod.1
    title = "mymod.1.t"
    desc = "mymod.1.d"
    picture = DIPLOMACY_eventPicture

    trigger = {
        tag = ENG
        is_at_war = no
        NOT = { has_country_flag = mymod_reform_debate_started }
    }

    mean_time_to_happen = {
        months = 120
        modifier = {
            factor = 0.5
            stability = 1
        }
        modifier = {
            factor = 2
            has_disaster = internal_conflicts
        }
    }

    immediate = {
        hidden_effect = {
            set_country_flag = mymod_reform_debate_started
        }
    }

    option = {
        name = "mymod.1.a"
        ai_chance = {
            factor = 70
            modifier = {
                factor = 2
                adm_power = 100
            }
        }
        add_prestige = 10
        add_country_modifier = {
            name = mymod_reform_momentum
            duration = 3650
        }
    }

    option = {
        name = "mymod.1.b"
        trigger = {
            treasury = 100
        }
        ai_chance = { factor = 30 }
        add_treasury = -100
        add_stability = 1
    }
}

province_event = {
    id = mymod.2
    title = "mymod.2.t"
    desc = "mymod.2.d"
    picture = CITY_VIEW_eventPicture
    is_triggered_only = yes

    option = {
        name = "mymod.2.a"
        add_base_tax = 1
        owner = {
            add_prestige = 5
        }
    }
}
```

## decisions/mymod_decisions.txt

```txt
country_decisions = {
    mymod_found_royal_academy = {
        major = yes
        potential = {
            normal_or_historical_nations = yes
            government = monarchy
            NOT = { has_country_flag = mymod_royal_academy_founded }
        }
        provinces_to_highlight = {
            OR = {
                province_id = 236
                area = home_counties_area
            }
            owned_by = ROOT
            is_state = yes
        }
        allow = {
            is_at_war = no
            adm_power = 100
            treasury = 200
            capital_scope = {
                is_state = yes
            }
        }
        effect = {
            add_adm_power = -100
            add_treasury = -200
            capital_scope = {
                add_province_modifier = {
                    name = mymod_royal_academy
                    duration = -1
                }
            }
            set_country_flag = mymod_royal_academy_founded
            country_event = { id = mymod.1 days = 30 }
        }
        ai_will_do = {
            factor = 1
            modifier = {
                factor = 0
                num_of_loans = 1
            }
        }
        ai_importance = 200
    }
}
```

## missions/mymod_missions.txt

```txt
mymod_eng_missions = {
    slot = 1
    generic = no
    ai = yes

    potential_on_load = {
        tag = ENG
    }
    potential = {
        tag = ENG
    }

    has_country_shield = yes

    mymod_secure_capital = {
        icon = mission_mymod_capital
        required_missions = { }
        position = 1

        provinces_to_highlight = {
            province_id = 236
            owned_by = ROOT
            NOT = { has_province_modifier = mymod_royal_academy }
        }

        trigger = {
            capital_scope = {
                is_state = yes
                prosperity = 100
            }
            stability = 1
        }

        effect = {
            capital_scope = {
                add_base_tax = 1
                add_base_production = 1
            }
            add_prestige = 10
        }

        ai_weight = { factor = 100 }
    }

    mymod_project_power = {
        icon = mission_build_up_to_force_limit
        required_missions = { mymod_secure_capital }
        position = 2

        trigger = {
            army_size_percentage = 1
            manpower_percentage = 0.6
        }

        effect = {
            add_army_tradition = 5
            random_owned_province = {
                limit = { is_state = yes }
                add_province_modifier = {
                    name = mymod_recruitment_drive
                    duration = 3650
                }
            }
        }
    }
}
```

## common/scripted_triggers/mymod_triggers.txt

```txt
mymod_is_stable_monarchy = {
    government = monarchy
    stability = 1
    legitimacy = 75
}

mymod_has_advisor_of_level = {
    OR = {
        treasurer = $LEVEL$
        statesman = $LEVEL$
        commandant = $LEVEL$
    }
}
```

Usage:

```txt
trigger = {
    mymod_is_stable_monarchy = yes
    mymod_has_advisor_of_level = { LEVEL = 2 }
}
```

## common/scripted_effects/mymod_effects.txt

```txt
mymod_reward_reform_effect = {
    add_prestige = $PRESTIGE$
    change_government_reform_progress = $REFORM_PROGRESS$
    if = {
        limit = { has_estate = estate_nobles }
        add_estate_loyalty = {
            estate = estate_nobles
            loyalty = 5
        }
    }
}

mymod_add_or_extend_province_modifier = {
    if = {
        limit = { has_province_modifier = $MODIFIER$ }
        extend_province_modifier = {
            name = $MODIFIER$
            duration = $DURATION$
        }
    }
    else = {
        add_province_modifier = {
            name = $MODIFIER$
            duration = $DURATION$
        }
    }
}
```

Usage:

```txt
effect = {
    mymod_reward_reform_effect = {
        PRESTIGE = 10
        REFORM_PROGRESS = 25
    }
}
```

## common/on_actions/mymod_on_actions.txt

```txt
on_startup = {
    events = {
        mymod.10
    }
    mymod_startup_cleanup_effect = yes
}

on_monarch_death = {
    if = {
        limit = {
            tag = ABC
            has_country_flag = mymod_dynasty_story_active
        }
        country_event = { id = mymod.20 days = 10 }
    }
}
```

Keep on_actions readable. Put large logic in scripted effects.

## common/event_modifiers/mymod_modifiers.txt

```txt
mymod_reform_momentum = {
    reform_progress_growth = 0.10
    global_unrest = -1
}

mymod_royal_academy = {
    local_development_cost = -0.05
    local_institution_spread = 0.20
}

mymod_recruitment_drive = {
    local_manpower_modifier = 0.25
    local_regiment_cost = -0.10
}
```

## common/country_tags/mymod_countries.txt

```txt
ABC = "countries/My Country.txt"
```

## common/countries/My Country.txt

```txt
graphical_culture = westerngfx

color = { 120 40 40 }
revolutionary_colors = { 4 7 10 }

historical_idea_groups = {
    defensive_ideas
    trade_ideas
    economic_ideas
}

historical_units = {
    western_medieval_infantry
    western_medieval_knights
}

monarch_names = {
    "Henry #0" = 20
    "Edward #0" = 20
}

leader_names = {
    "Talbot"
    "Neville"
}

ship_names = {
    "Saint George"
}

army_names = {
    "Army of $PROVINCE$"
}

fleet_names = {
    "Fleet of $PROVINCE$"
}
```

## history/countries/ABC - My Country.txt

```txt
government = monarchy
add_government_reform = feudalism_reform
mercantilism = 25
technology_group = western
primary_culture = english
religion = catholic
capital = 236
national_focus = ADM

1444.11.11 = {
    monarch = {
        name = "Henry"
        dynasty = "Lancaster"
        birth_date = 1421.12.6
        adm = 2
        dip = 2
        mil = 2
    }
    heir = {
        name = "Edward"
        dynasty = "Lancaster"
        birth_date = 1430.1.1
        claim = 75
        adm = 3
        dip = 3
        mil = 3
    }
}
```

## history/provinces/236 - London.txt

```txt
owner = ABC
controller = ABC
culture = english
religion = catholic
hre = no
base_tax = 7
base_production = 7
trade_goods = cloth
base_manpower = 6
capital = "London"
is_city = yes
add_core = ABC
discovered_by = western
center_of_trade = 2

1534.11.1 = {
    religion = protestant
}
```

## common/ideas/mymod_ideas.txt

```txt
mymod_ABC_ideas = {
    start = {
        discipline = 0.05
        trade_efficiency = 0.10
    }

    bonus = {
        global_tax_modifier = 0.10
    }

    trigger = {
        tag = ABC
    }
    free = yes

    abc_idea_1 = {
        infantry_power = 0.10
    }
    abc_idea_2 = {
        production_efficiency = 0.10
    }
    abc_idea_3 = {
        diplomatic_reputation = 1
    }
    abc_idea_4 = {
        global_manpower_modifier = 0.15
    }
    abc_idea_5 = {
        global_trade_power = 0.10
    }
    abc_idea_6 = {
        stability_cost_modifier = -0.10
    }
    abc_idea_7 = {
        land_morale = 0.10
    }
}
```

## common/religions/mymod_religion.txt

```txt
christian = {
    mymod_faith = {
        color = { 120 120 220 }
        icon = 20

        country = {
            tolerance_own = 1
            global_missionary_strength = 0.01
        }

        province = {
            local_missionary_strength = -0.01
        }

        allowed_conversion = {
            catholic
            protestant
        }

        heretic = { LOLLARD WALDENSIAN }
    }
}
```

Religion additions often need icons/UI/localisation and should be tested carefully.

## common/government_reforms/mymod_reforms.txt

```txt
mymod_civic_monarchy = {
    icon = government_monarchy
    monarchy = yes
    valid_for_nation_designer = yes
    allow_normal_conversion = yes

    potential = {
        government = monarchy
    }

    modifiers = {
        global_tax_modifier = 0.10
        reform_progress_growth = 0.05
    }
}
```

Government reform syntax varies by patch and tier system; compare against a current working reform when possible.

## common/tradenodes/mymod_trade.txt

```txt
mymod_node = {
    location = 236
    color = { 120 40 40 }

    members = {
        236 237 238
    }

    outgoing = {
        name = english_channel
        path = {
            236 237
        }
        control = {
            236 237
        }
    }
}
```

Trade node changes can break economy balance and map display. Keep province IDs valid.

## map snippets

`definition.csv`:

```csv
province;red;green;blue;x;x
5000;10;20;30;New Province;x
```

`default.map`:

```txt
max_provinces = 5001
definitions = "definition.csv"
provinces = "provinces.bmp"
positions = "positions.txt"
terrain = "terrain.bmp"
rivers = "rivers.bmp"
terrain_definition = "terrain.txt"
heightmap = "heightmap.bmp"
```

`area.txt`:

```txt
mymod_area = {
    5000 5001 5002
}
```

`region.txt`:

```txt
mymod_region = {
    areas = {
        mymod_area
    }
}
```

`superregion.txt`:

```txt
mymod_superregion = {
    mymod_region
}
```

## interface/mymod_sprites.gfx

```txt
spriteTypes = {
    spriteType = {
        name = "mission_mymod_capital"
        texturefile = "gfx//interface//missions//mission_mymod_capital.dds"
    }

    spriteType = {
        name = "GFX_mymod_button"
        texturefile = "gfx//interface//mymod_button.dds"
        noOfFrames = 1
    }
}
```

## interface/mymod_window.gui

```txt
guiTypes = {
    windowType = {
        name = "mymod_window"
        position = { x = 100 y = 100 }
        size = { x = 400 y = 200 }
        orientation = "UPPER_LEFT"
        moveable = 0

        iconType = {
            name = "mymod_background"
            spriteType = "GFX_mymod_button"
            position = { x = 0 y = 0 }
        }

        instantTextBoxType = {
            name = "mymod_title"
            position = { x = 20 y = 20 }
            font = "vic_18"
            text = "mymod_window_title"
            maxWidth = 360
            maxHeight = 30
        }
    }
}
```

Many windows are hardcoded; custom GUI support is limited and patch-dependent.

## music/songs.txt

```txt
song = {
    name = "mymod_theme"
    chance = {
        factor = 1
        modifier = {
            factor = 2
            tag = ABC
        }
    }
}
```

Pair song declarations with valid `.ogg` assets and any required `.asset` entries.

## sound/all_sounds.asset

```txt
sound = {
    name = "mymod_sound"
    file = "sound/mymod_sound.wav"
}
```

## localisation/mymod_l_english.yml

```yaml
l_english:
 mymod.1.t:0 "A Question Of Reform"
 mymod.1.d:0 "The realm debates whether old privileges should bend to new realities."
 mymod.1.a:0 "Reform with care."
 mymod.1.b:0 "Spend what is needed."
 mymod.2.t:0 "A Provincial Opportunity"
 mymod.2.d:0 "Local notables ask for support."
 mymod.2.a:0 "Invest locally."

 mymod_found_royal_academy_title:0 "Found a Royal Academy"
 mymod_found_royal_academy_desc:0 "Centralize elite education and train administrators for the crown."

 mymod_secure_capital_title:0 "Secure the Capital"
 mymod_secure_capital_desc:0 "A stable capital is the base of a stable state."
 mymod_project_power_title:0 "Project Power"
 mymod_project_power_desc:0 "Our army must match our ambitions."

 mymod_reform_momentum:0 "Reform Momentum"
 mymod_reform_momentum_desc:0 "The administration has found a rhythm for change."
 mymod_royal_academy:0 "Royal Academy"
 mymod_royal_academy_desc:0 "A permanent institution for training the state's servants."
 mymod_recruitment_drive:0 "Recruitment Drive"
 mymod_recruitment_drive_desc:0 "Local officers are recruiting with unusual energy."

 ABC:0 "My Country"
 ABC_ADJ:0 "My Country"
 mymod_ABC_ideas:0 "My Country Ideas"
 mymod_ABC_ideas_start:0 "My Country Traditions"
 mymod_ABC_ideas_bonus:0 "My Country Ambition"
 abc_idea_1:0 "Drilled Infantry"
 abc_idea_1_desc:0 "Our infantry trains to hold formation under pressure."
```

## run file: my_test.txt

```txt
if = {
    limit = {
        tag = ENG
        stability = 1
    }
    add_prestige = 1
    log = "mymod test: trigger true for [Root.GetName]"
}
else = {
    add_prestige = -1
    log = "mymod test: trigger false for [Root.GetName]"
}

```

Console:

```txt
run my_test.txt
```

Keep the blank final line.

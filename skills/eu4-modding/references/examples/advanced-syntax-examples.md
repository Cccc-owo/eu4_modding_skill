# Advanced Syntax Examples

Use these as copyable starting shapes for complex EU4 `common/` systems. Keep the object shape, then rename keys, replace IDs, add localisation, and compare against current vanilla when patch-specific behavior matters.

## common/disasters/mymod_disasters.txt

```txt
mymod_succession_crisis = {
    potential = {
        normal_or_historical_nations = yes
        government = monarchy
        NOT = { has_country_flag = mymod_succession_crisis_happened }
    }

    can_start = {
        has_any_disaster = no
        is_at_war = no
        legitimacy = 50
        has_heir = no
    }

    can_stop = {
        OR = {
            has_any_disaster = yes
            has_heir = yes
            legitimacy = 75
        }
    }

    progress = {
        modifier = {
            factor = 1
            NOT = { legitimacy = 75 }
        }
        modifier = {
            factor = 2
            stability = -1
        }
        modifier = {
            factor = 0.5
            has_estate = estate_nobles
            estate_loyalty = {
                estate = estate_nobles
                loyalty = 50
            }
        }
    }

    can_end = {
        legitimacy = 75
        NOT = { num_of_rebel_controlled_provinces = 1 }
    }

    modifier = {
        global_unrest = 3
        stability_cost_modifier = 0.10
    }

    on_start = mymod.100
    on_end = mymod.101

    on_monthly = {
        random_events = {
            900 = 0
            100 = mymod.102
        }
    }
}
```

Pair `on_start`, `on_end`, and monthly events with triggered-only event definitions.

## common/estate_privileges/mymod_privileges.txt

```txt
mymod_estate_nobles_military_charter = {
    icon = privilege_grant_autonomy
    land_share = 5
    loyalty = 0.05
    influence = 0.05
    max_absolutism = -5

    is_valid = {
        has_estate = estate_nobles
    }

    can_select = {
        is_at_war = no
        NOT = { has_estate_privilege = mymod_estate_nobles_military_charter }
    }

    can_revoke = {
        estate_loyalty = {
            estate = estate_nobles
            loyalty = 50
        }
    }

    on_granted = {
        add_estate_loyalty = {
            estate = estate_nobles
            loyalty = 5
        }
    }

    on_revoked = {
        add_estate_loyalty = {
            estate = estate_nobles
            loyalty = -10
        }
    }

    benefits = {
        land_forcelimit_modifier = 0.10
    }

    penalties = {
        global_tax_modifier = -0.05
    }

    conditional_modifier = {
        trigger = {
            army_tradition = 50
        }
        modifier = {
            army_tradition_decay = -0.005
        }
    }

    ai_will_do = {
        factor = 1
        modifier = {
            factor = 0
            NOT = { manpower_percentage = 0.5 }
        }
    }
}
```

## common/estate_agendas/mymod_agendas.txt

```txt
mymod_estate_nobles_fortify_border = {
    max_days_active = 7300

    can_select = {
        has_estate = estate_nobles
        any_owned_province = {
            fort_level = 1
            any_neighbor_province = {
                owner = {
                    is_rival = ROOT
                }
            }
        }
    }

    provinces_to_highlight = {
        owned_by = ROOT
        fort_level = 1
        any_neighbor_province = {
            owner = {
                is_rival = ROOT
            }
        }
    }

    selection_weight = {
        factor = 1
        modifier = {
            factor = 2
            any_rival_country = {
                any_owned_province = {
                    any_neighbor_province = {
                        owned_by = ROOT
                    }
                }
            }
        }
    }

    immediate_effect = {
        hidden_effect = {
            set_country_flag = mymod_nobles_fortify_border_agenda
        }
    }

    task_requirements = {
        any_owned_province = {
            fort_level = 2
            any_neighbor_province = {
                owner = {
                    is_rival = ROOT
                }
            }
        }
    }

    task_completed_effect = {
        add_estate_loyalty = {
            estate = estate_nobles
            loyalty = 10
        }
        add_mil_power = 50
        clr_country_flag = mymod_nobles_fortify_border_agenda
    }

    failing_effect = {
        add_estate_loyalty_modifier = {
            estate = estate_nobles
            desc = EST_VAL_AGENDA_DENIED
            loyalty = -5
            duration = 3650
        }
        clr_country_flag = mymod_nobles_fortify_border_agenda
    }

    invalid_trigger = {
        NOT = { has_estate = estate_nobles }
    }

    on_invalid = {
        clr_country_flag = mymod_nobles_fortify_border_agenda
    }
}
```

## common/policies/mymod_policies.txt

```txt
mymod_frontier_administration = {
    monarch_power = ADM

    potential = {
        has_idea_group = expansion_ideas
        has_idea_group = administrative_ideas
    }

    allow = {
        full_idea_group = expansion_ideas
        full_idea_group = administrative_ideas
    }

    global_autonomy = -0.03
    governing_capacity_modifier = 0.05

    effect = {
    }

    removed_effect = {
    }

    ai_will_do = {
        factor = 1
        modifier = {
            factor = 1.5
            governing_capacity_percentage = 0.8
        }
    }
}
```

## common/ages/mymod_ages.txt

```txt
age_of_discovery = {
    start = 1400

    can_start = {
        always = yes
    }

    objectives = {
        mymod_obj_secure_capital = {
            custom_trigger_tooltip = {
                tooltip = mymod_obj_secure_capital_tt
                capital_scope = {
                    fort_level = 2
                    prosperity = 100
                }
            }
        }
    }

    abilities = {
        mymod_ab_capital_administration = {
            effect = {
                set_country_flag = mymod_capital_administration_age_ability
            }
            modifier = {
                num_of_age_rewards = 1
                global_tax_modifier = 0.05
            }
            ai_will_do = {
                factor = 5
            }
        }
    }
}
```

Adding to a vanilla age may require overriding/merging a vanilla keyed block. Check load behavior carefully.

## common/cb_types/mymod_cb_types.txt

```txt
mymod_cb_border_war = {
    valid_for_subject = no

    is_triggered_only = yes
    months = 120

    prerequisites_self = {
        is_subject = no
    }

    prerequisites = {
        FROM = {
            is_neighbor_of = ROOT
        }
    }

    war_goal = mymod_take_border
}
```

Grant from an effect:

```txt
add_casus_belli = {
    type = mymod_cb_border_war
    target = FRA
    months = 120
}
```

## common/wargoal_types/mymod_wargoals.txt

```txt
mymod_take_border = {
    type = take_province

    attacker = {
        badboy_factor = 0.75
        prestige_factor = 1
        peace_cost_factor = 0.90

        allowed_provinces = {
            is_claim = ROOT
        }

        peace_options = {
            po_demand_provinces
        }

        prov_desc = ALL_CLAIMS
    }

    defender = {
        badboy_factor = 1
        prestige_factor = 1
        peace_cost_factor = 1

        peace_options = {
            po_demand_provinces
        }
    }

    war_name = CONQUEST_WAR_NAME
}
```

## common/subject_types/mymod_subject_types.txt

```txt
mymod_march_subject = {
    copy_from = march

    sprite = GFX_icon_march
    diplomacy_overlord_sprite = GFX_diplomacy_leadmarch
    diplomacy_subject_sprite = GFX_diplomacy_weakmarch

    can_be_annexed = yes
    pays_overlord = 0.25
    forcelimit_to_overlord = 0.20
    liberty_desire_development_ratio = 0.20
    restoration_cb = cb_disloyal_vassal

    scutage = yes
    divert_trade = yes
    seize_territory = yes

    modifier_subject = {
        modifier = march_subject
    }

    modifier_overlord = {
        modifier = vassal_subject
    }

    overlord_opinion_modifier = is_vassal
    subject_opinion_modifier = is_vassal
}
```

## common/institutions/mymod_institutions.txt

```txt
mymod_state_bureaucracy = {
    bonus = {
        governing_capacity_modifier = 0.05
    }

    history = {
        is_year = 1500
        owner = {
            adm_tech = 7
        }
    }

    can_start = {
        is_year = 1500
        development = 20
        owner = {
            stability = 1
            adm_tech = 7
        }
    }

    start_chance = 5
    on_start = institution_events.1

    can_embrace = {
        always = yes
    }

    embracement_speed = {
        modifier = {
            factor = 0.2
            scale = yes
            owner = {
                stability = 1
            }
        }
        modifier = {
            factor = 0.1
            scale = yes
            has_building = courthouse
        }
    }
}
```

Institution syntax is sensitive. For production mods, copy a current vanilla institution closest to your origin/spread model.

## common/great_projects/mymod_great_projects.txt

```txt
mymod_royal_academy_project = {
    start = 236
    date = 1444.11.11
    time = { months = 0 }
    build_cost = 0
    can_be_moved = no
    move_days_per_unit_distance = 7
    starting_tier = 1
    type = monument

    build_trigger = {
        owner = {
            primary_culture = english
        }
    }

    on_built = {
    }

    on_destroyed = {
    }

    can_use_modifiers_trigger = {
        owner = {
            primary_culture = english
        }
    }

    can_upgrade_trigger = {
        owner = {
            stability = 1
        }
    }

    keep_trigger = {
    }

    tier_0 = {
        upgrade_time = { months = 0 }
        cost_to_upgrade = { factor = 0 }
        province_modifiers = {
        }
        area_modifier = {
        }
        country_modifiers = {
        }
        on_upgraded = {
        }
    }

    tier_1 = {
        upgrade_time = { months = 120 }
        cost_to_upgrade = { factor = 1000 }
        province_modifiers = {
            local_development_cost = -0.05
        }
        area_modifier = {
        }
        country_modifiers = {
            reform_progress_growth = 0.05
        }
        on_upgraded = {
            owner = { add_prestige = 10 }
        }
    }

    tier_2 = {
        upgrade_time = { months = 120 }
        cost_to_upgrade = { factor = 2000 }
        province_modifiers = {
            local_development_cost = -0.10
        }
        area_modifier = {
            local_institution_spread = 0.10
        }
        country_modifiers = {
            reform_progress_growth = 0.10
        }
        on_upgraded = {
            owner = { add_prestige = 25 }
        }
    }

    tier_3 = {
        upgrade_time = { months = 120 }
        cost_to_upgrade = { factor = 4000 }
        province_modifiers = {
            local_development_cost = -0.15
        }
        area_modifier = {
            local_institution_spread = 0.20
        }
        country_modifiers = {
            reform_progress_growth = 0.15
        }
        on_upgraded = {
            owner = { add_prestige = 50 }
        }
    }
}
```

## common/triggered_modifiers/mymod_triggered_modifiers.txt

```txt
mymod_secure_trade_route = {
    potential = {
        num_of_ports = 1
    }

    trigger = {
        num_of_ports = 5
        has_merchant = yes
        trade_income_percentage = 0.25
    }

    trade_efficiency = 0.05
    global_trade_power = 0.05
}
```

## common/province_triggered_modifiers/mymod_province_triggered_modifiers.txt

```txt
mymod_capital_school = {
    potential = {
        is_city = yes
        owner = {
            has_country_flag = mymod_schools_enabled
        }
    }

    trigger = {
        is_capital = yes
        owner = {
            stability = 1
        }
    }

    local_development_cost = -0.05
    local_institution_spread = 0.10

    on_activation = {
        owner = { add_prestige = 1 }
    }

    on_deactivation = {
    }
}
```

Apply from an effect when needed:

```txt
capital_scope = {
    add_province_triggered_modifier = mymod_capital_school
}
```

## common/rebel_types/mymod_rebels.txt

```txt
mymod_reform_rebels = {
    color = { 120 60 60 }

    area = nation
    government = any
    defection = none
    independence = none
    unit_transfer = no
    gfx_type = culture_owner
    will_relocate = yes

    resilient = no
    reinforcing = yes
    general = yes
    smart = yes

    artillery = 0.0
    infantry = 0.8
    cavalry = 0.2
    morale = 1.0

    handle_action_negotiate = yes
    handle_action_stability = yes

    spawn_chance = {
        factor = 1
        modifier = {
            factor = 2
            owner = {
                has_country_modifier = mymod_reform_backlash
            }
        }
    }

    siege_won_trigger = {
        NOT = { local_autonomy = 50 }
    }

    siege_won_effect = {
        add_local_autonomy = 10
    }

    can_negotiate_trigger = {
        always = yes
    }

    can_enforce_trigger = {
        always = yes
    }

    demands_description = "mymod_reform_rebels_demand"

    demands_enforced_effect = {
        add_stability = -1
        add_country_modifier = {
            name = mymod_reform_backlash
            duration = 3650
        }
    }
}
```

Spawn from province scope:

```txt
spawn_rebels = {
    type = mymod_reform_rebels
    size = 2
}
```

## common/trade_companies/mymod_trade_companies.txt

```txt
mymod_trade_company_north_sea = {
    color = { 50 120 200 }

    provinces = {
        236 237 238
    }

    names = {
        trigger = {
            OR = {
                tag = ENG
                tag = GBR
            }
        }
        name = "MYMOD_TRADE_COMPANY_NORTH_SEA_ENGLISH"
    }

    names = {
        name = "MYMOD_TRADE_COMPANY_NORTH_SEA_GENERIC"
    }
}
```

## common/tradecompany_investments/mymod_investments.txt

```txt
mymod_company_school = {
    category = local_venture
    sprite = "GFX_mymod_company_school"
    upgrades_to = mymod_company_college
    cost = 200.0

    company_province_area_modifier = {
        local_institution_spread = 0.10
    }

    area_modifier = {
        local_development_cost = -0.02
    }

    ai_global_worth = { factor = 0 }
    ai_area_worth = { factor = 1 }
    ai_region_worth = { factor = 0 }
}
```

## common/holy_orders/mymod_holy_orders.txt

```txt
mymod_scholars = {
    icon = GFX_mymod_holy_order_scholars
    trigger = {
        religion_group = christian
        has_country_flag = mymod_enable_scholars
    }
    color = { 120 140 180 }
    cost = 50
    cost_type = adm_power
    per_province_effect = {
        add_base_tax = 1
    }
    per_province_abandon_effect = {
        add_base_tax = -1
        hidden_effect = {
            if = {
                limit = { NOT = { base_tax = 1 } }
                set_base_tax = 1
            }
        }
    }
    modifier = {
        local_institution_spread = 0.10
    }
    ai_priority = {
        factor = 1
    }
    localization = holy_order
}
```

## common/parliament_issues/mymod_parliament_issues.txt

```txt
mymod_fund_roadworks = {
    category = 3

    allow = {
        treasury = 100
    }

    effect = {
        add_treasury = -100
        random_owned_province = {
            limit = { is_state = yes }
            add_base_production = 1
        }
    }

    modifier = {
        build_cost = -0.05
    }

    chance = {
        factor = 1
        modifier = {
            factor = 2
            treasury = 300
        }
    }

    ai_will_do = {
        factor = 1
    }
}
```

## common/estate_crown_land/mymod_interactions.txt

```txt
interaction = {
    key = mymod_sell_charters
    random_seed = crown_land_share
    cooldown_months = 60

    trigger = {
        crown_land_share = 20
        NOT = { num_of_rebel_armies = 1 }
        hidden_trigger = { has_any_estates = yes }
    }

    effect = {
        add_treasury = 100
        add_estate_loyalty = {
            estate = all
            loyalty = 5
        }
        change_estate_land_share = {
            estate = all
            share = -5.0
        }
        set_country_flag = mymod_recent_charter_sale
    }

    ai_will_do = {
        factor = 1
        modifier = {
            factor = 0
            NOT = { num_of_loans = 1 }
        }
    }
}
```

## common/diplomatic_actions/mymod_diplomatic_actions.txt

```txt
declarewar = {
    condition = {
        tooltip = mymod_cannot_declare_reform_war
        potential = {
            has_country_flag = mymod_reform_peace_required
            FROM = { NOT = { overlord_of = ROOT } }
        }
        allow = {
            always = no
        }
    }
}
```

## common/new_diplomatic_actions/mymod_new_actions.txt

```txt
mymod_request_scholars = {
    category = influence
    alert_index = 56
    alert_tooltip = mymod_request_scholars_alert_tooltip
    require_acceptance = yes

    is_visible = {
        is_subject = no
        FROM = {
            is_subject = no
        }
    }

    is_allowed = {
        adm_power = 50
        FROM = {
            opinion = {
                who = ROOT
                value = 100
            }
        }
    }

    on_accept = {
        add_adm_power = -50
        FROM = {
            add_opinion = {
                who = ROOT
                modifier = mymod_sent_scholars
            }
        }
    }

    on_decline = {
        add_prestige = -5
    }

    ai_acceptance = {
        add_entry = {
            name = OPINION
            export_to_variable = {
                variable_name = ai_value
                value = opinion
                who = FROM
                with = ROOT
            }
            multiply_variable = {
                which = ai_value
                value = 0.2
            }
        }
    }

    ai_will_do = {
        always = no
    }
}
```

## common/peace_treaties/mymod_peace_treaties.txt

```txt
mymod_make_academy_subject = {
    category = 6
    power_cost_base = 1.0
    prestige_base = 0.1
    ae_base = 1.0

    warscore_cost = {
        no_provinces = 20.0
    }

    warscore_cap = 60
    is_make_subject = yes
    requires_is_allowed = yes

    is_visible = {
        always = yes
    }

    is_allowed = {
        FROM = {
            is_subject = no
        }
    }

    effect = {
        create_subject = {
            subject = FROM
            subject_type = mymod_march_subject
        }
    }

    ai_weight = {
        export_to_variable = {
            variable_name = ai_value
            value = 25
        }
        limit = {
            army_size_percentage = 1
        }
    }
}
```

Localise `mymod_make_academy_subject_desc`, `CB_ALLOWED_mymod_make_academy_subject`, and `PEACE_mymod_make_academy_subject`.

## common/scripted_functions/mymod_scripted_functions.txt

```txt
can_move_capital = {
    condition = {
        tooltip = mymod_cannot_move_capital_during_reform
        potential = {
            FROM = {
                has_country_flag = mymod_reform_peace_required
            }
        }
        allow = {
            always = no
        }
    }
}
```

## common/estate_crown_land/mymod_bonuses.txt

```txt
bonus = {
    key = mymod_crown_land_high
    range_from = 70.0
    range_to = 100.0
    modifier = {
        reform_progress_growth = 0.10
        global_autonomy = -0.01
    }
}
```

## common/custom_gui/mymod_custom_gui.txt

```txt
custom_button = {
    name = mymod_reform_button
    potential = {
        has_country_flag = mymod_show_reform_button
    }
    trigger = {
        adm_power = 50
    }
    effect = {
        add_adm_power = -50
        change_government_reform_progress = 25
    }
    tooltip = mymod_reform_button_tt
    frame = {
        number = 1
        trigger = { always = yes }
    }
}
```

Matching `.gui` object must have the same `name` and `scripted = yes`:

```txt
guiButtonType = {
    name = "mymod_reform_button"
    scripted = yes
    position = { x = 20 y = 20 }
    quadTextureSprite = "GFX_mymod_button"
}
```

## common/government_mechanics/mymod_mechanics.txt

```txt
mymod_reform_energy = {
    alert_icon_gfx = GFX_alerticons_government_mechanics
    alert_icon_index = 1

    available = {
        has_country_flag = mymod_reform_mechanic_enabled
    }

    powers = {
        mymod_reform_energy_power = {
            gui = government_shared_power
            min = 0
            max = 100
            default = 0
            reset_on_new_ruler = no
            base_monthly_growth = 0.5
            development_scaled_monthly_growth = 0

            scaled_modifier = {
                trigger = { always = yes }
                modifier = {
                    reform_progress_growth = 0.10
                }
                start_value = 0
                end_value = 100
            }
        }
    }

    interactions = {
        mymod_spend_reform_energy = {
            gui = government_interaction_type
            icon = GFX_mymod_button
            cost_type = mymod_reform_energy_power
            cost = 50
            trigger = {
                stability = 1
            }
            effect = {
                change_government_reform_progress = 50
            }
            cooldown_years = 5
            ai_chance = {
                factor = 1
            }
        }
    }
}
```

Government mechanics require UI support and generated modifier localisation/icons for `monthly_<power>` and `<power>_gain_modifier`.

# Events, Decisions, And Missions

Use this for scripted gameplay content.

## Events

Events need: unique ID, title, description, picture, trigger/triggering method, options, and optional immediate effects.

Choose root scope:

```txt
country_event = { ... }
province_event = { ... }
```

Country events have a country ROOT. Province events have a province ROOT. Either can still scope into provinces or countries.

### Standard Country Event

```txt
namespace = mymod

country_event = {
    id = mymod.1
    title = "mymod.1.t"
    desc = "mymod.1.d"
    picture = DIPLOMACY_eventPicture

    trigger = {
        tag = ENG
        NOT = { has_country_flag = mymod_seen_event }
    }

    mean_time_to_happen = {
        months = 120
        modifier = {
            factor = 0.5
            stability = 1
        }
    }

    immediate = {
        hidden_effect = {
            set_country_flag = mymod_seen_event
        }
    }

    option = {
        name = "mymod.1.a"
        ai_chance = { factor = 75 }
        add_prestige = 10
    }
    option = {
        name = "mymod.1.b"
        ai_chance = { factor = 25 }
        add_stability = -1
        add_treasury = 100
    }
}
```

### Triggered Event

Use `is_triggered_only = yes` for events fired by effects, decisions, missions, or on_actions:

```txt
country_event = {
    id = mymod.2
    title = "mymod.2.t"
    desc = "mymod.2.d"
    picture = COURT_eventPicture
    is_triggered_only = yes

    option = {
        name = "mymod.2.a"
        add_legitimacy = 10
    }
}
```

Fire it from an effect:

```txt
country_event = { id = mymod.2 days = 30 }
```

### Event Options

Options may include:

- `trigger = { ... }`: option visibility/availability.
- `highlight = { ... }`: province highlight.
- `goto = <province scope/id>`: where the option jumps.
- `ai_chance = { factor = N modifier = { ... } }`: AI weighting.
- Effects directly in the option body.

### Event Checklist

- Namespace is unique.
- ID is unique inside namespace.
- Root scope matches content.
- Triggered-only events do not also rely on MTTH.
- Every visible string is localised.
- Flags used to prevent repeats are set and checked consistently.
- Picture key exists in loaded `.gfx`.

## Decisions

Every decision file must wrap decisions:

```txt
country_decisions = {
    mymod_decision = {
        major = yes
        potential = {
            normal_or_historical_nations = yes
            tag = ENG
            NOT = { has_country_flag = mymod_decision_taken }
        }
        provinces_to_highlight = {
            OR = {
                province_id = 236
                area = home_counties_area
            }
            NOT = { owned_by = ROOT }
        }
        allow = {
            is_at_war = no
            owns_core_province = 236
        }
        effect = {
            add_prestige = 25
            set_country_flag = mymod_decision_taken
        }
        ai_will_do = {
            factor = 1
        }
        ai_importance = 200
    }
}
```

Decision sections:

- `potential`: whether the decision appears.
- `allow`: whether it can be clicked.
- `effect`: what happens after clicking.
- `provinces_to_highlight`: map UX, often broader or cleaner than `allow`.
- `ai_will_do`: AI probability/weight when available.
- `ai_importance`: AI priority for important decisions.
- `major = yes`: green/major decision styling.

Decision localisation:

```yaml
 mymod_decision_title:0 "Visible Decision Name"
 mymod_decision_desc:0 "Visible decision description."
```

## Missions

Missions are grouped into series. A series controls column slot, generic/specific status, potential, and country shield display.

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

    mymod_first_mission = {
        icon = mission_assemble_an_army
        required_missions = { }
        position = 1
        completed_by = 1500.1.1

        trigger = {
            army_size_percentage = 1
        }

        effect = {
            add_army_tradition = 5
        }

        ai_weight = {
            factor = 100
        }
    }
}
```

Mission rules:

- `slot`: column. Default UI has five columns.
- `generic = yes`: generic missions may be replaced by specific missions occupying the same slot/position.
- `position`: row. Script order also matters, but explicit position is clearer.
- `required_missions`: internal mission keys, not titles.
- `potential_on_load`: determines initial tree loading.
- `potential`: rechecked when mission trees are swapped.
- `swap_non_generic_missions = yes`: useful after tag changes or mission set changes.
- Mission/UI helper names are not guessable. Only reuse helper triggers/effects/preview hooks that you have confirmed in current vanilla or wiki-backed examples.

Mission highlights:

```txt
provinces_to_highlight = {
    OR = {
        province_id = 1856
        area = sussex_area
        region = italy_region
        colonial_region = colonial_florida
    }
    NOT = { country_or_non_sovereign_subject_holds = ROOT }
}
```

Mission localisation:

```yaml
 mymod_first_mission_title:0 "Build The Army"
 mymod_first_mission_desc:0 "Our ambitions require a force able to defend them."
```

Mission icons are sprite keys. If adding custom icons, define a `spriteType` in `.gfx` and put the DDS in the referenced path.

Do not invent mission-preview helpers, swap helpers, or hidden mission UI hooks from naming intuition. If the helper is not proven by a current example, do not use it.

## On Actions

On actions fire events/effects from engine hooks. Keep large logic in scripted effects:

```txt
on_startup = {
    events = {
        mymod.10
    }
    mymod_startup_effect = yes
}
```

## Common Pitfalls

- Missing `country_decisions = {}` wrapper.
- Decision can be taken repeatedly because no flag or changed condition prevents it.
- Mission tree hidden because `potential_on_load` is false.
- Mission dependency references a key in another hidden series.
- Triggered event lacks `is_triggered_only = yes`.
- Event option localisation missing.
- Province highlights use effects or unsupported scopes.

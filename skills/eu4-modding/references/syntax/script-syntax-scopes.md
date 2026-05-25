# Script Syntax And Scopes

EU4 uses Clausewitz-style script: nested key/value blocks, comments with `#`, and folder-specific object formats.

## Syntax Basics

```txt
stability = 1
capital_scope = {
    is_state = yes
}
```

Types commonly used:

- Boolean: `yes`, `no`
- Integer: `5`
- Float: `0.05`
- Date: `1444.11.11`
- Country tag: `ENG`
- Province ID: `236`
- Identifier: `catholic`, `cloth`, `western`
- String: `"Text with spaces"`

Comments:

```txt
# Comment outside quotes
name = "Text # this is part of the string"
```

## Game State Model

- Scopes select objects in game state.
- Triggers read game state and return true/false.
- Effects mutate game state.
- Modifiers are numeric properties attached to countries, provinces, ideas, religions, governments, etc.

## Trigger Context vs Effect Context

Trigger contexts include:

- `potential`, `allow`, `trigger`, `limit`
- decision `provinces_to_highlight`
- mission `trigger`
- event option `trigger`

Effect contexts include:

- `effect`, `immediate`, event option body
- scripted effects
- run files
- many on_action bodies

Do not mix them. `add_prestige` is an effect; `prestige = 50` is a trigger.

## Dynamic Scopes

- `ROOT`: receiver/base scope of the event, decision, mission, or effect.
- `FROM`: sender/caller/secondary scope.
- `PREV`: previous scope after scope changes.
- `THIS`: current scope, valid only in contexts that support it.

Examples:

```txt
FRA = {
    add_opinion = {
        who = ROOT
        modifier = mymod_opinion
    }
}

capital_scope = {
    add_base_tax = 1
}

every_owned_province = {
    limit = {
        region = italy_region
    }
    add_province_modifier = {
        name = mymod_modifier
        duration = 3650
    }
}
```

## Logic

`AND` is often implicit, but explicit blocks improve readability:

```txt
trigger = {
    OR = {
        religion = catholic
        religion = protestant
    }
    NOT = { is_year = 1500 }
}
```

## Effect Flow Control

```txt
if = {
    limit = { has_dlc = "Domination" }
    add_prestige = 10
}
else_if = {
    limit = { has_dlc = "Emperor" }
    add_prestige = 5
}
else = {
    add_prestige = 1
}
```

Useful effect tools:

- `hidden_effect = { ... }`: hides technical effects from tooltip.
- `custom_tooltip = mymod_tt`: show custom localisation.
- `random_owned_province = { limit = { ... } ... }`
- `every_owned_province = { limit = { ... } ... }`
- `random_list = { 50 = { ... } 50 = { ... } }`

## Scripted Triggers And Effects

Scripted triggers:

```txt
mymod_is_strong_england = {
    tag = ENG
    army_size_percentage = 1
}
```

Scripted effects:

```txt
mymod_reward_prestige_effect = {
    add_prestige = 10
    add_legitimacy = 5
}
```

Use arguments for reusable templates:

```txt
mymod_add_scaled_prestige = {
    add_prestige = $AMOUNT$
}

mymod_add_scaled_prestige = { AMOUNT = 10 }
```

Scripted trigger/effect arguments are text substitution, not typed parameters. `$ARG$` or `$CONTEXT$` content is pasted into the generated script before compilation. That means:

- argument names must match the called template exactly
- scope arguments must expand to a valid scope token such as `ROOT`, `FROM`, or a proven scope-switch effect
- malformed substitutions fail at parse/load time rather than at a friendly runtime boundary

Do not invent helper names or macro conventions by analogy. Copy the exact argument pattern from a current scripted trigger/effect example before introducing scope-bearing arguments.

## Flags, Event Targets, Variables

Flags:

```txt
set_country_flag = mymod_reform_done
has_country_flag = mymod_reform_done
clr_country_flag = mymod_reform_done
```

Event targets:

```txt
capital_scope = {
    save_event_target_as = mymod_capital
}
event_target:mymod_capital = {
    add_base_tax = 1
}
```

Variables are persistent numeric values attached to scopes. Use them for counters or accumulators when flags are not enough.

## Naming Discipline

Use a mod prefix for:

- namespaces and event IDs
- flags and event targets
- scripted triggers/effects
- modifiers
- missions and decisions
- variables
- localisation keys

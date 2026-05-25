# Countries, History, And Common Databases

Use this for new countries, province ownership, start history, idea groups, religions, reforms, modifiers, estates, trade, units, and other `common/` content.

## New Country Checklist

Required in most cases:

```txt
common/country_tags/zz_mymod_countries.txt
common/countries/My Country.txt
history/countries/ABC - My Country.txt
localisation/mymod_l_english.yml
gfx/flags/ABC.tga
```

Optional:

```txt
common/ideas/mymod_ideas.txt
missions/mymod_missions.txt
decisions/mymod_decisions.txt
events/mymod_events.txt
common/country_colors/mymod_country_colors.txt
common/dynasty_colors/mymod_dynasty_colors.txt
```

Country presentation support files:

- `common/country_colors/*.txt`: per-tag unit/banner palette with `color1`, `color2`, `color3`
- `common/dynasty_colors/*.txt`: ordered RGB pool for dynasty presentation

For scenario-style start dates or bookmark-heavy mods, also expect:

```txt
history/advisors/mymod_advisors.txt
history/diplomacy/mymod_diplomacy.txt
history/wars/mymod_wars.txt
common/bookmarks/mymod_bookmark.txt
```

## Country Tag

Use an unused 3-character uppercase tag:

```txt
ABC = "countries/My Country.txt"
```

Avoid tags used by vanilla or other active mods.

## Country File

Country files in `common/countries/` define visual and name pools:

```txt
graphical_culture = westerngfx

color = { 120 40 40 }
revolutionary_colors = { 4 7 10 }

historical_idea_groups = {
    defensive_ideas
    trade_ideas
}

historical_units = {
    western_medieval_infantry
    western_medieval_knights
}

monarch_names = {
    "Edward #0" = 20
}

leader_names = {
    "Talbot"
}

ship_names = {
    "Saint George"
}
```

## Country History

Country history in `history/countries/ABC - My Country.txt` sets start state and dated changes:

```txt
government = monarchy
add_government_reform = feudalism_reform
mercantilism = 25
technology_group = western
primary_culture = english
religion = catholic
capital = 236
national_focus = DIP

1444.11.11 = {
    monarch = {
        name = "Henry"
        dynasty = "Lancaster"
        adm = 2
        dip = 2
        mil = 2
    }
}
```

Common fields:

- `government`, `add_government_reform`, `technology_group`
- `primary_culture`, `add_accepted_culture`
- `religion`, `capital`, `national_focus`
- estate land share and privileges
- monarch, heir, queen, leader blocks
- dated changes in `YYYY.M.D = { ... }`

Related dated history files:

- `history/advisors/*.txt`: advisor pools tied to provinces, dates, and advisor types.
- `history/diplomacy/*.txt`: alliances, unions, vassals, marriages, truces, and empire setup.
- `history/wars/*.txt`: scripted historical wars with dated participant joins and removals.

## Province History

Province history defines ownership, development, trade goods, cores, and dated changes:

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

1534.11.1 = {
    religion = protestant
}
```

When changing ownership, check cores, missions, decisions, trade node membership, area/region membership, and subject relationships.

## Ideas

National ideas generally define traditions, seven ideas, and ambition:

```txt
mymod_ABC_ideas = {
    start = {
        discipline = 0.05
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
}
```

Localise idea group and individual ideas.

## Modifiers

Modifiers are reusable numeric blocks:

```txt
mymod_country_modifier = {
    land_morale = 0.05
    global_tax_modifier = 0.10
}
```

Apply through effects:

```txt
add_country_modifier = {
    name = mymod_country_modifier
    duration = 3650
}
```

Use `duration = -1` only when permanent behavior is intended.

## Religions, Cultures, Governments

These are large common databases. For new entries:

- Use unique keys.
- Provide colours/icons where required.
- Add localisation.
- Check every cross-reference: mechanics, UI, decisions, events, triggers, reforms.
- Be careful overriding vanilla groups, because many scripts reference group keys.

## On Actions And Scripted Helpers

Use `common/on_actions` to hook engine events. Keep complex code in:

- `common/scripted_triggers`
- `common/scripted_effects`

Scripted helpers reduce duplication and make logs easier to read.

# Localisation

EU4 localisation maps internal keys to visible text. Folder name must be `localisation`, not `localization`.

## File Format

English file names must end with `_l_english.yml`:

```txt
localisation/mymod_l_english.yml
```

File content:

```yaml
l_english:
 mymod_decision_title:0 "Visible Decision Name"
 mymod_decision_desc:0 "Visible decision description."
 mymod.1.t:0 "Event Title"
 mymod.1.d:0 "Event description."
 mymod.1.a:0 "Option text"
```

Rules:

- First line is exactly `l_english:` for English.
- Every key line starts with exactly one leading space.
- Key characters should be `A-Z`, `a-z`, `0-9`, `_`, `.`, `-`.
- Format is `<space><key>:<optional number><space>"Text"`.
- `#` outside quotes starts a comment.
- Newline inside visible text is `\n`.
- Backslash is `\\`.
- Deviation can cause the current and following keys to fail.

Encoding:

- Localisation files should be UTF-8 with BOM.
- Other EU4 text data files should use Windows-1252 / CP-1252 and should not have BOM.
- For non-localisation files, match the encoding and line-ending style of a current working file from the same folder type instead of introducing a new encoding guess.
- If you keep non-localisation files ASCII-only, they remain compatible with this rule.
- Do not re-encode an existing text file unless the user has confirmed that change; conversion can corrupt characters or parser-significant bytes.
- If unsure, copy a working vanilla/mod localisation file and edit it.

## Replace Folder

To intentionally override existing localisation keys, use:

```txt
localisation/replace/
```

For new keys, use normal `localisation/`.

## Common Key Conventions

Decisions:

```yaml
 mymod_decision_title:0 "Decision Title"
 mymod_decision_desc:0 "Decision description."
```

Events:

```yaml
 mymod.1.t:0 "Event Title"
 mymod.1.d:0 "Event description."
 mymod.1.a:0 "First option"
```

Missions:

```yaml
 mymod_first_mission_title:0 "Mission Title"
 mymod_first_mission_desc:0 "Mission description."
```

Modifiers:

```yaml
 mymod_modifier:0 "Modifier Name"
 mymod_modifier_desc:0 "Modifier description."
```

Countries:

```yaml
 ABC:0 "My Country"
 ABC_ADJ:0 "My Country adjective"
```

## Dynamic Text

Bracket commands can insert scoped data:

```yaml
 mymod_tt:0 "[Root.GetName] gains a claim on [From.GetName]."
```

Scope names are case sensitive. Common scopes: `Root`, `From`, `This`, `Prev`.

`@TAG` can show a country flag icon in localisation:

```yaml
 mymod_flag_text:0 "@ENG England"
```

## Customizable Localisation

Folder:

```txt
customizable_localization/
```

Customizable localisation uses `defined_text` blocks that select one localisation key from ordered trigger branches:

```txt
defined_text = {
    name = GetMymodRulerTitle

    text = {
        localisation_key = MYMOD_EMPRESS
        trigger = {
            is_female = yes
            government = monarchy
        }
    }
    text = {
        localisation_key = MYMOD_EMPEROR
        trigger = {
            government = monarchy
        }
    }
    text = {
        localisation_key = MYMOD_RULER
    }
}
```

Usage in normal localisation:

```yaml
 mymod_dynamic_tt:0 "The [Root.GetMymodRulerTitle] will review the charter."
```

Rules:

- `name` is the callable dynamic text key used by bracket syntax.
- `text = { ... }` entries are checked top to bottom; first valid branch wins.
- `localisation_key` points to a normal localisation key in `localisation/*.yml`.
- A trailing branch without `trigger` is the fallback and should usually exist.
- Triggers use normal Clausewitz trigger syntax and current scope rules.
- Empty output is possible with `localisation_key = ""`, but use it deliberately.

Dynamic mission descriptions often use preview-branch triggers such as `has_preview_missions_of_key_for_branch_in_batch`; copy a working vanilla shape before using mission-UI-specific helpers.

Do not derive new dynamic localisation helper names by analogy. A name like `GetMymod...` is safe only for your own `defined_text` entry; preview-branch or mission-UI helper triggers must match a proven existing helper exactly.

## Formatting

EU4 uses `§` formatting:

```yaml
 mymod_tt:0 "Gain §G10§! prestige and §Yclaims§!."
```

Common colour markers include `§Y` yellow, `§G` green, `§R` red, and `§!` reset.

## Coverage Review

Check every visible reference:

- Event `title`, `desc`, option `name`.
- Decision title/desc derived from decision key.
- Mission title/desc derived from mission key.
- Modifier names and descriptions.
- Custom tooltips.
- Country names/adjectives.
- Interface text, scripted GUI tooltips, icon labels.

Missing localisation appears as raw keys in game.

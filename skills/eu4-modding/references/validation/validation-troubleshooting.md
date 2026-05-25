# Validation And Troubleshooting

EU4 mod validation combines static review, logs, console commands, and reduced repros.

## Static Checklist

- Unique custom prefixes for keys.
- No accidental vanilla key override.
- Trigger blocks contain triggers only.
- Effect blocks contain effects only.
- Event IDs are namespaced.
- Decisions are inside `country_decisions = { ... }`.
- Mission dependencies point to existing visible mission keys.
- Localisation covers every visible string.
- Assets referenced by script are declared and exist.
- Country tags point to existing country files.
- Map province IDs are consistent across all map/history files.

## Logs

EU4 log directory:

```txt
Documents/Paradox Interactive/Europa Universalis IV/logs/
```

Important logs:

- `error.log`: non-fatal data/syntax issues. Start here.
- `exceptions.log`: fatal crash information.
- `game.log`: read this when using `log = "..."` output or when a scripted effect runs but data-load logs stay clean.

Logs are overwritten on launch. Capture the relevant run before relaunching.

Use Steam launch option:

```txt
-debug
```

## Console Testing

Useful commands:

```txt
event mymod.1 ENG
event mymod.2 236
run my_test.txt
observe
```

`event` tests events directly. `observe` lets AI run and exposes delayed crashes or bad AI behavior.

## Run Files

Run files are effect files read when executed, so they are excellent for fast iteration without restarting.

Locations:

```txt
Documents/Paradox Interactive/Europa Universalis IV/
Documents/Paradox Interactive/Europa Universalis IV/mod/<active mod folder>/
```

Console:

```txt
run my_test.txt
```

Run-file behavior:

- ROOT and FROM are the player country executing the file.
- Effects execute top to bottom.
- Leave a blank final line. If the last run-file effect appears to do nothing, suspect missing trailing newline before changing script logic.

Trigger test pattern:

```txt
if = {
    limit = {
        tag = ENG
    }
    add_prestige = 1
}
else = {
    add_prestige = -1
}
```

Variable/log pattern:

```txt
log = "My variable: [Root.my_var.GetValue]"
```

## Debugging Order

1. Reproduce with only the target mod enabled.
2. Read `error.log`.
3. If crash, read `exceptions.log`.
4. Identify load phase: databases/common, history, events, GUI, graphics, map.
5. Remove or disable recent files by folder until the failure disappears.
6. Reintroduce the smallest failing object.
7. Compare with a known-good template.
8. Fix references before changing design.

## Checksum

EU4 checksum is used for installation integrity and multiplayer compatibility. Treat checksum comparisons as meaningful only from a fresh main-menu launch state. If you see `XXXX` after entering single player and returning to menu, restart before using checksum as evidence.

## Known Fatal Patterns

- `country_tags` points to a missing country file.
- `default.map` province limit is lower than highest province ID.
- Map text references province IDs absent from `definition.csv`.
- Colonial region has no province.
- Bad meta-script/scripted-effect argument recursion causing stack overflow.
- Asset declaration references a missing file during graphics load.

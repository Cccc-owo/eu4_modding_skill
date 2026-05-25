# EU4 Modding Skill

EU4 modding skill pack. Focus: Clausewitz script, file placement, scope discipline, copyable examples, validation workflow. Target: EU4 `1.37`.

## Included Skill

- `eu4-modding`
  - Events, decisions, missions
  - `common/`, `history/`, `map/`, `localisation/`, `customizable_localization/`
  - Interface, graphics, music, sound
  - Wiki-backed command/modifier checking
  - Large example set, conservative scope checks

## Install

List repo skills:

```bash
npx skills add Cccc-owo/eu4_modding_skill --list
```

Install from GitHub:

```bash
npx skills add Cccc-owo/eu4_modding_skill --skill eu4-modding
```

Install for Codex, global:

```bash
npx skills add Cccc-owo/eu4_modding_skill --skill eu4-modding -a codex -g
```

Install from local clone:

```bash
npx skills add . --skill eu4-modding
```

## Use

Ask agent to use `eu4-modding` when working on EU4 mod files.

Best fit:

- New mod content
- Existing mod review/debug
- File/folder routing
- Scope and syntax sanity checks
- Vanilla-pattern lookup

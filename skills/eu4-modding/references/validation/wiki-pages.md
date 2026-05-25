# EU4 Wiki Pages

Use Playwright when direct `curl` or raw MediaWiki endpoints hit the Paradox client challenge. A headed Playwright session can pass the initial challenge and read page content.

## Verified Entry Points

- Main wiki: <https://eu4.paradoxwikis.com/Europa_Universalis_4_Wiki>
- Modding portal: <https://eu4.paradoxwikis.com/Modding>
- Mod structure: <https://eu4.paradoxwikis.com/Mod_structure>
- Scripting tutorial: <https://eu4.paradoxwikis.com/Scripting_Tutorial>
- Scopes: <https://eu4.paradoxwikis.com/Scopes>
- Triggers: <https://eu4.paradoxwikis.com/Triggers>
- Effects: <https://eu4.paradoxwikis.com/Effects>
- Variables: <https://eu4.paradoxwikis.com/Variables>
- Modifier list: <https://eu4.paradoxwikis.com/Modifier_list>
- Customizable localisation: <https://eu4.paradoxwikis.com/Customizable_localization>
- Event modding: <https://eu4.paradoxwikis.com/Event_modding>
- Decision modding: <https://eu4.paradoxwikis.com/Decision_modding>
- Mission modding: <https://eu4.paradoxwikis.com/Mission_modding>
- Country creation: <https://eu4.paradoxwikis.com/Country_creation>
- Scripted function modding: <https://eu4.paradoxwikis.com/Scripted_function_modding>
- Advisor modding: <https://eu4.paradoxwikis.com/Advisor_modding>
- Age modding: <https://eu4.paradoxwikis.com/Age_modding>
- Building modding: <https://eu4.paradoxwikis.com/Building_modding>
- Casus belli modding: <https://eu4.paradoxwikis.com/Casus_belli_modding>
- Colonial regions modding: <https://eu4.paradoxwikis.com/Colonial_regions_modding>
- Culture modding: <https://eu4.paradoxwikis.com/Culture_modding>
- Defines: <https://eu4.paradoxwikis.com/Defines>
- Diplomatic action modding: <https://eu4.paradoxwikis.com/Diplomatic_action_modding>
- Disaster modding: <https://eu4.paradoxwikis.com/Disaster_modding>
- Empire of China modding: <https://eu4.paradoxwikis.com/Empire_of_China_modding>
- Estate modding: <https://eu4.paradoxwikis.com/Estate_modding>
- Faction modding: <https://eu4.paradoxwikis.com/Faction_modding>
- Government modding: <https://eu4.paradoxwikis.com/Government_modding>
- Government mechanic modding: <https://eu4.paradoxwikis.com/Government_mechanic_modding>
- Great project modding: <https://eu4.paradoxwikis.com/Great_project_modding>
- History modding: <https://eu4.paradoxwikis.com/History_modding>
- Holy Roman Empire modding: <https://eu4.paradoxwikis.com/Holy_Roman_Empire_modding>
- Idea group modding: <https://eu4.paradoxwikis.com/Idea_group_modding>
- Institutions modding: <https://eu4.paradoxwikis.com/Institutions_modding>
- Mercenaries modding: <https://eu4.paradoxwikis.com/Mercenaries_modding>
- Modifier modding: <https://eu4.paradoxwikis.com/Modifier_modding>
- On Actions: <https://eu4.paradoxwikis.com/On_Actions>
- Parliament modding: <https://eu4.paradoxwikis.com/Parliament_modding>
- Peace treaty modding: <https://eu4.paradoxwikis.com/Peace_treaty_modding>
- Policy modding: <https://eu4.paradoxwikis.com/Policy_modding>
- Rebel modding: <https://eu4.paradoxwikis.com/Rebel_modding>
- Religion modding: <https://eu4.paradoxwikis.com/Religion_modding>
- Ruler personality modding: <https://eu4.paradoxwikis.com/Ruler_Personality_modding>
- Subject type modding: <https://eu4.paradoxwikis.com/Subject_type_modding>
- Technology modding: <https://eu4.paradoxwikis.com/Technology_modding>
- Trade company modding: <https://eu4.paradoxwikis.com/Trade_company_modding>
- Trade goods modding: <https://eu4.paradoxwikis.com/Trade_goods_modding>
- Unit modding: <https://eu4.paradoxwikis.com/Unit_modding>
- Scenario/bookmark modding: <https://eu4.paradoxwikis.com/Scenario_modding>
- Localisation: <https://eu4.paradoxwikis.com/Localisation>
- Map modding: <https://eu4.paradoxwikis.com/Map_modding>
- Map modding quick reference: <https://eu4.paradoxwikis.com/Map_Modding_Quick_Reference>
- Province modding: <https://eu4.paradoxwikis.com/Province_modding>
- Random New World modding: <https://eu4.paradoxwikis.com/Random_New_World_modding>
- Trade node modding: <https://eu4.paradoxwikis.com/Trade_node_modding>
- Interface modding: <https://eu4.paradoxwikis.com/Interface_modding>
- Graphical asset modding: <https://eu4.paradoxwikis.com/Graphical_asset_modding>
- Model modding: <https://eu4.paradoxwikis.com/Model_modding>
- Font modding: <https://eu4.paradoxwikis.com/Font_modding>
- Unit models: <https://eu4.paradoxwikis.com/Unit_models>
- Music modding: <https://eu4.paradoxwikis.com/Music_modding>
- Sound modding: <https://eu4.paradoxwikis.com/Sound_modding>
- Console commands: <https://eu4.paradoxwikis.com/Console_commands>
- Run files: <https://eu4.paradoxwikis.com/Run_files>
- Mod troubleshooting: <https://eu4.paradoxwikis.com/Mod_troubleshooting>
- Checksum: <https://eu4.paradoxwikis.com/Checksum>
- Clausewitz Scenario Editor: <https://eu4.paradoxwikis.com/Clausewitz_Scenario_Editor>
- EU4 Mod Editing Tool: <https://eu4.paradoxwikis.com/EU4_Mod_Editing_Tool>
- JoroDox mod making tool: <https://eu4.paradoxwikis.com/JoroDox_mod_making_tool>
- The Validator: <https://eu4.paradoxwikis.com/The_Validator>

## Playwright Pattern

```bash
export CODEX_HOME="${CODEX_HOME:-$HOME/.codex}"
export PWCLI="$CODEX_HOME/skills/playwright/scripts/playwright_cli.sh"
PLAYWRIGHT_CLI_SESSION=eu4wiki "$PWCLI" open https://eu4.paradoxwikis.com/Modding --headed
PLAYWRIGHT_CLI_SESSION=eu4wiki "$PWCLI" snapshot
PLAYWRIGHT_CLI_SESSION=eu4wiki "$PWCLI" eval "JSON.stringify({ title: document.title, text: document.querySelector('#mw-content-text')?.innerText.slice(0, 2000) })"
```

## How To Use Wiki Evidence

- Use the Triggers, Effects, Scopes, and Modifier list pages as the primary authority for whether a script command or modifier exists.
- Use wiki pages to understand engine concepts: scopes, trigger/effect context, mod folder purpose, map bitmap constraints, logs, checksum behavior.
- Some wiki pages explicitly mark older verification versions; treat local game files as stronger evidence when available.
- For a new feature, open the wiki page for the concept and then inspect at least one known-good vanilla or existing mod example if available.

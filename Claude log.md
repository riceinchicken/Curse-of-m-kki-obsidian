# Claude log

Shared memory for the Claude sessions working on the game ([repo](https://github.com/VHlearning3/Curse-of-Mokki)) — one on Otto's PC, one on Vili's PC. Its purpose is to stop the two from writing the same script twice or editing each other's work.

Related: [[Curse of the mökki GDD]] · [[Otto]] · [[Vili]]

## Rules for Claude

1. **Before starting work:** pull this vault (`git pull`) and read this note. Also pull the game repo.
2. **Before creating a script:** check the Script registry below. If the script (or the system it belongs to) is already listed or belongs to the other person's area, do not create it — use what exists, or tell the user.
3. **When you start something that will take a while:** add a row to In progress, commit and push the vault *before* writing code, so the other Claude sees the claim.
4. **After creating or changing a script:** add or update its row in the Script registry, add a line to the Session log, commit and push the vault.
5. **Never edit a script owned by the other person** without the user saying so. If a change is needed there, write it under Requests for the other side.
6. Keep entries short. Paths are relative to the game repo root.

## Who owns what

From the work plans. "Owner" means that person's Claude writes and edits those scripts.

| Area | Owner |
| --- | --- |
| Player movement (walk, sneak, sprint, hide) | Otto |
| Audio manager | Otto |
| First person camera | Otto |
| Cutscenes | Otto |
| Interaction mechanic | Otto |
| Stone throwing | Otto |
| Madness system / player HP | Vili |
| Inventory system / player HUD | Vili |
| Enemies (wolf, cultist) | Vili |
| NPC friend | Vili |
| Dialogue | Vili |
| Menus (main, pause, settings, death/end) | Vili |

## Script registry

Every game script, who made it, and what other scripts can rely on.

| Script | Owner | Purpose | Public API others may use | Updated |
| --- | --- | --- | --- | --- |
| *(none yet)* | | | | |

## In progress

| What | Who | Started |
| --- | --- | --- |
| *(nothing)* | | |

## Shared conventions

Decisions both sides must follow. Add here when one is made.

- Unity 6000.3.24f1, URP.
- Asset naming follows the GDD (`TypeAbbreviation_BaseName_AdditionalInfo`).
- *(script folder layout, namespaces, input system: not decided yet)*

## Requests for the other side

Things one Claude needs from the other person's scripts.

| From | To | Request | Status |
| --- | --- | --- | --- |
| *(none)* | | | |

## Session log

Newest first. One line per session: date, who, what was done.

- 2026-11-10 — Otto's Claude — Created this log. Game repo state: fresh Unity project, one commit by Vili ("Korjaus liike"), only `Assets/Scenes/SampleScene.unity` and Unity's TutorialInfo scripts. No game scripts yet.

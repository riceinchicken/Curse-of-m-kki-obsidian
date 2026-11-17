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

| Area                                        | Owner |
| ------------------------------------------- | ----- |
| Player movement (walk, sneak, sprint, hide) | Otto  |
| Audio manager                               | Otto  |
| First person camera                         | Otto  |
| Cutscenes                                   | Otto  |
| Interaction mechanic                        | Otto  |
| Stone throwing                              | Otto  |
| Madness system / player HP                  | Vili  |
| Inventory system / player HUD               | Vili  |
| Enemies (wolf, cultist)                     | Vili  |
| NPC friend                                  | Vili  |
| Dialogue                                    | Vili  |
| Menus (main, pause, settings, death/end)    | Vili  |
| Save Point System                           | Vili  |

## Script registry

Every game script, who made it, and what other scripts can rely on.

| Script | Owner | Purpose | Public API others may use | Updated |
| --- | --- | --- | --- | --- |
| `Assets/Scripts/Player/SmokingEffect.cs` | Otto | Test feature, visual only: press G to take a hit — placeholder joint (primitives) is raised to the mouth, ember glows, joint lowers, smoke puff, then green screen haze and FOV sway. Attach to the player camera. No gameplay effect, builds its own joint/canvas/particles at runtime; animation is code-driven, no .anim assets. | `Smoke()` takes a hit; `IsTakingHit` is true during the hand animation; `IsHigh` is true from the hit until the effect fades | 2026-11-10 |
| `Assets/Scripts/Madness/PlayerMadness.cs` | Vili | Madness = player HP (0 sane … 100 insane, Sanity = 100 − Madness). Hit at ≥ death threshold (80 %) kills, otherwise adds madness. Drain sources add madness at the depletion rate (never kills, stops at 100). Passive sanity recovery starts after a delay since madness last went up. All values editable in the Inspector. Put on the player. | `TakeHit()` / `TakeHit(amount)` (returns true if killed), `AddMadness`, `ReduceMadness`, `RecoverOverTime(amount, seconds)` (pills), `BeginDrain(source, multiplier)` / `EndDrain(source)` (enemy aura, hiding place), `SetMadness`, `ResetMadness`; read `Madness`, `Madness01`, `Sanity`, `IsInDangerZone`, `IsFullyInsane`, `IsDead`, `IsDraining`; events `OnMadnessChanged(float 0..1)`, `OnDangerZoneChanged(bool)`, `OnDied` | 2026-11-17 |
| `Assets/Scripts/Madness/MadnessDebug.cs` | Vili | TEST ONLY: on-screen madness bar + test keys H = hit, J = drain on/off, K = reset. Needs `PlayerMadness` on the same object. Remove from the real player. | none | 2026-11-17 |
| `Assets/Scripts/Madness/MadnessVignette.cs` | Vili | Madness camera effect, visual only (no audio). Creates its own global URP Volume at runtime: red edges (Vignette) from 50 % madness, stronger in the danger zone and building up over time while a drain is active (enemy aura, hiding place), fading when no drain and as sanity recovers; optional whole-image red tint and visual pulse. All values and on/off toggles in the Inspector. Reads `PlayerMadness`, doesn't touch the camera script. Camera needs Post Processing enabled. | none (reads `PlayerMadness`) | 2026-11-17 |

## In progress

| What | Who | Started |
| --- | --- | --- |
| `Assets/Scripts/Player/PlayerMovement.cs` (walk; sneak, sprint, hide follow this week) and `Assets/Scripts/Audio/AudioManager.cs` | Otto | 2026-11-17 |

## Shared conventions

Decisions both sides must follow. Add here when one is made.

- Unity 6000.3.24f1, URP.
- Asset naming follows the GDD (`TypeAbbreviation_BaseName_AdditionalInfo`).
- Input: the project uses the new Input System only (`activeInputHandler: 1`). Use `UnityEngine.InputSystem`, not `Input.GetKey`.
- Script folders (proposed by Otto's Claude, change if you disagree): `Assets/Scripts/<Area>/`, e.g. `Assets/Scripts/Player/`.
- Scenes: every team member works in their own scene (`Assets/Scenes/Otto.unity` for Otto; Vili and the others make their own). Don't edit another person's scene — two people saving the same `.unity` file causes merge conflicts.
- *(namespaces: not decided yet)*

## Requests for the other side

Things one Claude needs from the other person's scripts.

| From | To | Request | Status |
| --- | --- | --- | --- |
| Vili | Otto | When the first person player camera exists: enable **Post Processing** on it (Camera > Rendering > Post Processing), so `MadnessVignette` (red madness edges) shows. No code change needed in Otto's scripts. | open |

## Session log

Newest first. One line per session: date, who, what was done.

- 2026-11-17 — Vili's Claude — Made `MadnessVignette.cs` (red madness edges + optional tint and pulse, visual only). Compiles; 14 logic checks pass. Added it to `Player_MadnessTest` and enabled Post Processing on Main Camera in `Assets/Scenes/Vili scene.unity` only. Added a request for Otto: enable Post Processing on the player camera. Not committed to the game repo yet.
- 2026-11-17 — Vili's Claude — Made `PlayerMadness.cs` (madness = player HP + passive sanity recovery, per GDD Primary Mechanics) and `MadnessDebug.cs` (test overlay). Compiles; 14 logic checks pass in Unity (hit/death at 80 %, drain, recovery delay, pills API). Added test object `Player_MadnessTest` to `Assets/Scenes/Vili scene.unity` only. Not yet attached to a real player — when Otto's player exists, `PlayerMadness` goes on it. Otto's hide can later call `BeginDrain`/`EndDrain` (GDD: lose sanity in hiding place). Not committed to the game repo yet.
- 2026-11-17 — Vili's Claude — Setup check on Vili's PC: vault and game repo readable (git user.name is Vili). Unity MCP (official, com.unity.ai.assistant relay) connected and tested via Claude Desktop. Removed a stray "ok" line from the end of this note. No scripts made. Next on Vili's plan: Madness system / player HP.
- 2026-11-17 — Otto's Claude — Otto made his own scene `Assets/Scenes/Otto.unity` (not committed yet). Added the one-scene-per-person rule to Shared conventions. No scripts made.
- 2026-11-11 — Vili's Claude — Connected to this vault and the game repo on Vili's PC. No scripts made yet. On 2026-11-10: fixed `CLAUDE.md` in the game repo (both PCs use the user folder `Pelifarmi`, so the developer is now identified with `git config user.name`; added Vili's vault path), and synced [[Curse of the mökki GDD]] with the Google Doc (tunable values, animation lists, audio overview, SFX names). Four GDD images are still missing from `Images` (cave layout, texturing reference, forest and cave color palettes) and show as empty links until added.
- 2026-11-10 — Otto's Claude — Merged `otto/smoking-effect` into game repo `main` and pushed, together with Vili's commit "isohko kuutio testi" (scene change, CLAUDE.md update). `SmokingEffect.cs` is now on `main`.
- 2026-11-10 — Otto's Claude — `SmokingEffect.cs` tested by Otto in Unity (works), committed and pushed to game repo branch `otto/smoking-effect`. Not merged to `main` yet. The component is not saved on any camera in `SampleScene`; add it to the player camera to use it.
- 2026-11-10 — Otto's Claude — `SmokingEffect.cs`: added a placeholder joint held in view and a code-driven hit animation (raise, inhale, lower, exhale). Still on branch `otto/smoking-effect`, uncommitted, not tested in Unity by Claude.
- 2026-11-10 — Otto's Claude — Added `SmokingEffect.cs` (visual-only test feature) on game repo branch `otto/smoking-effect`. Not compiled or tested in Unity yet, not committed, not on `main`.
- 2026-11-10 — Otto's Claude — Created this log. Game repo state: fresh Unity project, one commit by Vili ("Korjaus liike"), only `Assets/Scenes/SampleScene.unity` and Unity's TutorialInfo scripts. No game scripts yet.

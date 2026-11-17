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
| Readable notes (part of Dialogue)           | Vili  |
| Interaction prompt ("Press E")              | Otto  |

## Script registry

Every game script, who made it, and what other scripts can rely on.

| Script | Owner | Purpose | Public API others may use | Updated |
| --- | --- | --- | --- | --- |
| `Assets/Scripts/Player/SmokingEffect.cs` | Otto | Test feature, visual only: press G to take a hit — placeholder joint (primitives) is raised to the mouth, ember glows, joint lowers, smoke puff, then green screen haze and FOV sway. Attach to the player camera. No gameplay effect, builds its own joint/canvas/particles at runtime; animation is code-driven, no .anim assets. | `Smoke()` takes a hit; `IsTakingHit` is true during the hand animation; `IsHigh` is true from the hit until the effect fades | 2026-11-10 |
| `Assets/Scripts/Player/PlayerMovement.cs` | Otto | Written by Otto himself (learning). WASD walking with `CharacterController`, normalized direction, gravity with grounded check. Reads `Keyboard.current` directly. Needs a `CharacterController` on the same object. Sneak, sprint and hide still to come. On branch `otto/audio-manager`, not on `main` yet. | None yet (no public members) | 2026-11-17 |
| `Assets/Scripts/Madness/PlayerMadness.cs` | Vili | Madness = player HP (0 sane … 100 insane, Sanity = 100 − Madness). Hit at ≥ death threshold (80 %) kills, otherwise adds madness. Drain sources add madness at the depletion rate (never kills, stops at 100). Passive sanity recovery starts after a delay since madness last went up. All values editable in the Inspector. Put on the player. | `TakeHit()` / `TakeHit(amount)` (returns true if killed), `AddMadness`, `ReduceMadness`, `RecoverOverTime(amount, seconds)` (pills), `BeginDrain(source, multiplier)` / `EndDrain(source)` (enemy aura, hiding place), `SetMadness`, `ResetMadness`; read `Madness`, `Madness01`, `Sanity`, `IsInDangerZone`, `IsFullyInsane`, `IsDead`, `IsDraining`; events `OnMadnessChanged(float 0..1)`, `OnDangerZoneChanged(bool)`, `OnDied` | 2026-11-17 |
| `Assets/Scripts/Madness/MadnessDebug.cs` | Vili | TEST ONLY: on-screen madness bar + test keys H = hit, J = drain on/off, K = reset. Needs `PlayerMadness` on the same object. Remove from the real player. | none | 2026-11-17 |
| `Assets/Scripts/Madness/MadnessVignette.cs` | Vili | Madness camera effect, visual only (no audio). Creates its own global URP Volume at runtime: red edges (Vignette) from 50 % madness, stronger in the danger zone and building up over time while a drain is active (enemy aura, hiding place), fading when no drain and as sanity recovers; optional whole-image red tint and visual pulse. All values and on/off toggles in the Inspector. Reads `PlayerMadness`, doesn't touch the camera script. Camera needs Post Processing enabled. | none (reads `PlayerMadness`) | 2026-11-17 |
| `Assets/Scripts/Inventory/ItemData.cs` | Vili | ScriptableObject for one kind of item (name, `UI_*` icon, stackable, max count, show in HUD). Create via Create > Curse of Mokki > Item. Assets in `Assets/Data/Items/`: `ITEM_Lantern`, `ITEM_Pills` (stack 5), `ITEM_Key_Test` (test key — make one ITEM_Key_* per door). | `DisplayName`, `Icon`, `Stackable`, `MaxCount`, `ShowInHUD` | 2026-11-17 |
| `Assets/Scripts/Inventory/PlayerInventory.cs` | Vili | Items the player carries (lantern, pills, keys; no stone slot for now). Put on the player. | `Add(item, amount=1)` (returns amount added), `Remove(item, amount=1)` (false if not enough), `Has(item, amount=1)`, `Count(item)`, `CanAdd(item)`, `Clear()`, `Slots`; events `OnItemAdded(item, n)`, `OnItemRemoved(item, n)`, `OnChanged` | 2026-11-17 |
| `Assets/Scripts/Inventory/PillUser.cs` | Vili | Q takes a pill: removes one `ITEM_Pills` and calls `PlayerMadness.RecoverOverTime` (30–50 sanity over 5–10 s, editable). Optional overdose (off). Reads Q directly for now — see question to Otto. | `TryUsePill()`; events `OnPillTaken(amount, seconds)`, `OnOverdose` | 2026-11-17 |
| `Assets/Scripts/HUD/PlayerHUD.cs` | Vili | Player HUD: item icons + counts and a short "+1 Pills" note, shown for a few seconds after any inventory change, then fades (all editable). Builds its own Screen Space Overlay canvas (TextMeshPro) at runtime. Madness has no HUD element (GDD) — see `MadnessVignette`. | `ShowNow()` | 2026-11-17 |
| `Assets/Scripts/Inventory/InventoryDebug.cs` | Vili | TEST ONLY: keys to add items (Vili scene: 1 lantern, 2 pills, 3 key; Left Alt + key removes). Remove from the real player. | none | 2026-11-17 |
| `Assets/Scripts/Menus/GameSettings.cs` | Vili | Static settings saved with PlayerPrefs (GDD: music + SFX volume and on/off). **Audio manager should read these.** | `MusicVolume`, `SfxVolume` (0..1), `MusicEnabled`, `SfxEnabled`, `EffectiveMusicVolume`, `EffectiveSfxVolume` (0 when off), event `OnChanged`, `Save()` | 2026-11-17 |
| `Assets/Scripts/Menus/MainMenu.cs` | Vili | Main menu: Play, Settings, Credits, Quit game. On prefab `Menu_MainMenu`. Test mode (default): Play just closes the menu; tick *Load Scene On Play* to load `MAIN SCENE`. Pauses time while open. | `Open()`, `Close()`, `Play()`, static `IsOpen`, `Instance`, `SkipOpenOnNextStart` | 2026-11-17 |
| `Assets/Scripts/Menus/PauseMenu.cs` | Vili | Pause menu, key **P**: Resume, Settings, Quit to main menu. On prefab `Menu_PauseMenu` (put in every game scene). Stops time (`Time.timeScale` 0) and frees the cursor while paused, restores both on resume. | static `IsPaused`, static event `OnPauseChanged(bool)`, `Pause()`, `Resume()` | 2026-11-17 |
| `Assets/Scripts/Menus/SettingsMenu.cs` | Vili | Settings panel (2 sliders + 2 toggles + Back) writing `GameSettings`. Prefab `Menu_Settings`, used inside both menus. | `Open()`, `Close()`, event `OnClosed` | 2026-11-17 |
| `Assets/Scripts/Menus/CreditsMenu.cs` | Vili | Credits panel; the text is a TMP object in prefab `Menu_Credits` (placeholder names). | `Open()`, `Close()`, event `OnClosed` | 2026-11-17 |
| `Assets/Scripts/Menus/DeathEndScreen.cs` | Vili | Death / end screen (GDD: Continue, Quit). Prefab `Menu_DeathEndScreen` (put in every game scene). Death: opens by itself on `PlayerMadness.OnDied` after a short delay; Continue restarts the scene for now (**→ save point respawn when the Save Point System exists**). End: Continue shows the credits, then the main menu. Quit (both) → main menu. Stops time and frees the cursor while shown; pause (P) is blocked. | `ShowEnd()` (**Otto: call at the end of the final cutscene**), `ShowDeath()`, static `IsOpen`, `CurrentMode` | 2026-11-17 |
| `Assets/Scripts/Menus/MenuUtility.cs` | Vili | Menu helpers: creates an EventSystem if the scene has none, loads scenes (works in Editor without Build Settings), quits. | `LoadScene(name)`, `ReloadCurrentScene(skipMainMenu)`, `QuitGame()`, `EnsureEventSystem()` | 2026-11-17 |

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
- UI text uses **TextMeshPro**. TMP Essential Resources imported by Vili's Claude on 2026-11-17 (`Assets/TextMesh Pro/`) — pull instead of importing again.
- Menu prefabs: `Assets/Art/Prefabs/UI/Menus/` (`Menu_Button` is nested in all menus — restyle it once). The GDD naming table has no prefab prefix yet; these use `Menu_`.
- *(namespaces: not decided yet)*

## Requests for the other side

Things one Claude needs from the other person's scripts.

| From | To | Request | Status |
| --- | --- | --- | --- |
| Vili | Otto | When the first person player camera exists: enable **Post Processing** on it (Camera > Rendering > Post Processing), so `MadnessVignette` (red madness edges) shows. No code change needed in Otto's scripts. | open |
| Vili | Otto | **Question — stones:** does your stone throwing keep track of the picked-up stone(s) itself? Vili's inventory has **no stone slot for now**. If you'd rather store stones in `PlayerInventory`, say so here. | open |
| Vili | Otto | **Question — Q key for pills:** OK to add a `UsePills` action (Q) to the shared `Assets/InputSystem_Actions.inputactions`, or should Vili's `PillUser` read the key directly with `Keyboard.current` (like `SmokingEffect` does with G)? For now `PillUser` reads Q directly; the key is changeable in the Inspector. | open |
| Vili | Otto | **Info — interaction:** for pickups use `PlayerInventory.Add(item)`; for locked doors check `PlayerInventory.Has(keyItem)` (and `Remove` if the key is used up). API in the Script registry once done. | info |
| Vili | Otto | **Pause (P):** while `PauseMenu.IsPaused` is true, mouse look and movement should stop. `Time.timeScale` is 0, but mouse-delta look usually ignores it — please check `PauseMenu.IsPaused` (or listen to `PauseMenu.OnPauseChanged`) in the camera/movement scripts. The pause menu frees the cursor and restores your lock state on resume. | open |
| Vili | Otto | **Audio settings:** when the audio manager exists, read volumes from `GameSettings.EffectiveMusicVolume` / `EffectiveSfxVolume` and listen to `GameSettings.OnChanged`. | open |
| Vili | Otto | **Question — P key:** same as the Q question: `PauseMenu` reads P directly with `Keyboard.current` for now. OK, or add a `Pause` action to `InputSystem_Actions`? | open |
| Vili | Otto | **Build Settings / MAIN SCENE:** the menus are tested in `Vili scene` only. Before the real game build, decide together: scene order in Build Settings (main menu first) and putting `Menu_PauseMenu` into `MAIN SCENE`. Vili's Claude won't edit `MAIN SCENE` or Build Settings until then. | open |
| Vili | Otto | **Cutscenes → end/death screen:** at the end of the final cutscene (5), call `FindFirstObjectByType<DeathEndScreen>().ShowEnd()`. Closet death (4): kill the player with `PlayerMadness.TakeHit()` (or tell Vili if the cutscene should open the death screen itself). | open |

## Session log

Newest first. One line per session: date, who, what was done.

- 2026-11-17 — Vili's Claude — Added the death/end screen: `DeathEndScreen.cs` + prefab `Menu_DeathEndScreen` (Continue, Quit), added to `Assets/Scenes/Vili scene.unity`. Small updates to `MainMenu.cs`, `PauseMenu.cs`, `MenuUtility.cs` (scene restart, pause blocked during the screen). Compiles; checks pass. Not committed to the game repo yet.
- 2026-11-17 — Vili's Claude — Made placeholder menus: `GameSettings`, `MainMenu`, `PauseMenu` (P), `SettingsMenu`, `CreditsMenu`, `MenuUtility` (`Assets/Scripts/Menus/`) and prefabs `Menu_Button`, `Menu_Settings`, `Menu_Credits`, `Menu_MainMenu`, `Menu_PauseMenu` (`Assets/Art/Prefabs/UI/Menus/`). Compiles; 15 logic checks pass. Added both menus + an EventSystem to `Assets/Scenes/Vili scene.unity` only — `MAIN SCENE` and Build Settings untouched. Added requests for Otto (pause vs mouse look, audio settings, P key, build order). Not committed to the game repo yet.
- 2026-11-17 — Vili's Claude — Vili committed and pushed today's work to game repo `main` (commit bf6f89e "Madness+MadnessVignette+Inventory prototype"): `Assets/Scripts/Madness/`, `Assets/Scripts/Inventory/`, `Assets/Scripts/HUD/`, `Assets/Data/Items/`, `Assets/TextMesh Pro/`, `Assets/Scenes/Vili scene.unity` (+ .meta). Checked: committed files match the tested versions. Small change before the commit: removed a note from the Overdose header in `PillUser.cs` (Inspector text only).
- 2026-11-17 — Otto's Claude — Committed and pushed `PlayerMovement.cs`, `Assets/Scenes/Otto.unity` and two test materials to game repo branch `otto/audio-manager` (commit b933e9c). Not merged to `main`. `AudioManager.cs` still local only, untested.
- 2026-11-17 — Otto's Claude — Otto wrote `Assets/Scripts/Player/PlayerMovement.cs` himself (WASD with `CharacterController`, normalized direction, gravity with grounded check; reads `Keyboard.current` directly; no public API yet). Works in `Otto.unity`. Sneak/sprint/hide not started. `Assets/Scripts/Audio/AudioManager.cs` drafted by Claude: untested, known bug (volume changes ignored after a music fade). All of this is uncommitted on local game repo branch `otto/audio-manager` — not on GitHub yet, so not in the Script registry yet.
- 2026-11-17 — Vili's Claude — Made inventory + player HUD: `ItemData`, `PlayerInventory`, `PillUser`, `InventoryDebug` (`Assets/Scripts/Inventory/`), `PlayerHUD` (`Assets/Scripts/HUD/`) and item assets in `Assets/Data/Items/`. Imported TMP Essential Resources (`Assets/TextMesh Pro/`). Compiles; logic checks pass. Components added to `Player_MadnessTest` in `Assets/Scenes/Vili scene.unity` only. Added ownership rows (notes = Vili/Dialogue, "Press E" prompt = Otto/Interaction) and questions for Otto (stones, Q key). Not committed to the game repo yet.
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

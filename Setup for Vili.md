# Setup for Vili

How to get your Claude using the same shared memory as Otto's Claude, so the two don't write the same scripts. Takes about five minutes.

The shared memory is the note [[Claude log]] in this vault. Both Claudes read it before working and write to it after.

## 1. Check the vault is syncing

You already have this vault open in Obsidian, so this is probably done.

- You should see the notes `Claude log` and `Setup for Vili` (this one). If not, press `Ctrl+P` and run **Git: Pull**.
- In **Settings → Git**, make sure the auto commit-and-sync interval, auto pull interval and pull on startup are set.

## 2. Find the vault's folder path

Claude needs the folder path on your PC.

- In Obsidian, right-click any note in the file list and choose **Show in system explorer**.
- Copy the folder path from the Explorer address bar (the folder that contains `Claude log.md`). You'll paste it in step 4.

## 3. Open Claude in the game project

- Start a Claude session with the game repo folder (`Curse-of-Mokki`, the one containing `Assets`) as the project folder.
- Pull the game repo first so you have the latest.

## 4. Paste this to your Claude

Replace `<VAULT PATH>` with the path from step 2, then send it as your first message:

```text
I'm Vili. I'm making the Unity game Curse of Mökki with Otto. Otto has his own Claude on his PC, and you two must not overlap.

Our shared memory is an Obsidian vault, a Git repo cloned on this PC at:
<VAULT PATH>

Do this now:
1. Ask me for access to that folder if you don't have it.
2. Run git pull in the vault, then read "Claude log.md" in it. Follow the rules at the top of that note from now on, in every session.
3. Also read "Curse of the mökki GDD.md" and "Work plans/Vili.md" in the vault for the design and my tasks.
4. Save to your memory: who I am, the vault path, and that you must read and update "Claude log.md" whenever you work on this game. If the game repo has no CLAUDE.md, tell me instead of creating one — Otto has one waiting to be merged.
5. Tell me in a few lines what the log says is mine, what is Otto's, and what is already made.

Rules in short: before creating any script, check the Script registry and ownership table in the log. Only work on my areas. After creating or changing a script, update the Script registry and Session log, then commit and push the vault. Never edit Otto's scripts — write a request in the log instead.
```

## 5. Check it worked

Your Claude should:

- ask for access to the vault folder (allow it),
- report that your areas are: madness system / player HP, inventory / HUD, enemies, NPC friend, dialogue and menus,
- report that Otto's areas are: player movement, audio manager, first person camera, cutscenes, interaction and stone throwing.

After its first piece of work, open [[Claude log]] in Obsidian. There should be a new line under **Session log** signed by your Claude, and any script it made should be in the **Script registry**. If the log didn't change, tell Claude: "update Claude log.md and push the vault".

## Day to day

- You don't need to paste the prompt again if Claude saved it to memory. If a new session seems unaware of the log, paste it again.
- If you and Otto agree to change who owns what, edit the **Who owns what** table in [[Claude log]] yourselves — both Claudes follow whatever it says.
- If your Claude needs something from one of Otto's scripts, it writes it under **Requests for the other side**; Otto's Claude picks it up next time it reads the log.

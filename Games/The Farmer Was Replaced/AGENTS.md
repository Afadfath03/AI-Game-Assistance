I want help writing, debugging, and optimizing drone-control scripts for The Farmer Was Replaced.

Source of Truth:
- Function signatures, tick/second costs, entity/item/unlock names, and syntax rules change between game updates — don't answer from memorized lists when precision matters (a specific tick-cost claim, an exact entity/item/unlock name, an error message's exact wording, or anything the user is currently stuck on). This applies to every number and name in this file too: treat them as examples, and verify against the documentation folder before quoting them to the user.
- In these cases, always ask the user for the whole documentation **folder**, never a single file — the game ships this locally at:
  `SteamLibrary/steamapps/common/The Farmer Was Replaced/TheFarmerWasReplaced_Data/StreamingAssets/Languages/<LANG>/` (e.g. `EN` for English)
  Asking for the folder (not one file) matters because answering a single question often needs cross-referencing more than one file inside it (e.g. a tick-cost question may also need the matching `docs/unlocks/*.md` page to explain why). Once the user shares the folder, the relevant files inside are:
  - `Strings/code_tooltips.txt` — every built-in function, exact tick/second cost, usage example.
  - `Strings/object_tooltips.txt`, `Strings/item_tooltips.txt`, `Strings/unlock_tooltips.txt` — current entities, grounds, items, unlocks.
  - `docs/scripting/*.md` — language syntax (for, while, if, functions, lists, dicts, sets, tuples, scopes, operators, import).
  - `docs/unlocks/*.md` — mechanics for a specific unlock (e.g. `mazes.md`, `pumpkins.md`, `polyculture.md`, `sunflowers.md`).
  - `Strings/parse_errors.txt` / `execute_errors.txt` — exact in-game error wording, useful when the user shares an error message or screenshot.
- The local documentation folder is authoritative. The public community wiki (thefarmerwasreplaced.wiki.gg) is a fallback only when the folder is unavailable; when you rely on it, say so in the response and flag the names as unconfirmed against the local docs.
- For general logic, structure, and strategy questions where exact wording doesn't matter, answer directly without asking for the folder.
- Once the user shares the folder, actually open and read the specific file(s) relevant to the question before answering — receiving the folder is not enough; the answer must reflect what's actually inside it, not a prior assumption of what it probably says.
- Assumptions about unlocks, farm size, or available entities/items from earlier in the conversation (or an earlier session) do not carry over automatically — progress differs per save and changes over time. Re-state them as assumptions at the start of a new task (see Response Format step 1) so the user can correct them.
- The scripting language is a restricted Python subset. `import` of the player's own other in-game files is supported; external/real Python libraries are not.

Correctness Rules (priority over optimization):
1. `harvest()` destroys whatever is under the drone if it isn't ready — always guard with `can_harvest()` unless destruction is intended.
2. `till()` toggles Soil ↔ Grassland — check `get_ground_type()` first unless toggling is the goal.
3. Traverse with `get_world_size()`, never a hardcoded grid size.
4. Don't assume an entity/item/unlock is available — check `num_unlocked()` in the script where it matters, and list it as an assumption in the response, since a fresh save and a late-game save have very different tech trees.

Optimization Priorities (in order):
1. Parallelize with `spawn_drone()` when multiple drones are relevant — this cuts real wall-clock time, which single-drone tick tuning can't match.
2. Never put functions that cost real seconds (unaffected by speed upgrades) in a production loop. Candidates: `do_a_flip()`, `print()`, `pet_the_piggy()`; use the zero-cost `quick_print()` for logging. Confirm which functions cost real seconds in `Strings/code_tooltips.txt` before stating it as fact.
3. Avoid redundant expensive actions (re-planting an occupied tile, re-tilling correct ground) by checking the matching cheap sense function first (e.g. the sense function for what is under the drone, `get_ground_type()` for the ground, `can_harvest()` for readiness; the worked example names the first one). Take exact costs from `Strings/code_tooltips.txt`; never quote tick numbers from memory.

Required Response Format:
1. State assumptions (farm size, origin, which unlocks/entities/drones are available, one-time vs indefinite loop) for everything the user didn't specify. If `my_progress.md` exists, base the assumptions on it and ask the user to confirm it is still accurate.
2. Full script in a fenced Python code block.
3. 2-4 sentences on traversal/parallelization strategy and correctness guards used — not a line-by-line walkthrough.
4. After the response, create or update `my_progress.md` in the same folder as this `AGENTS.md` file — i.e. `The Farmer Was Replaced/my_progress.md` (see Progress Tracking below).

Progress Tracking (my_progress.md):
- Location: always `The Farmer Was Replaced/my_progress.md`, next to this `AGENTS.md` — never a different folder, and never a per-topic or per-session filename.
- This file is personal to one save and is listed in the repo's `.gitignore`; don't commit it.
- This file exists to survive across sessions and prevent hallucination — since assumptions don't carry over automatically (see above), this is the explicit, checkable record instead of relying on chat memory.
- At the start of a task, check whether `my_progress.md` exists at that path. If it does, read it for the last known state and ask the user to confirm it is still accurate rather than assuming it is current, since progress changes as they play.
- If you cannot write files in this environment, output the full updated contents of `my_progress.md` in a fenced markdown block at the end of the response and tell the user to save it manually.
- Keep exactly two sections, overwriting the file each time rather than only appending:
  1. **Game Progress** — current unlocks, farm size/coordinate system in use, key entities/items/drones available. Overwrite outdated entries; this section reflects current state only, not history.
  2. **Chat-Built Scripts** — a running log of scripts produced so far: goal, key functions/logic used, and date/session if known. Append new entries; remove an entry only if the user says that script is no longer in use.
- Never invent an entry in either section — only record what the user confirmed or what was actually produced in this conversation.

Clarification Rule:
Ask before writing code only in these cases:
- Goal: must resolve to a concrete `action + entity` pair (e.g. "plant and harvest grass" = `plant(Entities.Grass)` + `harvest()` in a loop). If the user's goal is vague ("automate my farm", "make money"), ask which entity/entities and which actions (plant, harvest, till, water, swap...) before proceeding — don't infer a specific crop or action on their behalf.
- Fixed sub-area: if the user gives explicit dimensions (e.g. "3x3", "3x4") but no origin, ask whether the origin corner is the default `(0, 0)` or a custom coordinate (e.g. a 3x4 area starting at `(0, 5)`) — never assume `(0, 0)` for a custom-sized area without confirming.
- Exact details: if the request hinges on an exact function detail, entity/item name, or error message, ask the user to share the documentation folder from the path above rather than assuming.

Do not ask about anything else. Use these defaults and state them in Response Format step 1:
- Farm size: auto-scale to the whole farm via `get_world_size()`, origin `(0, 0)`.
- Drones: a single drone.
- Unlocks: only what the user or `my_progress.md` has stated; guard the rest with `num_unlocked()`.
- Run mode: an indefinite `while True` loop.

Worked Example (goal: continuously plant and harvest grass across the whole farm, 1 drone, no custom coordinates given):

**Assumptions:** single drone, `Entities.Grass` already unlocked, auto-scale via `get_world_size()` since the user didn't request a fixed sub-area, origin defaults to `(0, 0)`, indefinite loop.

```python
while True:
    for i in range(get_world_size()):
        if can_harvest():
            harvest()
        if get_entity_type() == None:
            plant(Entities.Grass)
        move(North)
    move(East)
```

**Function names in this example** (`get_entity_type()`, `can_harvest()`, `harvest()`, `plant()`, `move()`, `get_world_size()`) were checked against the public wiki (fallback source, see Source of Truth), not the local docs folder. The local folder remains authoritative: confirm them in `Strings/code_tooltips.txt` when the user shares it.

**Why:** `can_harvest()` guards `harvest()` so nothing unripe is destroyed; the `get_entity_type()` check plants only on empty tiles, so an already-planted tile is never re-planted; `get_world_size()` keeps the sweep valid if the farm expands later; `move(East)` wraps the drone back to column 0 automatically once it crosses the farm's edge. `Entities.Grass` is assumed unlocked, so no `num_unlocked()` guard appears here — if that unlock is not confirmed, guard the `plant()` call per Correctness rule 4 after checking its signature in `Strings/code_tooltips.txt`.

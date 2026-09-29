I want help writing, debugging, and optimizing drone-control scripts for The Farmer Was Replaced.

Source of Truth:
- Function signatures, tick/second costs, entity/item/unlock names, and syntax rules change between game updates — don't answer from memorized lists when precision matters (a specific tick-cost claim, an exact entity/item/unlock name, an error message's exact wording, or anything the user is currently stuck on).
- In these cases, always ask the user for the whole documentation **folder**, never a single file — the game ships this locally at:
  `SteamLibrary/steamapps/common/The Farmer Was Replaced/TheFarmerWasReplaced_Data/StreamingAssets/Languages/<LANG>/` (e.g. `EN` for English)
  Asking for the folder (not one file) matters because answering a single question often needs cross-referencing more than one file inside it (e.g. a tick-cost question may also need the matching `docs/unlocks/*.md` page to explain why). Once the user shares the folder, the relevant files inside are:
  - `Strings/code_tooltips.txt` — every built-in function, exact tick/second cost, usage example.
  - `Strings/object_tooltips.txt`, `Strings/item_tooltips.txt`, `Strings/unlock_tooltips.txt` — current entities, grounds, items, unlocks.
  - `docs/scripting/*.md` — language syntax (for, while, if, functions, lists, dicts, sets, tuples, scopes, operators, import).
  - `docs/unlocks/*.md` — mechanics for a specific unlock (e.g. `mazes.md`, `pumpkins.md`, `polyculture.md`, `sunflowers.md`).
  - `Strings/parse_errors.txt` / `execute_errors.txt` — exact in-game error wording, useful when the user shares an error message or screenshot.
- For general logic, structure, and strategy questions where exact wording doesn't matter, answer directly without asking for the folder.
- Once the user shares the folder, actually open and read the specific file(s) relevant to the question before answering — receiving the folder is not enough; the answer must reflect what's actually inside it, not a prior assumption of what it probably says.
- Assumptions about unlocks, farm size, or available entities/items from earlier in the conversation (or an earlier session) do not carry over automatically — progress differs per save and changes over time. Re-confirm current state via the Clarification Rule below at the start of a new task rather than reusing what was true last time.
- The scripting language is a restricted Python subset. `import` of the player's own other in-game files is supported; external/real Python libraries are not.

Correctness Rules (priority over optimization):
1. `harvest()` destroys whatever is under the drone if it isn't ready — always guard with `can_harvest()` unless destruction is intended.
2. `till()` toggles Soil ↔ Grassland — check `get_ground_type()` first unless toggling is the goal.
3. Traverse with `get_world_size()`, never a hardcoded grid size.
4. Don't assume an entity/item/unlock is available — check `num_unlocked()` or ask the user, since a fresh save and a late-game save have very different tech trees.

Optimization Priorities (in order):
1. Parallelize with `spawn_drone()` when multiple drones are relevant — this cuts real wall-clock time, which single-drone tick tuning can't match.
2. Never put `do_a_flip()`, `print()`, or `pet_the_piggy()` in a production loop — they cost real seconds, unaffected by speed upgrades (`quick_print()` is the 0-tick alternative for logging).
3. Avoid redundant 200-tick calls (re-planting an occupied tile, re-tilling correct ground) by checking the matching 1-tick sense function first.

Required Response Format:
1. State assumptions (farm size, which unlocks/entities/drones are available) if the user didn't specify them.
2. Full script in a fenced Python code block.
3. 2-4 sentences on traversal/parallelization strategy and correctness guards used — not a line-by-line walkthrough.
4. After the response, create or update `my_progress.md` in the same folder as this `AGENTS.md` file — i.e. `The Farmer Was Replaced/my_progress.md` (see Progress Tracking below).

Progress Tracking (my_progress.md):
- Location: always `The Farmer Was Replaced/my_progress.md`, next to this `AGENTS.md` — never a different folder, and never a per-topic or per-session filename.
- This file exists to survive across sessions and prevent hallucination — since assumptions don't carry over automatically (see above), this is the explicit, checkable record instead of relying on chat memory.
- At the start of a task, check whether `my_progress.md` exists at that path. If it does, read it for the last known state before running the Clarification Rule — but always confirm with the user that it's still accurate rather than assuming it's current, since progress changes as they play.
- Keep exactly two sections, overwriting the file each time rather than only appending:
  1. **Game Progress** — current unlocks, farm size/coordinate system in use, key entities/items/drones available. Overwrite outdated entries; this section reflects current state only, not history.
  2. **Chat-Built Scripts** — a running log of scripts produced so far: goal, key functions/logic used, and date/session if known. Append new entries; remove an entry only if the user says that script is no longer in use.
- Never invent an entry in either section — only record what the user confirmed or what was actually produced in this conversation.

Clarification Rule:
If not stated, ask before writing code:
- Goal: must resolve to a concrete `action + entity` pair before writing code (e.g. "plant and harvest grass" = `plant(Entities.Grass)` + `harvest()` in a loop). If the user's goal is vague ("automate my farm", "make money"), ask which entity/entities and which actions (plant, harvest, till, water, swap...) before proceeding — don't infer a specific crop or action on their behalf.
- Farm size and coordinate scope — ask explicitly which of these applies:
  1. Auto-scale to the whole farm via `get_world_size()` (default assumption if the user doesn't care about size).
  2. A fixed sub-area with explicit dimensions from the user (e.g. "3x3", "3x4").
  If (2), also ask whether the origin corner is the default `(0, 0)` or a custom coordinate the user specifies (e.g. a 3x4 area starting at `(0, 5)`) — never assume `(0, 0)` for a custom-sized area without confirming.
- Which unlocks/entities/items/drones are currently available?
- One-time setup script or an indefinite `while True` loop?
- If the request hinges on an exact function detail, entity/item name, or error message: ask the user to share the documentation folder from the path above rather than assuming.

Worked Example (goal: continuously plant and harvest grass across the whole farm, 1 drone, no custom coordinates given):

**Assumptions:** single drone, `Entities.Grass` already unlocked, auto-scale via `get_world_size()` since the user didn't request a fixed sub-area, origin defaults to `(0, 0)`.

```python
while True:
    for i in range(get_world_size()):
        if can_harvest():
            harvest()
            plant(Entities.Grass)
        move(North)
    move(East)
```

**Why:** `can_harvest()` guards the 200-tick `harvest()`/`plant()` pair so nothing is destroyed or re-planted needlessly; `get_world_size()` keeps the sweep valid if the farm expands later; `move(East)` wraps the drone back to column 0 automatically once it crosses the farm's edge.

# AI Game Assistance Agent

You have two jobs in this repo: helping users solve puzzles in various games, and maintaining the instruction files that make that possible.

## Request Types

Classify every request before acting:

1. **Game help**: the user wants to play, solve, or get help with a game. Follow "Game Help".
2. **Maintenance**: the user wants to change the repo itself: update a game's instructions, add a game, fix a rule, edit the README or this file. Follow "Maintenance".
3. **Unclear**: ask one short question to find out which of the two applies.

A game name in the request does not decide the type. "Help me with Beltmatic" is game help. "Update the Beltmatic instructions" is maintenance. When the user is doing maintenance, do not apply that game's response format to your reply.

## Game Help

- When the user mentions a specific game, read the corresponding instruction file at `Games/<GameName>/AGENTS.md` and follow its rules exactly.
- If no game is specified, list available games from the `Games/` directory and ask which one the user wants help with.
- Always follow the response format defined in each game's instruction file.
- Do not deviate from the rules stated in the game's instruction file.
- If you cannot read files in this environment, ask the user to paste or upload the game's `AGENTS.md` before helping.

## Maintenance

### Updating a game's instructions
1. Read the whole current `Games/<GameName>/AGENTS.md` first. Never edit from memory.
2. Change only what the user asked for. Keep the file's existing section names and order. Do not rewrite unrelated sections.
3. After the edit, check the file for contradictions between sections (for example a default that a clarification rule makes unreachable, or a rule that says "ask" next to one that says "assume").
4. If the change affects the worked example, recompute it exhaustively (by script when the environment allows) and update it so it obeys the new rules. Check the example's table against the row count, row order, and status rules, not only the arithmetic.
5. Report what changed, and list any contradictions you found but did not fix.

### Adding a game
1. Create `Games/<GameName>/AGENTS.md`. Use the game's exact name as the folder name.
2. Add a row to the alias table in "Game Name Matching" below.
3. Add a row to the Supported Games table in `README.md`.
4. Add an entry to `CHANGELOG.md`.
5. Anything about the game you do not know (rules, function names, costs, version-specific details) is written as unknown or as "verify against source", never invented.

### Editing root files
- `README.md`: keep the structure tree, usage steps, and Supported Games table in sync with what exists under `Games/`.
- `CLAUDE.md`: must contain only `@AGENTS.md`. Change instructions here, not there.
- `.gitignore`: personal progress files (`Games/*/my_progress.md`) stay ignored. Never commit them.
- This file: when adding a new kind of request, add it to "Request Types" as well.

### Every maintenance change
- Add a line to `CHANGELOG.md` under `Unreleased`. Do not invent dates or version numbers.
- If the request is ambiguous about which file or which rule it targets, ask one clarifying question before editing.

## Game Name Matching

Match the game name case-insensitively and ignoring spaces, using the folder name under `Games/` as the canonical name. Also accept these aliases:

| Game | Folder | Aliases |
|---|---|---|
| Beltmatic | `Games/Beltmatic/` | none |
| The Farmer Was Replaced | `Games/The Farmer Was Replaced/` | Farmer, TFWR |

If a mention could match more than one game, ask which one the user means.

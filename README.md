# AI Game Assistance

A collection of AI prompts and instructions designed to help solve puzzles across various games.

## Structure

```
README.md
AGENTS.md
CLAUDE.md
CHANGELOG.md
.gitignore
Games/
└── <GameName>/
    ├── AGENTS.md        (instructions set for AI)
    └── my_progress.md   (optional, git-ignored)
```

Each folder under `Games/` represents a supported game. The `AGENTS.md` file contains the rules, optimality criteria, and response format that an AI agent automatically reads when assisting with that game.

## Usage

1. Mention the game name in your conversation with the AI (e.g., "Help me with Beltmatic")
2. The AI automatically reads the corresponding `Games/<GameName>/AGENTS.md` and follows its rules
3. Follow the prompts — the AI will ask for the required parameters for that game (for Beltmatic: target number, allowed base numbers, allowed operations; for The Farmer Was Replaced: the goal as an action plus an entity)

## How It Works

The root `AGENTS.md` acts as a router. When a game is mentioned, the AI reads that game's `AGENTS.md` file and applies its specific rules and response format. Game names are matched case-insensitively, with aliases listed in the root `AGENTS.md`. The same file also tells the AI how to handle maintenance requests (updating a game's instructions, adding a game, editing root files), so those follow the same rules every time.

## Tool Compatibility

- **OpenCode** reads `AGENTS.md` natively ([OpenCode](https://github.com/anomalyco/opencode/)).
- **Claude Code** reads `CLAUDE.md`, not `AGENTS.md`. The included `CLAUDE.md` imports `AGENTS.md` so both stay in sync.
- **Chat-only tools without file access**: paste or upload the relevant game's `AGENTS.md` at the start of the conversation.

## Supported Games

| Game | Description |
|------|-------------|
| [Beltmatic](Games/Beltmatic/) | Math puzzle — find the most optimal number combination to reach a target using user-defined operations and base numbers |
| [The Farmer Was Replaced](Games/The%20Farmer%20Was%20Replaced/) | Programming game — write, debug, and optimize drone-control scripts, with a per-save progress file (`my_progress.md`, git-ignored) |

## Notes

- `Games/*/my_progress.md` holds personal save progress and is excluded from git.

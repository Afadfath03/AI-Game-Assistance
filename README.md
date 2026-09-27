# AI Game Assistance

A collection of AI prompts and instructions designed to help solve puzzles across various games.

## Structure

```
Games/
└── <GameName>/
    └── AGENTS.md
```

Each folder under `Games/` represents a supported game. The `AGENTS.md` file contains the rules, optimality criteria, and response format that an AI agent automatically reads when assisting with that game.

## Usage

1. Mention the game name in your conversation with the AI (e.g., "Help me with Beltmatic")
2. The AI automatically reads the corresponding `Games/<GameName>/AGENTS.md` and follows its rules
3. Follow the prompts — the AI will ask for the required parameters (target number, allowed base numbers, allowed operations)

## How It Works

The root `AGENTS.md` acts as a router. When a game is mentioned, the AI reads that game's `AGENTS.md` file and applies its specific rules and response format. This convention is natively supported by [OpenCode](https://github.com/opencode-ai/opencode) and can be adapted to other AI tools that support per-directory agent instructions.

## Supported Games

| Game | Description |
|------|-------------|
| [Beltmatic](Games/Beltmatic/) | Math puzzle — find the most optimal number combination to reach a target using user-defined operations and base numbers |

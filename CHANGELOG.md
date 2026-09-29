# Changelog

## Unreleased

- Added `CLAUDE.md` importing `AGENTS.md`, so Claude Code reads the same instructions.
- Added `.gitignore` entry for `Games/*/my_progress.md`.
- Root `AGENTS.md`: added game name matching with aliases and a fallback for environments without file access; added request types (game help, maintenance, unclear) and maintenance rules for updating a game's instructions, adding a game, and editing root files.
- Beltmatic: removed the contradiction between `BEST (OTHER FORM)` and the no-rearrangement rule; defined how the operation hierarchy and tie-breakers combine; made defaults explicit; clarified parentheses in the Combination column.
- The Farmer Was Replaced: resolved ask-vs-assume conflict; removed hardcoded tick costs; fixed the example script so empty tiles get planted; added a no-file-access fallback for `my_progress.md`.
- README: added The Farmer Was Replaced to Supported Games and a note on tool compatibility.
- Beltmatic worked example: table now lists all valid rows up to the 5-row maximum (added 3×3+3 and (3+3)×2), checked by exhaustive search.
- The Farmer Was Replaced worked example: function names verified against the public wiki and flagged for confirmation against the local docs folder; `get_ground_type()` and `can_harvest()` named as sense functions in Optimization Priorities.
- README: structure tree now lists `README.md`, `CHANGELOG.md`, `.gitignore`, and the optional `my_progress.md`.
- Root `AGENTS.md`: worked-example recheck now requires an exhaustive recompute and a table-rule check.
- The Farmer Was Replaced: added a Source of Truth rule making the local docs folder authoritative and the public wiki a fallback only; Optimization Priorities no longer names `get_entity_type()` (only the worked example does, with its caveat next to it).
- The Farmer Was Replaced worked example: the Why paragraph now explains why no `num_unlocked()` guard is needed and when one is required, instead of leaving the guard rule unmodelled in the file's only concrete script.

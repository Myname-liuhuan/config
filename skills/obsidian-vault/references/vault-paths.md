# Vault Root Paths — Per Platform

`SKILL.md` detects the current platform at runtime from the system prompt's `Platform` field and picks the matching row. **This file is a path reference table, not state** — there is no "active" marker to flip; the model picks the right row based on the detected platform.

| Platform | Vault Root              |
|----------|-------------------------|
| Windows  | `D:/file/obsidian/`     |
| macOS    | `~/Documents/obsidian/` |
| Linux    | *(TBD)*                 |

## Conventions

- Use **forward slashes** (`/`) in paths even on Windows so the values work across tools (`Glob`, `Grep`, etc.).
- Quote paths with spaces if needed.
- Keep the trailing slash off — `<vault-root>/designs/` not `<vault-root>/designs/` as a *root* value (the folder separator belongs to the joined path, not the root).
- Prefer `~` (or `$HOME`) over hard-coding the absolute path when the platform supports it.

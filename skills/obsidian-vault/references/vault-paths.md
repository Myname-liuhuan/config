# Vault Root Paths — Per Platform

This file is the **single source of truth** for the user's Obsidian vault root on each platform. Edit here when adding or updating a platform path — `SKILL.md` reads from this file via reference.

## Status

| Platform | Vault Root                          | Status    | Notes                          |
|----------|-------------------------------------|-----------|--------------------------------|
| macOS    | `~/Documents/obsidian/`              | ✅ Active | Current working platform        |
| Windows  | `D:/file/obsidian/`                | 🔁 Alt    | Secondary; synced from laptop  |
| Linux    | *(TBD)*                             | ⏳ Pending | —                              |

The **Active** row in this table is what `SKILL.md` should treat as the current platform root. Reorder so the active row is at the top whenever status changes.

## How to switch the active platform

1. Fill the `<Platform>` cell with the absolute path to the vault root (forward slashes, no trailing slash).
2. Flip the status to ✅ Active and move it to the top of the table.
3. Update `SKILL.md` "Vault Root (current platform)" section so only the row marked ✅ Active appears there.
4. Don't duplicate a single vault across multiple platforms via symlinks — keep one physical vault root per platform, or document symlinks explicitly here.

## Conventions

- Use **forward slashes** (`/`) in paths even on Windows so the values work across tools (`Glob`, `Grep`, etc.).
- Quote paths with spaces if needed.
- Keep the trailing slash off — `<vault-root>/designs/` not `<vault-root>/designs/` as a *root* value (the folder separator belongs to the joined path, not the root).

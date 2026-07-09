# Vault Root Paths — Per Platform

This file is the **single source of truth** for the user's Obsidian vault root on each platform. Edit here when adding or updating a platform path — `SKILL.md` reads from this file via reference.

## Status

| Platform | Vault Root | Status   | Notes                          |
|----------|------------|----------|--------------------------------|
| Windows  | `D:/file/obsidian/` | ✅ Active | Current working platform |
| macOS    | *(TBD)*    | ⏳ Pending | —                              |
| Linux    | *(TBD)*    | ⏳ Pending | —                              |

## How to add a new platform

1. Fill the `<Platform>` cell with the absolute path to the vault root (forward slashes, no trailing slash).
2. Flip the status to ✅ Active and move it to the top of the table.
3. Update `SKILL.md` "Vault Root (current platform)" section if the *currently active platform* changed (only the row marked ✅ Active in the table above counts as "current").
4. Don't duplicate a single vault across multiple platforms via symlinks — keep one physical vault root per platform, or document symlinks explicitly here.

## Conventions

- Use **forward slashes** (`/`) in paths even on Windows so the values work across tools (`Glob`, `Grep`, etc.).
- Quote paths with spaces if needed.
- Keep the trailing slash off — `<vault-root>/designs/` not `<vault-root>/designs/` as a *root* value (the folder separator belongs to the joined path, not the root).

## Example when filled in

| Platform | Vault Root                          | Status   |
|----------|-------------------------------------|----------|
| Windows  | `D:/file/obsidian/`                 | ✅ Active |
| macOS    | `/Users/<user>/Documents/obsidian/`  | ✅ Active |
| Linux    | `/home/<user>/notes/obsidian/`      | ✅ Active |

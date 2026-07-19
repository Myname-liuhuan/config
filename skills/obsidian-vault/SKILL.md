---
name: obsidian-vault
description: Use when the user wants to find, read, write, save, or update notes in their personal Obsidian knowledge base vault. Triggers on phrases like "笔记", "知识库", "obsidian", "vault", "manifest.md", "~/Documents/obsidian", "$HOME/Documents/obsidian", or folder names like designs/, sessions/, issues/, branches/, docs/, 3_Resources/tech/.
---

# Obsidian Vault — Personal Knowledge Base

Personal Obsidian vault workflow: locate relevant notes quickly, save new notes in the right folder with the standard naming convention, and keep project indexes current.

## Vault Root (current platform)

The vault root depends on the runtime platform. The **current** platform is:

- **macOS**: `~/Documents/obsidian/` — current working platform.

All Read/Write/Glob/Grep calls into the vault must resolve from the platform-appropriate root. Shell-style `~` (or `$HOME`) is preferred over hard-coding the absolute path: keeps the skill portable across machines/usernames and avoids leaking the account name into prompts or logs. Other platforms see `references/vault-paths.md`.

## Read — locate, don't dump

1. **Don't scan the full vault or read `INDEX.md` end-to-end.** Wastes context; doesn't answer the question.
2. **Prefer locating by directory name / file name first** — use Glob (e.g. `designs/**`, `**/*<keyword>*.md`) or Grep with the topic before opening files.
3. **For project-specific queries, read that project's `manifest.md` first** — it is the project's own index inside the vault.
4. **Read on demand; stop once you have enough context.** Don't fetch related/sibling notes unless the task requires it.

## Write — folder taxonomy, naming, manifest

### Folder placement (by content type)

| Folder               | Content                                                    |
|----------------------|------------------------------------------------------------|
| `designs/`           | 架构设计、方案决策                                          |
| `sessions/`          | 实施过程、会话总结、用户反馈                                |
| `issues/`            | Bug、排查记录                                               |
| `branches/`          | 分支相关上下文                                              |
| `docs/`              | 其它项目文档                                                |
| `3_Resources/tech/`  | 跨项目技术知识                                              |

When a note could fit more than one folder, pick by primary signal:
- Debugging / root-cause trace → `issues/`
- Architecture / scheme decision → `designs/`
- Working journal, standup, or session summary → `sessions/`
- Tech you want to revisit later → `3_Resources/tech/`

### File naming convention

```
YYYY-MM-DD-title.md
```

- `YYYY-MM-DD` = the date the event happened (or the date the note is for, if that's the only date you have).
- `title` clearly describes the topic for searchability.
- Avoid spaces in filenames; prefer hyphens.

### Avoiding duplicates

Before creating a new note:
1. Glob or Grep for similar titles (`**/*<topic>*`, `**/*<chinese-keyword>*`).
2. If a good match exists, **update it** rather than create a duplicate.
3. Only create when no good match exists.

### Manifest sync

Whenever you create or significantly change a project note, **synchronously update that project's `manifest.md`** with the new entry so the index stays useful. An out-of-date `manifest.md` defeats the read rules above.

`manifest.md` is per-project and lives inside that project's folder. If it doesn't exist yet, **ask the user before creating it**.

## Typical Flow

- **Read:** Glob/Grep by directory or keyword → open project's `manifest.md` if scoped → open target files → stop.
- **Write:** Check duplicates → pick folder by content type → build path `<vault-root>/<folder>/YYYY-MM-DD-<title>.md` → write → update `manifest.md` → report the written (or updated) path back.

## References

- `references/vault-paths.md` — per-platform vault root paths (macOS is current; Windows/Linux kept for reference).

---
name: obsidian-vault
description: Use when the user wants to find, read, write, save, or update notes in their personal Obsidian knowledge base vault. Triggers on phrases like "笔记", "知识库", "obsidian", "vault", "manifest.md", or folder names like designs/, sessions/, issues/, branches/, docs/, 3_Resources/tech/. Triggers regardless of host platform — vault root is auto-detected.
---

# Obsidian Vault — Personal Knowledge Base

Personal Obsidian vault workflow: locate relevant notes quickly, save new notes in the right folder with the standard naming convention, and keep project indexes current.

## Vault Root (auto-detect by platform)

This skill is portable across machines. The path table is **hardcoded** (each platform lists its vault root below), but **which platform is current** is detected at runtime — read the system prompt's `Platform` field (`win32` / `darwin` / `linux`) and pick the matching row. **Do not pin a specific platform as "current" in this skill** — that defeats the portability.

Path table (see `references/vault-paths.md` for the full list and conventions):

- Windows: `D:/file/obsidian/`
- macOS: `~/Documents/obsidian/`
- Linux: *TBD*

All Read/Write/Glob/Grep calls into the vault must resolve from the detected platform's root. Shell-style `~` (or `$HOME`) is preferred over hard-coding the absolute path: keeps the skill portable across machines/usernames and avoids leaking the account name into prompts or logs. If the detected platform isn't in the table or the conventional path doesn't exist, ask the user before proceeding.

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

## Write — content discipline (final state only)

Notes are written for **future implementers**, not for the author to remember the discussion. **Write only the conclusion + implementation details. Never write meta-history.**

### Disallowed in note bodies

| Category | Patterns |
|---|---|
| Self-reference to the note itself | `上一版笔记` / `上一轮笔记` / `之前笔记` / `本笔记 L100` / `见上文第 N 行` |
| Self-reference to the discussion | `上轮讨论` / `本次讨论` / `前面讨论过` / `用户提到` / `Claude 提议` |
| Self-attribution of contribution | `我加了` / `我写的` / `我提议` / `我建议` / `我之前漏了` |
| Date / version anchors as preamble | `2026-XX-XX 补充:` / `新增:` / `更新:` (as paragraph opener, not inline reference) |
| Decision archaeology | `旧→新对照表` / `A 方案 → B 方案` / `推翻 X 方案的过程` / `为什么不用 Y` |

### Rewrite as

| Wrong | Right |
|---|---|
| `上一版笔记里 X 显得重` | `X 显得重` |
| `2026-07-29 补充:经讨论确认 Y` | `Y` (direct statement) |
| `本笔记 L100 已记录 Z` | `Z` (restate in current paragraph or expand inline) |
| `我提议了方案 A` | `方案 A` (describe the plan itself, not who proposed it) |
| `A 方案(已被 B 推翻)` | Drop A and keep B; if A must be mentioned, one sentence that states the conclusion without describing the reversal |

### Exceptions

- `manifest.md` index entries that mark a note as `已作废` / `已撤销` are necessary (the index needs to show invalidation); this rule is for **note bodies**, not the index.
- Implementation details, code snippets, configuration values, decision criteria — all fine, those ARE the final state.
- Brief inline references like "see §3 below" are OK only when they point to the *current* section's content, not historical reasoning.

### Why

The discussion process is transient session context. The implementer opening the note 6 months later has none of that context; self-references + decision archaeology = more noise than signal, making the note longer and harder to judge "what is the actual conclusion now".

---

## Typical Flow

- **Read:** Glob/Grep by directory or keyword → open project's `manifest.md` if scoped → open target files → stop.
- **Write:** Check duplicates → pick folder by content type → build path `<vault-root>/<folder>/YYYY-MM-DD-<title>.md` → write → update `manifest.md` → report the written (or updated) path back.

## References

- `references/vault-paths.md` — per-platform vault root path table; the model picks the row matching the runtime `Platform` field.

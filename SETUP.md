# Project Setup Automation

Instructions for AI agents to set up a new system with all agent configurations.

> **Important:** Before each step, check if the destination already exists.
> - **Matches source** — skip it, report as done.
> - **Exists but differs** — try to merge changes automatically. If conflicts can't be resolved, show the diff and ask the user.
> - **Missing** — perform the step.

---

## 1. Skills

Read `skills/SKILL_LIST.md` and run each `npx skills add` command listed there.

Then copy all custom skill folders from `skills/` (everything except `SKILL_LIST.md`) to both destinations.

**Destinations:**
- `~/.agents`
- `~/.claude/skills/`

---

## 2. Slash Commands

Copy all files from `slash-commands/` to their destinations.

| Source | Destination |
|---|---|
| All files in `slash-commands/` | `~/.claude/commands/` |
| All files in `slash-commands/` | `~/.codex/prompts/` |

---

## 3. Custom Instructions

Copy `custom-instructions/AGENTS.md` to `~/.claude/AGENTS.md`, then create symlinks.

| Source | Destination | Type |
|---|---|---|
| `custom-instructions/AGENTS.md` | `~/.claude/AGENTS.md` | Copy (real file) |
| | `~/.claude/CLAUDE.md` | Symlink → `~/.claude/AGENTS.md` |
| | `~/.codex/AGENTS.md` | Symlink → `~/.claude/AGENTS.md` |

---

## 4. Settings

Copy settings files to their destinations.

| Source | Destination |
|---|---|
| `agent-settings/settings.json` | `~/.claude/settings.json` |
| `agent-settings/config.toml` | `~/.codex/config.toml` |

---

## Execution Order

1. Create destination directories if they don't exist (`~/.claude/commands/`, `~/.claude/skills/`, `~/.codex/prompts/`, `~/.agents`)
2. Copy **Custom Instructions** (AGENTS.md) and create symlinks (CLAUDE.md, codex AGENTS.md)
3. Copy **Settings** (settings.json, config.toml)
4. Copy **Slash Commands** to both `~/.claude/commands/` and `~/.codex/prompts/`
5. Install **Skills** by running all `npx skills add` commands from `skills/SKILL_LIST.md`

---

## Summary Report

After completing all steps, print a summary table listing every item and what action was taken.

| Item | Action | Details |
|---|---|---|
| `~/.claude/AGENTS.md` | Copied / Skipped / Updated | — |
| `~/.claude/CLAUDE.md` | Symlinked / Skipped | — |
| `~/.codex/AGENTS.md` | Symlinked / Skipped | — |
| `~/.claude/settings.json` | Copied / Skipped / Updated | — |
| `~/.codex/config.toml` | Copied / Skipped / Updated | — |
| Each slash command file | Copied / Skipped | — |
| Each custom skill folder | Copied / Skipped | — |
| Each `npx skills add` command | Installed / Skipped | — |

For each row:
- **Action** — what the agent did: `Copied`, `Symlinked`, `Installed`, `Updated`, or `Skipped (already up to date)`.
- **Details** — if updated or skipped, briefly explain what differed or why it was skipped. Leave blank if straightforward.

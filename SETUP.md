# Project Setup Automation

Instructions for AI agents to set up a new system with all agent configurations.

> **Important:** Before performing any step, check if it is already done. If the destination files/folders already exist and match the source, only verify and report to the user — do not overwrite or re-run. Only perform the step if it is missing or outdated.

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

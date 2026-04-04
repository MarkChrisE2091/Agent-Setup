# AGENTS.md

> Rules, conventions, and guidelines for AI agents working in this codebase.

---

## User Rules

**Follow these always. No exceptions.**

- When the user says something works, **try it first**. Don't argue or push back.
- Never say "that's not possible" or "that's blocked" without testing it yourself first.
- Don't be confidently wrong. If unsure, test before speaking.
- No assumptions. Try everything out when not sure, and test the approach before starting implementation.
- No guessing. For unclear problems or missing information, search online first. Make the search comprehensive to find the best solution. If found, explore the solution and make plan to apply the relevant parts and copy the relevant solution over.
- If user requests you to do something, check it before you answer, even if you already done it before.

---

## Planning & Implementation

- For each major implementation, **make a plan first** (include a way to verify in the plan).
- After implementation, **verify against the plan**.
- Test and verify nothing existing is broken (unless it needed a dramatic refactoring).

---

## Bug Fixing

- Before fixing anything, **find all the relevant code** in the project first, then make a plan.
- Auto-ponder: "Is it the right fix?" If correct and confident, start fixing. If not, revise the plan until confident.
- After fixing, **test against the plan** to check if everything is working.

---

## Development Workflow

- **Test after each major implementation.** Don't batch — verify as you go.
- **Test whenever you touch logic.**
- Restart the app after each major fix if needed.
- When adding or removing features, scan the entire codebase to find all places that need modification, then update them all.

---

## Research & Problem Solving

- When there are things you don't know, **search the internet first**.
- For unclear problems or missing information, search online comprehensively to find the best solution.
- If you find a solution, explore it and make a plan to apply the relevant parts.

---

## Git Conventions

- **Never** include AI agent as co-author (of any kind) in commits or pushes.

---

## Project Documentation

- Maintain a `PROJECT.md` file at the project root.
- Generate it based on the project context. Update it as the project evolves.
- Include the following sections:
  - **Overview** — What the project is, what problem it solves, and who it's for.
  - **Features** — List of current capabilities and functionality.
  - **Architecture** — System architecture design, key components, how they interact, and design decisions.
  - **Project Structure** — Folder/file organization and what lives where.
  - **Roadmap** — Planned features, improvements, and next steps.

---

## README.md Guidelines

### Structure

| Section                      | Purpose                                              |
| ---------------------------- | ---------------------------------------------------- |
| **Title + One-liner**        | What it is in one sentence                           |
| **Hero visual**              | Screenshot, demo GIF, or architecture diagram        |
| **Installation**             | Prerequisites, install command, setup steps           |
| **Quick start**              | Get running in <2 min (first command + expected output) |
| **Features**                 | Bullet list of what it does                          |
| **Configuration**            | How to customize it                                  |
| **Architecture** (optional)  | Brief system overview for complex projects           |
| **API reference** (optional) | If it exposes an API                                 |
| **Contributing**             | How others can help                                  |
| **License**                  | License type                                         |

### Principles

- **Lead with the "why"** — What problem does this solve? Who is it for?
- **Show, don't tell** — A screenshot or GIF is worth 1000 words.
- **Copy-pasteable commands** — Every code block should work when pasted directly.
- **Progressive disclosure** — Quick start first, deep details later.
- **Keep it current** — Outdated READMEs are worse than no README.
- **Badges** — Build status, version, license for at-a-glance credibility.

### Anti-patterns

- Wall of text with no structure
- Documenting internals instead of user-facing behavior
- Missing install/setup steps (assuming the reader knows your stack)
- No examples of actual usage
- Overloading with every detail — link to docs instead

---

## Note: Symlink CLAUDE.md to AGENTS.md

For cmd:
```cmd
mklink CLAUDE.md AGENTS.md
```

For PowerShell:
```powershell
New-Item -ItemType SymbolicLink -Path "CLAUDE.md" -Target "AGENTS.md"
```

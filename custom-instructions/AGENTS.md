# AGENTS.md

> Rules, conventions, and guidelines for AI agents. These rules are mandatory. Read this file before doing anything else.

---

## User Rules

**MUST Follow these always. No exceptions.**

- When the user says something works, **try it first**. Don't argue or push back.
- Never say **"that's not possible"** or **"that's blocked"** without testing it yourself first.
- Don't be **confidently wrong**. If unsure, **test before speaking**.
- **No assumptions:** 
  - Making a conclusion with assumption without testing it out **is not accaptable**. User will **reject your answer**.
  - Try everything out when not sure, and **test the approach before starting implementation**.
- **No guessing.** For unclear problems or missing information, **search online first**. Make the search comprehensive to find the best solution. If found, explore the solution and make plan to apply the relevant parts and copy the relevant solution over.
- If user requests you to do something, **check it before you answer**, even if you already done it before.
- **Verify before answering.** If it's code, read the code. If it's not code, search online or test it. Remove uncertainty before speaking — don't speculate, don't flip-flop.
- **Verify before presenting.** Before showing any solution, approach, or answer — 
  test it yourself first. Do not present an untested approach. 
  If it's code, run it. If it's a plan, trace through it. 
  If it's a claim, confirm it. The user should never be the first one to discover it doesn't work.
  **After testing, restore everything to its original state** before presenting. 
  Leave no side effects, temp files, or changes from your verification behind.
- **Answer yes or no first.** If the user asks a confirmation question, lead with "Yes" or "No." Don't restate context the user already knows unless user asks for it. Add detail only if it's genuinely new information.
- **Do not use abbreviation** for first time mention of names or concepts.
- **Be brief and plain.** Lead with the answer. Skip preamble, filler, and restating 
  what the user already knows. Use the simplest language that is still accurate — 
  never dumb it down to the point of being wrong, but never use complex words 
  when simple ones work. The fewer words to convey the point clearly, the better. 
  If detail is needed, add it after — never before.

---

## Before Starting Any Work

Before designing, implementing, or fixing anything:

1. **Gather all relevant info first.** Read the code, run `--help`, check docs, search online. Don't start work based on assumptions.
2. **Check how the project already handles similar things.** Don't reinvent what already exists.
3. **Research existing solutions.** Search online for how others solved the same problem. Check reference codebases on disk if available. Compare multiple approaches before picking one.
4. **Scan the full blast radius.** Find all files, callers, and consumers that a change will affect before writing any code.
5. **Trace errors to root cause.** Don't guess — read the stack trace, check logs, reproduce first.
6. **Never claim something can't be done without verifying first.** Check docs, run commands, or search online.
7. **Verify dependency capabilities against their actual docs** — not just how the current project uses them.

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
  - **Database** *(if applicable)* — Schema overview, key models/tables, relationships, and migration strategy.
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

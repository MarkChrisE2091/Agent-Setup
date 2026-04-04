---
description: Explore and understand the entire project structure, architecture, and codebase.
---

# Explore: Project exploration

Explore the entire project/codebase and understand it thoroughly.

## Instructions

Systematically read and analyze the codebase from the root. Do not ask for clarification — proceed autonomously. Use file reading, directory listing, and search tools to gather everything you need.

### Phase 1 — Discovery
- List the full directory tree (ignore `node_modules`, `.git`, build/dist folders)
- Read `README`, `package.json` / `pyproject.toml` / `Cargo.toml` (or equivalent), config files, and any docs
- Identify the tech stack, language(s), frameworks, and key dependencies

### Phase 2 — Architecture Analysis
- Trace the entry point(s) of the application
- Map out the high-level folder structure and what each area owns
- Identify core modules, services, and how they relate to each other
- Note any design patterns in use (MVC, layered, event-driven, etc.)
- Find where data flows: inputs → processing → outputs/storage

### Phase 3 — Deep Dive
- Read key source files in full (not just skimming)
- Understand the most critical or complex logic in the codebase
- Identify shared utilities, constants, types/interfaces
- Note any tests and what they cover
- Flag anything unusual, inconsistent, or worth paying attention to

## Output Format

Respond with two clearly separated sections:

---

### 📋 Summary
- **Project:** What this project is and what it does (2–3 sentences)
- **Stack:** Languages, frameworks, major libraries
- **Structure:** Top-level folder map with one-line descriptions
- **Entry point(s):** Where execution begins
- **Key concepts:** 3–5 domain or architectural ideas a new contributor must understand

---

### 🔍 Deep Dive

Cover each of the following with enough detail to act on:

1. **Architecture** — How the system is organized and why
2. **Data flow** — How data moves through the system end-to-end
3. **Core modules** — What the most important files/folders do
4. **Patterns & conventions** — Coding style, naming, structure patterns used
5. **Tests** — What's covered and how tests are organized
6. **Gotchas & notes** — Anything surprising, technical debt, or worth flagging

---

Be thorough. Prioritize accuracy over speed. If something is unclear from reading the code, say so explicitly rather than guessing.
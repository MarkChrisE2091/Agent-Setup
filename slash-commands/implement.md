---
description: Execute the current plan fully across the entire codebase.
---

# Implement: Execute the plan

Execute the plan that is currently in context. Make sure to implement it fully across all relevant parts of the codebase. Make sure to modify all relevant parts of the project.

## Prerequisites

A plan must already be present in the conversation context before running this command. Do not infer or invent a plan — if none exists, stop and ask the user to provide one.

## Instructions

Work through the plan step by step, autonomously. Do not ask for clarification mid-implementation unless you encounter a genuine blocker that cannot be resolved from the codebase itself.

### Phase 1 — Pre-flight
- Re-read the plan carefully before touching any code
- Identify every file and module that will need to change
- Note any risks or dependencies between changes
- Confirm you have a clear way to verify the implementation — if not, define one before proceeding

### Phase 2 — Implementation
- Execute each step of the plan in logical order
- Make changes to **all** relevant parts of the project — don't stop at the happy path
- Cover: source files, types/interfaces, config, constants, related modules, and any other code affected by the change
- Follow existing code style, naming conventions, and patterns in the codebase
- For anything uncertain or unclear, search online for the best solution and use it

### Phase 3 — Verification
- Run the existing test suite
- If tests fail: diagnose, fix, and re-run — do not mark the task done until they pass
- If no tests exist: flag this explicitly, but proceed

## Done Criteria

The implementation is complete when:
- [ ] All steps in the plan are executed
- [ ] All affected parts of the project are updated (nothing left half-done)
- [ ] Existing tests pass

## Output Format

When done, respond with:

### ✅ Implementation Complete

**Plan completion:**

| Step | Description | Status |
|------|-------------|--------|
| 1 | [step from plan] | ✅ Done / ⚠️ Partial / ❌ Skipped |
| 2 | [step from plan] | ✅ Done / ⚠️ Partial / ❌ Skipped |
| ... | | |

Populate this table from the actual steps in the plan. Every step must have a status — nothing left blank.

**Changes made:**
A concise list of every file modified, created, or deleted — with one line explaining what changed and why.

**Test results:**
State whether tests passed, how many ran, and note anything skipped or newly broken.

**Anything to flag:**
Call out edge cases, trade-offs, or anything the user should be aware of before accepting the changes.

---

Be thorough. An incomplete implementation that misses side effects is worse than not starting. The user may revert if the result is not solid.

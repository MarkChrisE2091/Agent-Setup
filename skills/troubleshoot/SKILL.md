---
name: troubleshoot
description: "Use this skill whenever the user reports something broken, unexpected, or not working. Triggers on phrases like 'what is wrong', 'it is broken', 'something failed', 'why is this not working', 'debug this', or any error message being shared. Always follow the full troubleshoot loop: reproduce, trace the root cause step by step, report, then clean up."
---

# Troubleshoot

Follow these steps in order, every time. Do not fix anything unless the user asks.

---

## Step 1 — Understand the Problem

Before touching anything:
- What is the expected behavior?
- What is actually happening?
- When did it start? What changed?

If unclear, ask one focused question. Don't guess.

---

## Step 2 — Reproduce the Error (manually, in the foreground)

- Run the failing thing yourself — don't just read logs or assume.
- Do it step by step, in the foreground, and watch what happens.
- Confirm you can see the exact same error the user sees.
- If you can't reproduce it, say so and ask for more context.

Do not skip this step.

---

## Step 3 — Trace the Root Cause Step by Step

Dig through the chain of events exactly as they happened:
- Start from the error and work backwards — what triggered it?
- Follow each step: what called what, what changed, what was missing.
- Check logs, stack traces (the full list of steps that led to the crash), and system state.
- Keep going until you hit the original cause — not just the first symptom.
- Document each step you find, in order, like a timeline.

Ask: *why did this happen*, not just *what happened*.

---

## Step 4 — Report to the User

Report the root cause in one clear sentence. Use bullets, lists, or a table if it helps clarity. Plain prose if it doesn't.

Do not apply any fix. Wait for the user to decide what to do next.

---

## Step 5 — Clean Up and Restore

After reporting:
- Remove any test files, temporary changes, or debug code added during investigation.
- Restore any settings, configs, or state that were changed while reproducing the error.
- Leave the system exactly as it was before troubleshooting started.

Confirm cleanup is done before closing out.

---

## Rules

- Always work in the foreground — stay present and watch every step.
- Never assume the cause without reproducing the error first.
- Never fix anything unless the user explicitly asks.
- Never leave behind test files, debug logs, or temp changes unless the user asks.

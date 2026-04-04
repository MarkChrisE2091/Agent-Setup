---
description: Run the full test suite, fix issues until everything passes, with smoke and E2E where needed.
---

# Test: Run, fix, and verify everything

Run the full test suite across the project. Fix any failures until everything passes. Do not mark done until all tests are green.

## Instructions

Work through each testing layer in order. Do not skip a layer. Do not move to the next layer if the current one is failing — fix first, then proceed.

### Phase 1 — Smoke Tests
- Run a quick sanity check to confirm the project builds and starts without crashing
- Verify the entry point runs without errors
- Check that critical environment variables and configs are present
- If smoke fails: stop, fix, and re-run before proceeding

### Phase 2 — Unit Tests
- Run the full unit test suite
- If tests fail: diagnose the root cause, fix the code (not the test unless the test is wrong), and re-run
- Repeat until all unit tests pass
- Note any tests that are skipped or explicitly disabled

### Phase 3 — Integration Tests
- Run integration tests to verify modules work correctly together
- If tests fail: trace the issue across module boundaries, fix, and re-run
- If no integration tests exist: flag this as a gap but proceed

### Phase 4 — E2E Tests
- Only run if explicitly requested (e.g. `/test e2e`) or if Phase 2 and 3 pass AND the change touches a critical user flow (auth, navigation, core features, data mutations)
- If neither condition is met: skip E2E and note it was not run in the output
- Run E2E tests for critical user flows: auth, core features, navigation, data mutations
- If no E2E tests exist but the project has a UI or API: flag this and write minimal E2E coverage for the most critical flow before proceeding
- If tests fail: fix and re-run until passing

## Fix Policy
- Always fix the code, not the test — unless the test itself is clearly wrong
- For anything uncertain, search online for the best solution and use it
- If a fix introduces new failures, resolve those before moving on

## Done Criteria

The task is complete when:
- [ ] Smoke tests pass
- [ ] All unit tests pass
- [ ] All integration tests pass (or gaps are flagged)
- [ ] E2E tests pass for critical flows (or gaps are flagged)
- [ ] No regressions introduced by fixes

## Output Format

When done, respond with:

### ✅ Test Run Complete

| Layer | Status | Passed | Failed | Skipped |
|-------|--------|--------|--------|---------|
| 🟢 Smoke | ✅ / ❌ | n | n | n |
| 🔵 Unit | ✅ / ❌ | n | n | n |
| 🟣 Integration | ✅ / ❌ | n | n | n |
| 🔴 E2E | ✅ / ❌ | n | n | n |

**Fixes made:**
List every fix applied — what broke, what was changed, and which file.

**Gaps flagged:**
Any missing test layers or coverage gaps worth addressing.

**Anything to flag:**
Flaky tests, skipped suites, or anything that passed but looks fragile.

---

Be thorough. A passing test suite that skips the hard cases is not a passing test suite.
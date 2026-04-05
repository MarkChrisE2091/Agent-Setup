---
name: e2e-testing
description: Apply best practices when writing, reviewing, or planning end-to-end (E2E) tests. Use when the user asks to write E2E tests, set up a test suite, fix flaky tests, or plan a testing strategy for any kind of project — web apps, backend APIs, CLI tools, mobile apps, or desktop applications. Note: This skill is mostly for web apps, backend APIs, CLI tools. Mobile and desktop projects can use the core principles but may need platform-specific guidance beyond this skill.
---

# E2E Testing Skill

This skill governs how to approach any E2E testing task: writing new tests, reviewing existing ones, setting up infrastructure, or advising on strategy. Apply these principles as strong defaults, not suggestions. Deviate when the user explicitly asks for something different, or when the project's established conventions clearly point another way.

---

## Default Workflow

When asked to work on E2E tests without more specific instruction or context, follow this sequence:

1. **Inspect the existing suite** — read a sample of existing tests to understand conventions, patterns, and what's already covered
2. **Inspect app startup and test env** — check how the app is started for tests, what env vars are required, and how the DB or dependencies are managed
3. **Discover key journeys** — read routing, entrypoints, or feature flags to identify the highest-risk flows (see "Before Writing a Single Test" below)
4. **Write the smallest high-value test first** — one smoke-level test for the most critical flow, not a full suite
5. **Run it** — verify it passes in the actual test environment, not just locally or hypothetically
6. **Collect artifacts** — confirm screenshot, log, and trace capture is working on failure
7. **Report** — summarize what was tested, what was found, and what should be covered next

Don't skip straight to writing tests. Steps 1–3 take minutes and prevent hours of rework.

---

## Before Writing a Single Test

Before generating test code, gather enough context to apply the right defaults:

1. **What framework/runner is in use?** (Playwright, Cypress, Selenium, Detox, etc.) If none exists yet, recommend Playwright for web apps.
2. **What stack is under test?** (React, Next.js, Rails, etc.) This affects selector strategy and how to seed data.
3. **Is there an existing test suite?** If so, read a few existing tests first. Match their patterns unless they're clearly broken.
4. **What user journey does this test cover?** By default, redirect tests scoped to a single component or internal function — those belong at the unit or integration layer. Exception: if the repo intentionally uses browser-level component E2E (e.g., Storybook interaction tests, or a component library with no higher-level app to test against), treat that as the established convention and follow it.

   If no journeys are specified, don't ask — discover them. Read the codebase to identify critical paths:
   - **Web apps**: check the router config (React Router, Next.js `app/` or `pages/`, Rails routes) for top-level routes
   - **APIs**: check the entrypoint file, route registration, or OpenAPI/Swagger spec for the most-called or highest-risk endpoints
   - **Feature flags**: if a flag system exists, flagged features are often the riskiest and most worth covering
   - **Auth boundaries**: any route or endpoint that requires authentication is a candidate — auth failure modes are high-impact

   From these, identify 3–5 flows that would cause the most user-visible damage if broken. Propose them to the user with a one-line rationale for each before writing any tests.

5. **What environments exist?** CI, staging, local? This affects how to handle env vars, base URLs, and data seeding.

If context is missing and it materially affects the test, ask before generating.

---

## Core Principles

Apply all of the following when writing or reviewing E2E tests.

### Scope: Critical User Journeys Only

E2E tests are expensive — slow to run, hard to maintain, fragile under refactor. Reserve them for flows that:
- A real user would be blocked by if broken
- Cross meaningful system boundaries (browser → server → DB)
- Cannot be covered confidently by a unit or integration test

**Do not write E2E tests for:**
- Individual UI components in isolation
- Edge case input validation
- Pure logic or algorithms
- Anything already covered by a unit test — except critical smoke paths (login, core actions, data persistence), which still need E2E coverage.

If the user asks for E2E coverage of something that belongs at a lower layer, say so and offer to write the right kind of test instead.

### Environment: Isolated and Disposable

Every E2E test suite should run in an environment that is:
- **Separate** from production and shared staging: own app instance, own DB, own log output, own temp file space
- **Disposable**: can be torn down and recreated without side effects
- **Controlled**: no shared state between runs, no reliance on external services being up

Where it makes sense, prefer Docker Compose or equivalent to define the test environment declaratively. For serverless, managed cloud, or projects with existing test infrastructure, use whatever achieves the same goal: a reproducible, isolated environment that can be stood up and torn down on demand.

### Determinism: Seed Data, Mock Externals

A test that passes sometimes is worse than no test. Every test must be deterministic:

- **Seed known data** via API or direct DB call before the test runs — not through UI interactions
- **Freeze or stub time-dependent inputs** where relevant (timestamps, tokens, IDs)
- **Mock slow or unpredictable externals**: LLMs, email delivery, payment processors, SMS, third-party OAuth. Use contract stubs or service-level mocks, not monkey-patching inside the app
- **Do not mock core application logic** — that defeats the purpose of E2E

If the user asks to mock everything, push back. The value of E2E is the real integration path.

### Behavior: Test Through the Public Interface

Tests should exercise the application through the same interface a real user or consumer would use — never through internal shortcuts:

- **Browser apps**: real browser, real navigation, real clicks and form inputs. No reaching into internal state, component props, or Redux stores to assert. Assert what the user sees: DOM state, URLs, rendered text, success messages.
- **APIs**: real HTTP requests to a running server. No calling functions directly, no importing app internals into the test. Assert on response shape, status, and body.
- **CLI tools**: real subprocess invocation. Assert on stdout, stderr, exit code, and any files or state produced.

**Always verify both layers when it matters.** After a form submission or API call, don't just assert the response — also verify the side effect: the record exists in the DB, the job was queued, the file was written. Asserting only the surface catches fewer real regressions.

### Selectors: Stable Over Convenient (Browser Tests Only)

Use selectors in this priority order:

1. ARIA roles and accessible labels (`getByRole`, `getByLabelText`) — the best user-level contract; reflects how users actually interact and improves accessibility awareness
2. `data-testid` attributes — good fallback when no meaningful role or label exists; decoupled from styling and copy
3. Semantic text that is stable and intentional (`getByText` for a heading that won't change)
4. CSS classes or IDs — only if they're clearly semantic and stable, never for styling classes
5. XPath — avoid entirely unless there is no other option

When generating test code, always use the appropriate selector query helper for the framework (`page.getByTestId`, `cy.findByRole`, etc.) — never raw CSS string selectors unless the framework requires it.

If the codebase has no `data-testid` attributes on interactive elements, flag this and suggest adding them as part of the test work.

### Isolation: Each Test Owns Its State

Every test must be able to:
- Run alone (no dependency on prior test state)
- Run in any order (no implicit ordering)
- Clean up after itself (or rely on a fresh environment per test)

**Concretely:**
- Set up all required data at the start of each test or test file
- Do not rely on data left over from a previous test
- If tests share fixtures, those fixtures must be immutable and re-seeded per run
- Each test should have its own user, session, or tenant where possible to avoid state bleed

This is the prerequisite for parallelization. Always structure tests this way even before parallel runs are needed.

### Waiting: Signal-Based, Never Arbitrary

Never use `sleep`, `wait(n)`, or fixed timeouts to handle async behavior. Always wait for a specific observable signal:

| What to wait for | Example |
|---|---|
| DOM state | Element visible, text present, button enabled |
| Network response | Request completes, response status received |
| WebSocket event | Message received, connection state change |
| DB row | Record exists or has updated value |
| Log line | Specific output written to application log |

Most modern frameworks (Playwright, Cypress) handle this automatically for DOM events. For backend state (DB, logs), implement explicit polling helpers with a timeout and a clear error message on timeout.

If you see `await page.waitForTimeout(2000)` or equivalent in existing tests, flag it as a smell and replace it.

### Artifacts on Failure

Tests that fail silently are nearly useless. Every test run should capture on failure:

- **Browser tests**: screenshot at point of failure, browser console logs, network trace or HAR file
- **API tests**: full request/response pair, server logs for the duration of the test, DB state before/after
- **All tests**: application-level logs from the test run

When setting up a new suite, look up the correct failure artifact config for whichever runner is in use and configure it explicitly. Don't leave it at defaults — most runners require opt-in for screenshots, traces, and log capture.

If CI is in scope, ensure artifacts are uploaded and accessible after a failed run.

### Suite Shape: Smoke vs. Scenario

Structure the suite in two tiers:

**Smoke tests**
- Cover the single most critical path through each major feature (login works, core action completes, data persists)
- Must be fast: target < 2 minutes for the full smoke suite
- Run on every commit or PR

**Scenario tests**
- Cover fuller user journeys, edge-condition flows, multi-step interactions
- Slower, more thorough
- Run pre-deploy, nightly, or on merge to main

When writing a new test, infer the tier from the scope and complexity of the flow. Ask only if the answer would materially change what gets written. Don't lump everything into one undifferentiated suite.

### Flakiness: Treat as a Bug

A flaky test is a broken test. It erodes trust in the whole suite.

**When a test is flaky:**
1. Do not add a retry as the fix. A retry hides the symptom.
2. Diagnose the root cause: race condition, shared state, timing assumption, environment inconsistency
3. Fix the root cause
4. If the root cause is genuinely external (e.g., a third-party staging environment), mock that external dependency instead

Retries are acceptable as a short-term quarantine while investigating, not as a permanent solution. If using retries, add a comment explaining what is being investigated and when it should be resolved.

### Environment Parity

The test environment should match production *in application behavior*, not necessarily in infrastructure scale:
- Same environment variables and feature flags as production (except secrets — use test equivalents)
- Same authentication and authorization paths
- Same database schema and migrations
- Same background job behavior

What you *can and should* control for tests:
- External service calls (mock these)
- Time-based randomness (freeze or stub)
- Email/SMS delivery (intercept, don't send)
- LLM calls (stub responses)

Do not use an environment so stripped down that the tests don't exercise the real code paths.

---

## API / Backend E2E (No Browser)

A significant class of E2E tests have no browser. They test a full request chain: real HTTP client → real running server → real DB → real response. All the core principles above apply. The following are the browser-specific adaptations to drop or replace:

**What counts as E2E here**
- A request that passes through auth middleware, business logic, writes to the DB, and returns a response
- A background job triggered by an API call that produces an observable side effect (record updated, event published, email queued)
- A multi-service flow where service A calls service B and the result is verifiable in service B's state

**What doesn't count**
- Calling a function or method directly in the test — that's a unit test
- Hitting a mocked server with stubbed responses — the integration value comes from the real stack running end-to-end. Note: an in-process server can still be valid E2E if it runs the full application stack (real DB, real middleware, real business logic) — what matters is that nothing meaningful is stubbed out, not whether the process boundary exists

**Selectors don't apply.** Skip the entire selector section. Instead assert on:
- HTTP response: status code, body structure, headers
- Persisted state: query the DB directly after the request to verify the write
- Downstream effects: check the job queue, the email stub, the audit log — not just the HTTP response

**Waiting still applies.** If an async side effect is under test (e.g., a background job runs after the request), implement a polling helper that queries the expected DB or queue state with a timeout and a clear error on timeout. Never sleep.

**Data seeding** is the same principle: seed via DB or a dedicated test-data API. Do not chain real API calls in setup — that creates hidden test dependencies and makes failures harder to diagnose.

**Recommended tools** (runner doesn't matter, principles do): `supertest` (Node.js), `httpx` + `pytest` (Python), `RestAssured` (Java), or plain `fetch` in a custom harness.

---

## When Reviewing Existing Tests

When asked to review, audit, or improve an existing E2E test suite, check for and flag:

- Tests that are really unit tests in disguise (wrong layer)
- Arbitrary `sleep`/`wait` calls
- Selectors using styling classes or fragile text
- Tests that depend on execution order
- Missing cleanup or teardown
- No artifact capture on failure
- Retries used to mask flakiness
- All tests in one undifferentiated suite with no smoke/scenario split
- Tests seeding data through the UI instead of API or DB

Report findings clearly, grouped by severity: **critical** (will cause false positives/negatives), **structural** (makes the suite hard to maintain), **minor** (style or convenience issues).

---

## Code Style Defaults

When generating test code, apply these defaults unless the existing codebase differs:

- One test file per user journey or feature area, not one file per test
- Shared setup in `beforeEach` or fixtures — not copied per test
- Descriptive test names that read as user behavior: `"user can reset password via email link"`, not `"reset password test"`
- Assertions that read like expectations on observable output, not implementation details
- Page Object Model (POM) for any interaction sequence used in more than two tests

---

## What to Say When Redirecting

If the user asks for something that conflicts with these principles, don't silently comply. Be direct:

- **Asked to test a single component E2E**: "This is more of a component or unit test scope. Want me to write this as a unit test instead, or is there a fuller user journey you want to cover?"
- **Asked to mock the whole backend**: "Mocking the full backend removes the integration value of E2E. I'd recommend mocking only the third-party calls and letting the real app handle the rest. Want to take that approach?"
- **Asked to add retries to a flaky test**: "Retries will hide the flakiness rather than fix it. Let's find the root cause first. What does the failure look like?"
- **No data-testid attributes exist**: "The selectors here will be brittle. I'd recommend adding data-testid attributes to these elements as part of this work. Want me to suggest where?"
- **No journeys specified and codebase available**: Don't ask — read the router/entrypoints and propose the 3–5 highest-risk flows before writing anything.

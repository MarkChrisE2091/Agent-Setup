---
description: Scan the entire project for bugs, security issues, code quality, missing tests, and performance problems.
---

# Scan: Project health check

Scan the entire project and produce a full issues report. Do not fix anything — report only.

## Instructions

Read and analyze every relevant source file in the project. Ignore `node_modules`, `.git`, build/dist folders. Be thorough — a missed issue is worse than a false positive.

### Phase 1 — Bugs & Logic Errors
- Incorrect logic, wrong conditions, off-by-one errors
- Unhandled edge cases or null/undefined paths
- Async issues (race conditions, missing awaits, unhandled rejections)
- Incorrect error handling or swallowed exceptions

### Phase 2 — Security Vulnerabilities
- Hardcoded secrets, tokens, or credentials
- Unsanitised user input, injection risks (SQL, XSS, command)
- Insecure dependencies or outdated packages with known CVEs
- Exposed sensitive data in logs, errors, or responses
- Improper authentication or authorisation logic

### Phase 3 — Code Quality & Dead Code
- Unused variables, functions, imports, or entire files
- Duplicated logic that should be abstracted
- Overly complex functions that should be broken down
- Inconsistent naming or patterns across the codebase
- TODO/FIXME/HACK comments left in code

### Phase 4 — Missing Tests
- Features or modules with no test coverage
- Critical paths (auth, data mutation, error handling) that are untested
- Existing tests that are shallow or don't assert meaningfully

### Phase 5 — Performance Issues
- Unnecessary re-renders or recomputations
- N+1 query patterns or inefficient data fetching
- Missing indexes or heavy operations on the critical path
- Large assets or imports that could be lazy-loaded

## Output Format

### 🔍 Scan Report

**Summary:**

| Category | Issues Found |
|----------|-------------|
| 🐛 Bugs & Logic Errors | N |
| 🔐 Security Vulnerabilities | N |
| 🧹 Code Quality & Dead Code | N |
| 🧪 Missing Tests | N |
| ⚡ Performance Issues | N |

---

For each category, list issues in this format:

**🐛 Bugs & Logic Errors**

| # | Issue | File | Severity |
|---|-------|------|----------|
| 1 | [description] | `path/to/file.ts:line` | 🔴 High / 🟡 Medium / 🟢 Low |

*(Repeat table for each category)*

---

**Recommended priority:**
List the top 3–5 issues across all categories that should be fixed first and why.

---

Be conservative — flag anything that looks wrong even if you are not certain. Mark uncertain findings with `(needs review)` rather than omitting them.
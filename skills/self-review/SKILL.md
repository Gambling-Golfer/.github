---
name: self-review
description: Pre-commit self-review checklist for code changes. Use this before committing to catch issues that would be flagged during code review.
---

# Pre-Commit Self-Review

Run this checklist before committing any code changes. This catches issues that code review would flag, reducing review iterations.

## Process

1. Stage changes: `git add -A`
2. Review the diff: `git diff --cached | head -500`
3. Run through every applicable section below

## Correctness (Critical — review blockers)

- [ ] Null/undefined/nil handled — no unguarded access
- [ ] Edge cases covered (empty, zero, boundary)
- [ ] Error paths return or throw — no silent failures
- [ ] No race conditions in async code

## Security (Critical — review blockers)

- [ ] User input validated (Zod on backend, proper checks on iOS)
- [ ] No SQL injection (parameterized queries only)
- [ ] No hardcoded secrets or tokens
- [ ] Auth checks on protected endpoints/screens

## Type Safety

- [ ] Backend: no `any`, explicit return types, strict config respected
- [ ] iOS: no force unwraps, no `Any`, `@MainActor` on UI code

## Testing

- [ ] New code has test coverage
- [ ] Tests cover happy path, errors, and edge cases
- [ ] Tests are isolated and deterministic

## API Design (if endpoints changed)

- [ ] Response format matches standard `{ success, data, meta }`
- [ ] Swagger `@openapi` annotations updated
- [ ] iOS models still match backend DTOs

## Code Quality

- [ ] Functions are focused, not overly complex
- [ ] Names are descriptive
- [ ] No dead code or ignored errors
- [ ] Documentation matches implementation

## Platform-Specific

- [ ] Backend: `.js` imports, import ordering, Zod schemas, response helpers
- [ ] iOS: sub-view extraction, `MARK` sections, accessibility, localized strings

Fix any issues BEFORE committing to reduce review iterations.

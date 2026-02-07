---
name: coding-standards
description: Shared quality contract for the Gambling Golfer project. Use this when writing, reviewing, or fixing code to ensure consistent quality standards across implementation and review phases.
---

# Coding Standards — Gambling Golfer

This is the **shared quality contract** between implementation and review. Both writing and reviewing code should use these standards as the single source of truth.

---

## 1. Correctness (Critical)

- [ ] No logic errors or off-by-one mistakes
- [ ] Null/undefined/nil/optional values handled explicitly — no unguarded access
- [ ] Edge cases covered (empty collections, zero values, boundary inputs)
- [ ] Race conditions considered for concurrent/async code
- [ ] No memory leaks or retain cycles (especially in closures and delegates)
- [ ] Error paths return or throw appropriately — no silent failures

## 2. Security (Critical)

- [ ] All user input validated and sanitized before use
- [ ] No SQL injection risk — use parameterized queries only
- [ ] No XSS risk — escape output where applicable
- [ ] Authentication/authorization checks present on protected endpoints/screens
- [ ] Sensitive data (tokens, passwords) never logged, hardcoded, or exposed in responses
- [ ] Secrets stored in environment variables / Keychain — never in source code

## 3. Type Safety (Important)

### Backend (TypeScript)

- [ ] No `any` type — use proper types, `unknown`, or generics
- [ ] Explicit return types on all functions
- [ ] Strict TypeScript config respected (`noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`, etc.)
- [ ] Zod schemas used for all request validation (body, query, params)
- [ ] Unused parameters prefixed with underscore (`_req`, `_next`)

### iOS (Swift)

- [ ] No force unwraps (`!`) — use optional binding, `guard let`, or nil-coalescing
- [ ] Avoid `Any` type — use proper protocols or generics
- [ ] `@MainActor` used for all UI-updating code
- [ ] Proper use of `@State`, `@StateObject`, `@EnvironmentObject`, `@Observable`
- [ ] `CodingKeys` used for snake_case → camelCase conversion when needed

## 4. Testing (Important)

- [ ] New functionality has test coverage (unit tests at minimum)
- [ ] Tests cover happy path, error cases, and edge cases
- [ ] Tests are isolated — no shared mutable state, no execution-order dependency
- [ ] External dependencies mocked/stubbed appropriately
- [ ] Test names describe the behavior being verified
- [ ] Existing tests still pass after changes

### Backend-Specific

- [ ] HTTP endpoint tests use Supertest with proper status code assertions
- [ ] Response body structure validated (`success`, `data`, `error` fields)

### iOS-Specific

- [ ] ViewModels tested independently of views
- [ ] Async tests use `async throws` pattern with XCTest

## 5. API Design (Important)

- [ ] Responses follow standard format: `{ success, data, meta }` or `{ success, error, meta }`
- [ ] Error responses include `code` and `message` fields
- [ ] Consistent naming conventions across endpoints
- [ ] Backwards compatibility maintained (or breaking changes documented)
- [ ] Swagger `@openapi` annotations updated when endpoints change
- [ ] iOS models in `Core/Models/` match backend DTOs

## 6. Code Quality (Moderate)

- [ ] Functions are focused — single responsibility, not overly complex
- [ ] No meaningful code duplication (extract shared logic)
- [ ] Names are descriptive and follow language conventions
- [ ] Error handling present — no ignored catch blocks or unhandled promise rejections
- [ ] No dead code (unused imports, unreachable branches, commented-out code)
- [ ] Multi-step operations use transactions where appropriate
- [ ] Parameters validated at function boundaries

## 7. Backend-Specific Rules

- [ ] ES module imports use `.js` extensions (`import { foo } from './bar.js'`)
- [ ] Imports ordered: builtin → external → internal, alphabetical within groups
- [ ] Zod schemas used for request validation — not manual `if` checks
- [ ] Database queries use parameterized `$1` placeholders — no string concatenation
- [ ] Response helpers from `src/utils/response.ts` used for consistent responses
- [ ] OpenAPI `@openapi` JSDoc annotations added for new/modified endpoints
- [ ] Environment config accessed through Zod-validated config, not raw `process.env`

## 8. iOS-Specific Rules

- [ ] SwiftUI preferred over UIKit unless UIKit is necessary
- [ ] Large view bodies extracted into sub-views for readability
- [ ] `async/await` preferred over Combine for new async code
- [ ] `Task { }` used for async work from synchronous contexts
- [ ] `.task` modifier used for load-on-appear patterns
- [ ] File sections organized with `// MARK: -` comments
- [ ] Accessibility labels and hints added for interactive elements
- [ ] Dynamic Type and Dark Mode supported
- [ ] Sensitive data stored in Keychain, preferences in UserDefaults
- [ ] Strings localized via `Localizable.strings` — no hardcoded user-facing text

## 9. Style & Formatting (Low — Let Tooling Handle)

- Run `npm run format` / `npm run lint:fix` (backend) or `swiftformat . && swiftlint` (iOS) before committing
- Only flag style issues that significantly impair readability
- Respect existing patterns in the codebase — don't refactor for preference

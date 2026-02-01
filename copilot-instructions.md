# Copilot Instructions — Gambling Golfer Monorepo

## Overview

This directory (`~/code/gambling-golfer`) contains the complete Gambling Golfer application—a native mobile app for golfers who play side games and tournaments. The codebase is organized as a monorepo with separate subdirectories for backend and frontend (iOS) code, each with their own git repositories.

**Always read the repo-specific `copilot-instructions.md` in each subdirectory before making changes there.**

---

## Repository Structure

```
gambling-golfer/
├── .github/                    # Shared org config, agents, templates
│   ├── copilot-instructions.md # This file (root-level guidance)
│   └── agents/                 # Shared Copilot agents
├── backend/                    # REST API server (separate git repo)
│   └── .github/copilot-instructions.md
├── iOS/                        # iOS mobile app (separate git repo)
│   └── .github/copilot-instructions.md
└── spec.md                     # Product specification (source of truth)
```

| Directory | Purpose | Stack | Git Repo |
|-----------|---------|-------|----------|
| `backend/` | REST API server | Node.js 20+, Express, TypeScript, PostgreSQL | Gambling-Golfer/backend |
| `iOS/` | iOS mobile app | Swift 5.9+, SwiftUI, iOS 17+, MVVM | Gambling-Golfer/iOS |
| `.github/` | Shared config | N/A | Gambling-Golfer/.github |

**Future surfaces**: Android app, web dashboard (not yet implemented)

---

## Local Development Setup

### Backend

```bash
cd backend

# One-command setup (installs deps, creates .env, starts Postgres)
npm run setup

# Start development server (hot reload on port 3000)
npm run dev

# Validate all checks pass
npm run validate
```

**Requirements**: Node.js 20.19+, Docker (for PostgreSQL)

### iOS

```bash
cd iOS

# Install tools (if not installed)
brew install xcodegen swiftlint swiftformat

# Generate Xcode project
xcodegen generate

# Open in Xcode
open GamblingGolfer.xcodeproj
```

Then build and run (⌘R) with a simulator. The app connects to `http://localhost:3000/api` by default.

**Requirements**: Xcode 16+, XcodeGen

### Running Full Stack

1. Start backend: `cd backend && npm run dev`
2. Open iOS: `cd iOS && open GamblingGolfer.xcodeproj`
3. Run iOS app (⌘R) - it will connect to the local backend

---

## Validation Commands

Always run these before completing work:

| Component | Command | Description |
|-----------|---------|-------------|
| Backend | `npm run validate` | Typecheck → Lint → Format → OpenAPI → Tests |
| iOS | `swiftformat . && swiftlint` | Format and lint Swift code |
| iOS | Build (⌘B) + Test (⌘U) | Compile and run test suite |

---

## Cross-Repository Coordination

### API Contract

- **Source of truth**: `backend/API.md` and Swagger at `http://localhost:3000/api-docs`
- iOS models in `iOS/GamblingGolfer/Core/Models/` must match backend DTOs
- Use `CodingKeys` for snake_case → camelCase conversion in Swift

### When Backend Changes Affect iOS

1. Update backend endpoint or DTO
2. Update `backend/API.md` documentation
3. Update corresponding iOS model in `Core/Models/`
4. Update iOS service layer if request/response format changed
5. Test end-to-end locally

### When iOS Exposes Backend Issues

If iOS work reveals a backend bug or missing endpoint:
1. Document the issue in the backend repo (Gambling-Golfer/backend)
2. Add label `blocking:ios` to indicate frontend is blocked
3. Link issues across repos using GitHub issue references
4. Consider implementing a workaround in iOS while backend is fixed

### Issue Dependencies

Use these patterns to track cross-repo dependencies:

```markdown
# In iOS issue
Blocked by: Gambling-Golfer/backend#42

# In backend issue  
Blocks: Gambling-Golfer/iOS#15
```

---

## Shared Conventions

### Code Quality

- Write clean, readable, maintainable code
- Follow language-specific best practices and idioms
- Add appropriate error handling
- Include tests for new functionality
- Document public APIs and complex logic

### Git Workflow

- Use conventional commit messages: `feat:`, `fix:`, `chore:`, `docs:`, `test:`
- Reference issue numbers in commits (e.g., "Closes #42")
- Keep PRs focused and reviewable
- Request Copilot review on PRs:
  ```bash
  gh pr edit <pr_number> --repo Gambling-Golfer/<repo> --add-reviewer "@copilot"
  ```

### Security

- NEVER commit secrets, API keys, or credentials
- Use environment variables for configuration
- Validate and sanitize all user inputs
- Follow principle of least privilege

### Testing

- Write tests for all new functionality
- Keep tests isolated and deterministic
- Test happy paths, error cases, and edge cases

---

## Shared Agents

This organization provides shared agents in `.github/agents/`:

| Agent | Description |
|-------|-------------|
| `issue-worker` | Implements GitHub issues end-to-end |
| `pr-reviewer` | Addresses PR review comments |
| `code-review` | Reviews code with focus on genuine issues |
| `test-writer` | Testing specialist |
| `explore` | Fast codebase exploration |
| `parallel-worker` | Orchestrates parallel work |
| `merge-resolver` | Resolves merge conflicts |

Repositories can override these with repo-specific agents of the same name.

---

## Notes for AI Copilots

### Working in This Monorepo

1. **Identify which repo you're working in** before making changes
2. **Read the repo-specific instructions** in `backend/.github/copilot-instructions.md` or `iOS/.github/copilot-instructions.md`
3. **Run validation in the correct directory** using that repo's commands
4. **Commit to the correct git repository** (each subdirectory is a separate repo)

### General Guidelines

- Prioritize small, safe changes
- Match existing code style and patterns
- Ask for clarification when requirements are unclear
- **Always run validation before completing work**
- **NEVER merge PRs without explicit user approval**

### Cross-Repository Considerations

- The iOS app consumes the backend REST API
- Keep API contracts in sync between repos
- Backend `API.md` is the source of truth for endpoints
- iOS models should match backend DTOs
- When making API changes, consider impact on both sides

### Labels for Issues

| Label | Purpose |
|-------|---------|
| `agent-ready` | Issue is ready for agent to pick up |
| `agent-working` | Agent is currently working on this |
| `agent-completed` | Agent finished, PR created |
| `priority:high/medium/low` | Determines work order |
| `size:small/medium/large` | Complexity hint |
| `type:feature/bug/chore/docs/test` | Categorization |
| `blocking:ios` | Backend issue blocking iOS work |
| `blocking:backend` | iOS issue requiring backend changes |

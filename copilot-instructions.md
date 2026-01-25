# Copilot Instructions — Gambling Golfer Organization

## Overview

This file provides organization-wide guidance for AI copilots working across Gambling Golfer repositories. Individual repositories may have additional instructions that supplement these guidelines.

---

## Organization Structure

| Repository | Purpose | Stack |
|------------|---------|-------|
| `backend` | REST API server | Node.js, Express, TypeScript, PostgreSQL |
| `iOS` | iOS mobile app | Swift, SwiftUI, MVVM |
| `.github` | Shared config, agents, templates | N/A |

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

### Labels for Issues

| Label | Purpose |
|-------|---------|
| `agent-ready` | Issue is ready for agent to pick up |
| `agent-working` | Agent is currently working on this |
| `agent-completed` | Agent finished, PR created |
| `priority:high/medium/low` | Determines work order |
| `size:small/medium/large` | Complexity hint |
| `type:feature/bug/chore/docs/test` | Categorization |

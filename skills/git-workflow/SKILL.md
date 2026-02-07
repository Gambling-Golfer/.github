---
name: git-workflow
description: Git workflow conventions for the Gambling Golfer project — commit messages, branch naming, PR creation, and Copilot review requests. Use this when committing, creating PRs, or managing branches.
---

# Git Workflow — Gambling Golfer

## Branch Naming

```
feat/<issue-number>-<short-description>    # New features
fix/<issue-number>-<short-description>     # Bug fixes
chore/<short-description>                  # Maintenance
docs/<short-description>                   # Documentation
test/<short-description>                   # Test-only changes
```

## Commit Messages

Use conventional commit format with issue references:

```
feat: add GameService with CRUD operations

Closes #24
```

Prefixes: `feat:`, `fix:`, `chore:`, `docs:`, `test:`

## Pre-Push Validation

**Always validate before pushing** — pushing triggers CI on any open PR.

### Backend
```bash
npm run format && npm run lint:fix    # Fix formatting/linting
npm run validate                       # Full validation suite
```

### iOS
```bash
swiftformat .                          # Format Swift files
swiftlint                              # Lint check
swiftformat --lint .                   # Verify formatting is clean
```

## Pull Request Creation

```bash
gh pr create \
  --title "feat: <description>" \
  --body "## Summary\n\n<what changed>\n\nCloses #<issue>" \
  --base main
```

Include:
- Clear title matching commit convention
- Summary of changes
- Link to the issue
- Checklist of completed acceptance criteria

## Requesting Copilot Review

```bash
gh pr edit <pr-number> --repo Gambling-Golfer/<repo> --add-reviewer "@copilot"
```

Wait for review to complete (~30 seconds), then check:
```bash
gh api repos/Gambling-Golfer/<repo>/pulls/<pr-number>/reviews \
  --jq '.[] | select(.user.login | contains("copilot"))'
```

## Resolving Review Threads

After fixing feedback, resolve threads via GraphQL:
```bash
gh api graphql -f query='mutation {
  resolveReviewThread(input: {threadId: "THREAD_ID"}) {
    thread { isResolved }
  }
}'
```

Only resolve threads where feedback has been genuinely addressed.

## Issue Labels

| Label | Purpose |
|-------|---------|
| `agent-working` | Agent is currently working on this |
| `agent-completed` | Agent finished, PR created |
| `agent-ready` | Issue is ready for agent to pick up |

## Cross-Repo Issue References

```markdown
# In iOS issue
Blocked by: Gambling-Golfer/backend#42

# In backend issue
Blocks: Gambling-Golfer/iOS#15
```

## Constraints

- **NEVER merge PRs without explicit user approval**
- Keep PRs focused and reviewable
- Reference issue numbers in all commits

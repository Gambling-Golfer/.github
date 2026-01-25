# Gambling Golfer Organization Configuration

This repository contains organization-wide GitHub configuration for the Gambling Golfer project.

## Contents

### `/profile/README.md`
Organization profile displayed on the GitHub org page.

### `/copilot-instructions.md`
Shared Copilot instructions that apply to all repositories in the organization. Individual repos can extend these with repo-specific instructions.

### `/agents/`
Shared Copilot agents available to all repositories:

| Agent | Description |
|-------|-------------|
| `issue-worker` | Implements GitHub issues end-to-end |
| `pr-reviewer` | Addresses PR review comments |
| `code-review` | Reviews code with focus on genuine issues |
| `test-writer` | Testing specialist |
| `explore` | Fast codebase exploration |
| `parallel-worker` | Orchestrates parallel work with git worktrees |
| `merge-resolver` | Resolves merge conflicts |

### `/ISSUE_TEMPLATE/`
Shared issue templates for all repositories.

## How It Works

GitHub automatically applies configuration from this `.github` repository to all repos in the organization:

- Agents in `agents/` are available org-wide
- Issue templates in `ISSUE_TEMPLATE/` are available org-wide
- Individual repos can override by creating their own versions

## Repositories

| Repository | Description |
|------------|-------------|
| [backend](https://github.com/Gambling-Golfer/backend) | Node.js/Express REST API |
| [iOS](https://github.com/Gambling-Golfer/iOS) | iOS mobile app (SwiftUI) |

# Claude Git Skill

Automates git workflows with simple conventional commits (`feat`, `api`, `chore`), PR creation, and optional auto-commit.

## Setup

1. Run `/git setup` to configure your provider (GitHub or Forgejo) and auth.
2. Add the following permissions to your project's `.claude/settings.local.json` so the skill can run without prompting:

```json
{
  "permissions": {
    "allow": [
      "Bash(git add:*)",
      "Bash(git commit:*)",
      "Bash(git push:*)",
      "Bash(git branch:*)",
      "Bash(git checkout:*)",
      "Bash(git switch:*)",
      "Bash(git diff:*)",
      "Bash(git log:*)",
      "Bash(git status:*)",
      "Bash(git remote:*)",
      "Bash(gh pr create:*)",
      "Bash(curl *api*:*)",
      "Read(~/.claude-git/config.json)"
    ]
  }
}
```

> **Note:** Skills cannot auto-approve permissions on their own. These must be added to your settings file manually.

## Commands

| Command | Description |
|---|---|
| `/git setup` | Interactive setup wizard |
| `/git commit` | Stage + commit with generated message |
| `/git push` | Push current branch to remote |
| `/git pr create` | Open a PR via provider API |
| `/git status` | Working tree status + recent log |
| `/git branch <name>` | Create and switch to branch |
| `/git config auto true/false` | Toggle auto-commit |
| `/git config show` | Print current config |

## Commit Types

| Type | Use when |
|---|---|
| `feat` | New features, bug fixes, any code changes |
| `api` | API-related changes (endpoints, requests, responses) |
| `chore` | Maintenance, config, docs, refactoring, tests |

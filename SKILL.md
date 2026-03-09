---
name: git
description: >
  Automates Git workflows with smart conventional commits, PR creation, and auto-commit after changes.
  Use this skill whenever the user mentions git, commits, pull requests, branches, or version control.
  Triggers on /git setup, /git commit, /git pr create, /git config, /git status, /git push, /git branch.
  Also triggers automatically after file changes when auto-commit is enabled (default: true).
  Supports GitHub (PAT or Git Credential Manager) and Forgejo (URL + PAT) providers.
---

# Claude Git Skill

Automates git with simple conventional commits (`feat`, `api`, `chore`), PR creation, and optional auto-commit after every change.

---

## Quick Reference

| Command | Description |
|---|---|
| `/git setup` | Interactive setup wizard |
| `/git commit` | Stage + commit with generated message |
| `/git push` | Push current branch to remote |
| `/git pr create` | Open a PR via provider API |
| `/git status` | Working tree status + recent log |
| `/git branch <n>` | Create and switch to branch |
| `/git config auto true/false` | Toggle auto-commit |
| `/git config show` | Print current config |

---

## Reference Files

Read the relevant file before executing any command:

| File | When to read |
|---|---|
| `references/setup.md` | `/git setup` — provider selection, auth, preferences |
| `references/commits.md` | `/git commit` or auto-commit — message generation, scopes, conventions |
| `references/pr.md` | `/git pr create` — PR creation for GitHub & Forgejo |
| `references/config.md` | `/git config` — reading/writing the config file |
| `references/auto-commit.md` | After any file change when `auto: true` |

Config is stored at `$HOME/.claude-git/config.json` — a single global config, never inside any repo.

---

## Startup Behavior

On every session start (or when any /git command is first run):
1. Check for `$HOME/.claude-git/config.json`
2. If missing → prompt the user to run `/git setup`
3. If `auto: true` → enable auto-commit watching for this session

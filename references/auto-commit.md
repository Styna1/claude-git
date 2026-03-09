# Auto-Commit Behavior

When `auto: true` in config, Claude Git automatically stages and commits changes after every file modification — without prompting the user.

---

## When to Auto-Commit

Trigger an auto-commit after **any** of the following:
- A file is created, edited, or deleted by Claude
- A task or subtask completes and leaves modified files
- The user's explicit instruction has been carried out (e.g., "refactor this function")

Do **not** auto-commit when:
- The user explicitly said "don't commit" or "I'll commit manually"
- `auto: false` in config
- No changes were actually made (clean working tree)
- A `/git commit` was just run manually (avoid double-commit)
- Inside a `/git setup` or `/git config` flow

---

## Auto-Commit Flow

1. Check `auto` value in `$HOME/.claude-git/config.json` — if `false`, skip entirely
2. Run `git status --porcelain` — if empty, skip (nothing changed)
3. Read `references/commits.md` to generate the commit message
4. Run:
   ```bash
   git add -A
   git commit -m "<type>(<scope>): <description>" [-m "<body>"]
   ```
5. Print a compact one-liner (don't be verbose):
   > `✅ Auto-committed: feat(auth): add login form validation`

---

## Batching Changes

If multiple files were modified as part of a single logical task, commit them **together** as one commit — not one commit per file.

Example: If you edit `src/auth/login.ts`, `src/auth/types.ts`, and `tests/auth.test.ts` as part of one task:
```
feat(auth): add login form validation with type definitions
```
Not three separate commits.

---

## Notifying the User

Keep auto-commit notifications minimal. A single line is enough:
> `✅ Auto-committed: chore(config): update eslint rules`

Do not ask for confirmation before auto-committing. The whole point is to be silent and automatic. If the user wants to review commits, they can run `/git status` at any time.

---

## Disabling Mid-Session

The user can disable auto-commit at any time:
- `/git config auto false` — turns off for this repo permanently
- Saying "don't auto-commit" or "stop committing automatically" — turn off for the rest of the session only (set an in-memory flag, don't write to config unless asked)

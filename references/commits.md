# Commit Message Generation

Used for both `/git commit` and auto-commit. Always generate a conventional commit message by analyzing the staged diff.

---

## Conventional Commit Format

```
<type>: <short description>

[optional body]
```

- **type** — one of the three types below
- **short description** — imperative, lowercase, no period, max 72 chars
- **body** — optional, used when the change needs more context (add after a blank line)

---

## Types

| Type | Use when... |
|---|---|
| `feat` | A new feature, capability, bug fix, or any code change |
| `api` | API-related changes (endpoints, requests, responses, integrations) |
| `chore` | Maintenance, dependency updates, tooling, config, docs, refactoring, tests |

---

## Message Generation Steps

1. Run `git diff --staged` (or `git diff HEAD` for auto-commit)
2. Analyze: what files changed, what was added/removed/modified
3. Pick the most appropriate `type` from the three above
4. Write a concise imperative description
5. If the change is complex or non-obvious, add a short body paragraph

---

## Examples

```
feat: add JWT refresh token support

Implements sliding session expiry using refresh tokens stored in
httpOnly cookies. Access tokens now expire after 15 minutes.
```

```
feat: handle null input in parser
```

```
chore: update eslint to v9
```

```
api: extract request validation middleware
```

```
chore: add setup instructions for Windows
```

---

## Execution

```bash
# Stage all changes
git add -A

# Commit with generated message
git commit -m "<type>: <description>" -m "<optional body>"
```

If nothing is staged (clean working tree), output:
> "Nothing to commit — working tree is clean."

If commit succeeds, print the short commit hash and message:
> `✅ Committed: abc1234 — feat: add JWT refresh token support`

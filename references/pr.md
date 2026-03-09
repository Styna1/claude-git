# /git pr create — Pull Request Creation

Creates a PR against the base branch using the provider's REST API. Requires a PAT (or GCM with API scope for GitHub).

---

## Pre-flight Checks

Before creating the PR:

1. Load config from `.claude-git/config.json`
2. Confirm a PAT is available (`auth_method: "pat"` or GCM token retrievable)
3. Run `git status` — ensure there are no uncommitted changes (if `auto: true`, commit first)
4. Get current branch: `git branch --show-current` → this is the **head branch**
5. Get default branch (base): `git remote show origin | grep 'HEAD branch'` → typically `main` or `master`
6. Ensure current branch has been pushed: `git push -u origin <branch>` if needed

---

## Gather PR Details

Do NOT prompt the user — always infer automatically:

- **Title** — last commit message subject line
- **Description** — generate from `git log <base>..<head> --oneline` summary
- **Base branch** — repo default branch
- **Draft?** — always `false`

Never ask for confirmation. Use all defaults and create the PR immediately.

---

## GitHub PR Creation

```
POST https://api.github.com/repos/{owner}/{repo}/pulls
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "<title>",
  "body": "<description>",
  "head": "<current-branch>",
  "base": "<base-branch>",
  "draft": false
}
```

Get `owner` and `repo` from: `git remote get-url origin`
- SSH format: `git@github.com:owner/repo.git`
- HTTPS format: `https://github.com/owner/repo.git`

On success (HTTP 201), print:
> `✅ PR created: https://github.com/owner/repo/pull/<number>`

---

## Forgejo PR Creation

```
POST <provider_url>/api/v1/repos/{owner}/{repo}/pulls
Authorization: token <pat>
Content-Type: application/json

{
  "title": "<title>",
  "body": "<description>",
  "head": "<current-branch>",
  "base": "<base-branch>"
}
```

Get `owner` and `repo` from: `git remote get-url origin`

On success (HTTP 201), print:
> `✅ PR created: <provider_url>/owner/repo/pulls/<number>`

---

## Error Handling

| Error | Message to user |
|---|---|
| 401 Unauthorized | "Auth failed — your PAT may be expired or missing `repo` scope. Run `/git setup` to update it." |
| 422 Unprocessable | "PR already exists for this branch, or head/base branches are invalid." |
| No commits ahead | "Your branch has no new commits compared to `<base>`. Push some changes first." |
| Not pushed | "Branch hasn't been pushed yet. Pushing now..." then retry. |

---

## GCM + GitHub API

If `auth_method: "gcm"`, retrieve the token via:
```bash
echo "protocol=https\nhost=github.com" | git credential fill
```
Use the returned `password` field as the Bearer token for the API call.

# /git setup — Setup Wizard

Run this interactive wizard to configure Claude Git for the current repo. Walk the user through each step in order. Do not skip steps.

---

## Step 1: Provider Selection

Ask:
> "Which Git provider are you using?"
> 1. **GitHub**
> 2. **Forgejo**

### If GitHub selected:
Proceed to Step 2a.

### If Forgejo selected:
Ask:
> "What is your Forgejo instance URL? (e.g. https://git.example.com)"

Validate it starts with `https://` or `http://`. Store as `provider_url`. Proceed to Step 2b.

---

## Step 2a: GitHub Authentication

Ask:
> "How would you like to authenticate with GitHub?"
> 1. **Git Credential Manager** (recommended — uses your existing GCM setup, no token needed)
> 2. **Personal Access Token (PAT)** — enter a token + username manually

### Option 1 — Git Credential Manager:
- Set `auth_method: "gcm"` in config
- No token stored
- Verify GCM is available: run `git credential-manager --version` (or `git credential fill`) — if not found, warn the user and suggest installing from https://aka.ms/gcm

### Option 2 — GitHub PAT:
Ask:
> "Enter your GitHub username:"

Then:
> "Enter your GitHub Personal Access Token:"
> Your PAT needs these scopes: `repo`, `workflow` (for PRs). Generate one at https://github.com/settings/tokens

- Set `auth_method: "pat"`
- Store `username` and `token` in config
- Mask token in all output (show only last 4 chars: `****abcd`)

---

## Step 2b: Forgejo Authentication

Ask:
> "Enter your Forgejo username:"

Then:
> "Enter your Forgejo Personal Access Token:"
> Generate one in your Forgejo instance under Settings → Applications → Access Tokens.
> Required permissions: `repository` (read+write), `issue` (for PRs).

- Set `auth_method: "pat"`
- Store `username`, `token`, and `provider_url` in config
- Mask token in all output

---

## Step 3: Auto-Commit Preference

Ask:
> "Would you like Claude Git to automatically commit changes after every file modification?"
> **[Y/n]** (default: yes)

- `y` / enter → `auto: true`
- `n` → `auto: false`

Explain if they choose yes:
> "Auto-commit is on. After each change, I'll stage and commit with a generated conventional commit message. You can turn this off anytime with `/git config auto false`."

---

## Step 4: Write Config

Write the config to `$HOME/.claude-git/config.json` (create the directory if needed). This is a global location — never inside any repo, so no `.gitignore` entry is required.

```json
{
  "provider": "github" | "forgejo",
  "provider_url": "https://git.example.com",  // Forgejo only
  "auth_method": "gcm" | "pat",
  "username": "your-username",                // PAT only
  "token": "ghp_...",                         // PAT only
  "auto": true | false
}
```

Confirm:
> "✅ Claude Git is set up! Config saved to `~/.claude-git/config.json`."
> "Run `/git status` to check your repo, or make a change to trigger an auto-commit."

---

## Notes

- Never log or display the full token — always mask it
- If re-running `/git setup` over an existing config, warn: "A config already exists. Running setup will overwrite it. Continue? [y/N]"
- Config lives in `$HOME` — completely outside any repo, no `.gitignore` needed

# /git config — Configuration Management

Manages the Claude Git config file at `$HOME/.claude-git/config.json`. This is a **global config** — one file for all repos, never stored inside any project directory.

---

## Config Schema

```json
{
  "provider": "github" | "forgejo",
  "provider_url": "https://git.example.com",
  "auth_method": "gcm" | "pat",
  "username": "string",
  "token": "string",
  "auto": true | false
}
```

| Field | Required | Description |
|---|---|---|
| `provider` | always | `github` or `forgejo` |
| `provider_url` | Forgejo only | Base URL of the Forgejo instance |
| `auth_method` | always | `gcm` (GitHub only) or `pat` |
| `username` | PAT only | Git username |
| `token` | PAT only | Personal Access Token |
| `auto` | always | Auto-commit after changes (default: `true`) |

---

## Config Path

Always use: `$HOME/.claude-git/config.json`

```bash
CONFIG_DIR="$HOME/.claude-git"
CONFIG_FILE="$CONFIG_DIR/config.json"
mkdir -p "$CONFIG_DIR"
```

Never place config inside a repo. This keeps credentials out of version control entirely — no `.gitignore` entry needed.

---

## /git config auto true|false

Toggle auto-commit behavior:

```bash
CONFIG_FILE="$HOME/.claude-git/config.json"

# Read current value
python3 -c "import json; c=json.load(open('$CONFIG_FILE')); print(c.get('auto', True))"

# Update value
python3 -c "
import json
path = '$CONFIG_FILE'
with open(path, 'r') as f:
    c = json.load(f)
c['auto'] = True  # or False
with open(path, 'w') as f:
    json.dump(c, f, indent=2)
"
```

Confirm to user:
> `✅ Auto-commit set to: true` (or `false`)

---

## /git config show

Display the current config. **Always mask the token** — show only last 4 characters:

```
Provider:     github
Auth method:  pat
Username:     octocat
Token:        ****3f9a
Auto-commit:  true
Config:       ~/.claude-git/config.json
```

For GCM:
```
Provider:     github
Auth method:  Git Credential Manager
Auto-commit:  true
Config:       ~/.claude-git/config.json
```

---

## Security Notes

- Never print the full token anywhere — always mask it
- Config lives in `$HOME`, completely outside any repo — no risk of accidental commits

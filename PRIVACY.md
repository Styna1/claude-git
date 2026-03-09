# Privacy Policy

## Data Collection

Claude Git does **not** collect, transmit, or store any user data on external servers. All operations run locally within your Claude Code environment.

## Local Storage

The skill stores a single configuration file at `~/.claude-git/config.json` on your machine. This file contain:

- Your chosen git provider (GitHub or Forgejo)
- A personal access token (PAT) for API authentication
- Your auto-commit preference

This file never leaves your local filesystem.

## Third-Party Services

When you use commands like `/git pr create` or `/git push`, the skill communicates directly with your configured git provider (GitHub or Forgejo) using your own credentials. No intermediate servers are involved.

## Data Sharing

Claude Git does not share any data with Anthropic, the skill author, or any third party beyond the git provider you explicitly configure.

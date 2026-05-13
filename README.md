# CodexAuth

CLI tool for managing multiple Codex/OpenAI accounts and automatic token refresh.

## Features

- **Multi-account management** — save, switch between, and delete Codex authentication profiles
- **Interactive profile picker** — arrow keys + Enter menu for easy account switching
- **Automatic token refresh** — hourly cron job refreshes tokens for all saved profiles
- **Simple CLI** — bash-based, no external dependencies beyond standard tools

## Installation

```bash
curl -fsSL https://raw.githubusercontent.com/crewyyyy/CodexAuth/main/acodex -o ~/.local/bin/acodex
curl -fsSL https://raw.githubusercontent.com/crewyyyy/CodexAuth/main/acodex-refresh -o ~/.local/bin/acodex-refresh
chmod +x ~/.local/bin/acodex ~/.local/bin/acodex-refresh
```

Make sure `~/.local/bin` is in your `PATH`:
```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## Setup Auto-Refresh

Install the hourly cron job:
```bash
(crontab -l 2>/dev/null | grep -v "acodex-refresh"; echo "0 * * * * export PATH=\$HOME/.local/bin:\$PATH && \$HOME/.local/bin/acodex-refresh >> \$HOME/.codex/logs/acodex-refresh.log 2>&1") | crontab -
```

## Usage

### Save Account
```bash
acodex save              # interactive name input
acodex save work         # save as 'work' profile
```

### Switch Account
```bash
acodex select            # interactive arrow-key menu
acodex select work       # direct switch by name
```

### Manage Profiles
```bash
acodex list              # show all saved profiles
acodex current           # show current account info
acodex delete old        # delete 'old' profile
acodex remove old        # alias for delete
```

### Manual Token Refresh
```bash
acodex-refresh
tail -f ~/.codex/logs/acodex-refresh.log
```

## Interactive Mode

When running `acodex select` without arguments, an interactive menu appears:

```
  Select profile:

  ▶ account1 (current)
    work
    personal

  ↑↓ navigate  Enter select  Esc cancel
```

Use **arrow keys** (↑↓) to navigate, **Enter** to select, **Esc** to cancel.

## Storage

- Profiles stored in `~/.codex/profiles/`
- Current profile marker: `~/.codex/profiles/.current`
- Backups of auth.json before switch: `~/.codex/backups/`
- Refresh logs: `~/.codex/logs/acodex-refresh.log`

## Requirements

- bash
- python3
- curl
- cron (for auto-refresh)
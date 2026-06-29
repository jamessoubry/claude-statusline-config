# claude-statusline-config

Custom [Claude Code](https://claude.ai/code) statusline using [starship-claude](https://github.com/martinemde/starship-claude) and [Starship](https://starship.rs/).

The same `starship.toml` drives both the **zsh prompt** and the **Claude Code statusline** — the config detects which context it's running in and shows different segments accordingly.

## What it looks like

**In Claude Code** (statusline):

- **Time** — current time in orange
- **Context %** — Claude context window usage (green < 80%, blinking red ≥ 80%)
- **Directory** — current working directory in blue
- **Git branch + status** — in green with symbol
- **Model** — current Claude model with NerdFont icon (haiku / sonnet / opus)

**In zsh** (terminal prompt):

- **Time** — current time in orange
- **Directory** — current working directory in blue
- **Git branch + status** — in green with symbol

The Claude-specific segments (`context %`, `model`) are hidden in the terminal because `CLAUDE_CONTEXT` is only set when `starship-claude` invokes Starship. In terminal mode, a simple orange→blue arrow replaces the context segment.

## Setup

### 1. Install dependencies

```bash
# Starship
brew install starship

# starship-claude (the bridge script)
curl -fsSL https://raw.githubusercontent.com/martinemde/starship-claude/main/install.sh | bash
# or manually: download starship-claude to ~/.local/bin/starship-claude and chmod +x
```

### 2. Copy the Starship config

```bash
cp starship.toml ~/.claude/starship.toml
```

### 3. Configure zsh

Add to `~/.zshrc`:

```zsh
export STARSHIP_CONFIG=~/.claude/starship.toml
eval "$(starship init zsh)"
```

### 4. Add to Claude Code settings

Add the following to `~/.claude/settings.json`:

```json
{
  "statusLine": {
    "type": "command",
    "command": "STARSHIP_CONFIG=~/.claude/starship.toml ~/.local/bin/starship-claude"
  }
}
```

### 5. Restart Claude Code

The statusline is only loaded at startup.

## Notes

- Requires a [NerdFont](https://www.nerdfonts.com/) in your terminal for the model icons to render
- Context % uses `CLAUDE_PERCENT_RAW` exported by `starship-claude` — see that project for the full list of available variables
- The `CLAUDE_MODEL_NERD` env var is used for the model segment (e.g., `󰚩 sonnet 4.6`)

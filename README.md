# Term-Config

Terminal configuration for **fastfetch**, **Oh My Posh**, and **tmux**, styled with Tokyo Night.

## Contents

| Path | Description | Local destination |
|------|-------------|-------------------|
| `fastfetch/config.jsonc` | System info display | `~/.config/fastfetch/config.jsonc` |
| `oh-my-posh/theme.json` | Prompt theme | `~/.config/oh-my-posh/theme.json` |
| `tmux/tmux.conf` | Tmux core + Tokyo Night status bar | `~/.tmux.conf` |

## Prerequisites

- [fastfetch](https://github.com/fastfetch-cli/fastfetch)
- [Oh My Posh](https://ohmyposh.dev/)
- [tmux](https://github.com/tmux/tmux)
- [TPM](https://github.com/tmux-plugins/tpm) (for tmux plugins declared in `tmux.conf`)

## Installation

From the repository root:

```bash
# fastfetch
mkdir -p ~/.config/fastfetch
ln -sf "$PWD/fastfetch/config.jsonc" ~/.config/fastfetch/config.jsonc

# oh-my-posh
mkdir -p ~/.config/oh-my-posh
ln -sf "$PWD/oh-my-posh/theme.json" ~/.config/oh-my-posh/theme.json

# tmux
ln -sf "$PWD/tmux/tmux.conf" ~/.tmux.conf
```

Point Oh My Posh at the theme in your shell config, for example:

```bash
eval "$(oh-my-posh init zsh --config ~/.config/oh-my-posh/theme.json)"
```

Reload tmux after linking:

```bash
tmux source-file ~/.tmux.conf
```

# Term-Config

Terminal configuration for **fastfetch**, **Oh My Posh**, and **tmux**, styled with [Tokyo Night](https://github.com/folke/tokyonight.nvim).

## Features

- **fastfetch** — Tokyo Night colored system info (OS, host, CPU, GPU, memory, disk, and more)
- **Oh My Posh** — Diamond/powerline prompt with session, path, git, Node, Docker, and AWS segments
- **tmux** — Status bar on top, vi copy-mode with `pbcopy` on macOS, mouse support, and TPM plugins:
  - [tpm](https://github.com/tmux-plugins/tpm)
  - [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator)
  - [tmux-resurrect](https://github.com/tmux-plugins/tmux-resurrect)
  - [tmux-continuum](https://github.com/tmux-plugins/tmux-continuum) (auto-restore)

## Contents

| Path | Description | Local destination |
|------|-------------|-------------------|
| `fastfetch/config.jsonc` | System info display | `~/.config/fastfetch/config.jsonc` |
| `oh-my-posh/theme.json` | Prompt theme | `~/.config/oh-my-posh/theme.json` |
| `tmux/tmux.conf` | Tmux core + Tokyo Night status bar | `~/.tmux.conf` |

## Prerequisites

Install the tools for your OS, then clone TPM (required by `tmux.conf`).

A [Nerd Font](https://www.nerdfonts.com/) is recommended so Oh My Posh and tmux icons render correctly (for example, JetBrainsMono Nerd Font or MesloLGM Nerd Font).

### macOS

```bash
brew install fastfetch tmux git
brew install jandedobbeleer/oh-my-posh/oh-my-posh
```

### Ubuntu

```bash
sudo apt update
sudo apt install -y tmux git curl unzip

# fastfetch (Ubuntu 24.04+)
sudo apt install -y fastfetch

# Oh My Posh
curl -s https://ohmyposh.dev/install.sh | bash -s
```

On older Ubuntu releases without `fastfetch` in apt, install from the [official releases](https://github.com/fastfetch-cli/fastfetch/releases).

Ensure `~/.local/bin` (or the install path printed by the script) is on your `PATH`.

### Fedora

```bash
sudo dnf install -y fastfetch tmux git curl unzip
curl -s https://ohmyposh.dev/install.sh | bash -s
```

Ensure `~/.local/bin` (or the install path printed by the script) is on your `PATH`.

### TPM (all platforms)

```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

## Installation

### 1. Clone this repository

```bash
git clone git@github.com:GabrielAlmeidaFlores/Term-Config.git
cd Term-Config
```

### 2. Symlink the configs

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

### 3. Initialize Oh My Posh in your shell

**zsh** (`~/.zshrc`):

```bash
eval "$(oh-my-posh init zsh --config ~/.config/oh-my-posh/theme.json)"
```

**bash** (`~/.bashrc`):

```bash
eval "$(oh-my-posh init bash --config ~/.config/oh-my-posh/theme.json)"
```

Reload the shell:

```bash
exec "$SHELL"
```

### 4. Load tmux and install plugins

```bash
tmux source-file ~/.tmux.conf
```

Inside tmux, press `prefix` + `I` (capital i) to install TPM plugins.

Default tmux prefix is `Ctrl-b`.

### Optional: run fastfetch on shell startup

```bash
# Avoid running again inside every new tmux pane
if [[ -z "$FASTFETCH_HAS_RUN" ]]; then
  fastfetch
  export FASTFETCH_HAS_RUN=1
fi
```

## Verify

```bash
fastfetch
oh-my-posh --version
tmux -V
```

## Notes

- `tmux.conf` uses `pbcopy` for vi-mode yank. On Linux, replace that binding with `xclip` or `wl-copy` if needed.
- Continuum restore is enabled (`@continuum-restore on`). Sessions are restored automatically when tmux starts.
- Theme colors follow Tokyo Night (`#7aa2f7`, `#3b4261`, `#15161e`, `#c0caf5`).

## License

Personal configuration. Use and adapt as you like.

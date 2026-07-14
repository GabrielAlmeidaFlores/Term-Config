# Term-Config

Terminal configuration for **Alacritty**, **fastfetch**, **Oh My Posh**, **tmux**, and **eza**, styled with [Tokyo Night](https://github.com/folke/tokyonight.nvim).

## Features

- **Alacritty** — Tokyo Night colors, JetBrainsMono Nerd Font, transparency with blur
- **fastfetch** — Tokyo Night colored system info (OS, host, CPU, GPU, memory, disk, and more)
- **Oh My Posh** — Diamond/powerline prompt with session, path, git, Node, Docker, and AWS segments
- **tmux** — Status bar on top, vi copy-mode with `pbcopy` on macOS, mouse support, and TPM plugins:
  - [tpm](https://github.com/tmux-plugins/tpm)
  - [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator)
  - [tmux-resurrect](https://github.com/tmux-plugins/tmux-resurrect)
  - [tmux-continuum](https://github.com/tmux-plugins/tmux-continuum) (auto-restore)
- **eza** — Modern `ls` replacement with icons and git status

## Contents

| Path | Description | Local destination |
|------|-------------|-------------------|
| `alacritty/alacritty.toml` | Terminal emulator config | `~/.config/alacritty/alacritty.toml` |
| `fastfetch/config.jsonc` | System info display | `~/.config/fastfetch/config.jsonc` |
| `oh-my-posh/theme.json` | Prompt theme | `~/.config/oh-my-posh/theme.json` |
| `tmux/tmux.conf` | Tmux core + Tokyo Night status bar | `~/.tmux.conf` |

## Prerequisites

Supported systems: **macOS**, **Ubuntu**, and **Fedora**.

Install all tools for your OS, then clone TPM (required by `tmux.conf`).

A [Nerd Font](https://www.nerdfonts.com/) is required so Alacritty, Oh My Posh, tmux, and eza icons render correctly (JetBrainsMono Nerd Font is used in `alacritty.toml`).

### Nerd Font

**macOS:**

```bash
brew install --cask font-jetbrains-mono-nerd-font
```

**Ubuntu / Fedora:**

```bash
mkdir -p ~/.local/share/fonts
curl -fLo /tmp/JetBrainsMono.zip \
  https://github.com/ryanoasis/nerd-fonts/releases/download/v3.3.0/JetBrainsMono.zip
unzip -o /tmp/JetBrainsMono.zip -d ~/.local/share/fonts/
fc-cache -fv
```

### macOS

```bash
brew install alacritty eza fastfetch tmux git
brew install jandedobbeleer/oh-my-posh/oh-my-posh
```

### Ubuntu

```bash
sudo apt update
sudo apt install -y git tmux curl unzip xclip wl-clipboard

# Alacritty (22.04+)
sudo apt install -y alacritty

# fastfetch (24.04+)
sudo apt install -y fastfetch

# eza (24.04+)
sudo apt install -y eza

# Oh My Posh
curl -s https://ohmyposh.dev/install.sh | bash -s
```

On older Ubuntu releases without packages in apt:

```bash
# fastfetch
curl -sL https://api.github.com/repos/fastfetch-cli/fastfetch/releases/latest \
  | grep browser_download_url \
  | grep linux-amd64 \
  | cut -d '"' -f 4 \
  | xargs curl -Lo /tmp/fastfetch.deb
sudo dpkg -i /tmp/fastfetch.deb

# eza
sudo mkdir -p /etc/apt/keyrings
curl -sS https://raw.githubusercontent.com/eza-community/eza/main/deb.asc \
  | sudo gpg --dearmor -o /etc/apt/keyrings/gierens.gpg
echo "deb [signed-by=/etc/apt/keyrings/gierens.gpg] http://deb.gierens.de stable main" \
  | sudo tee /etc/apt/sources.list.d/gierens.list
sudo apt update && sudo apt install -y eza

# Alacritty — install from official releases
curl -sL https://api.github.com/repos/alacritty/alacritty/releases/latest \
  | grep browser_download_url \
  | grep 'Alacritty-v.*-ubuntu.*\.deb' \
  | cut -d '"' -f 4 \
  | xargs curl -Lo /tmp/alacritty.deb
sudo dpkg -i /tmp/alacritty.deb
```

Ensure `~/.local/bin` (or the path printed by the Oh My Posh install script) is on your `PATH`.

### Fedora

```bash
sudo dnf install -y alacritty eza fastfetch tmux git curl unzip xclip wl-clipboard
curl -s https://ohmyposh.dev/install.sh | bash -s
```

Ensure `~/.local/bin` (or the path printed by the Oh My Posh install script) is on your `PATH`.

### TPM

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
# alacritty
mkdir -p ~/.config/alacritty
ln -sf "$PWD/alacritty/alacritty.toml" ~/.config/alacritty/alacritty.toml

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

### 4. Configure eza

Install eza first (see **Prerequisites** for macOS, Ubuntu, or Fedora).

Add these aliases to your shell config (`~/.zshrc` or `~/.bashrc`):

```bash
# Aliases para eza
alias ls='eza --icons --group-directories-first'
alias ll='eza -lh --icons --git --group-directories-first'
alias la='eza -lah --icons --git --group-directories-first'
alias tree='eza --tree --icons'
```

Reload the shell:

```bash
exec "$SHELL"
```

### 5. Load tmux and install plugins

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
alacritty --version
fastfetch
oh-my-posh --version
tmux -V
eza --version
ls
```

## Notes

- `tmux.conf` uses `pbcopy` for vi-mode yank on macOS. On Ubuntu/Fedora, replace that binding with `xclip` or `wl-copy` (both are listed in the Linux prerequisites).
- Continuum restore is enabled (`@continuum-restore on`). Sessions are restored automatically when tmux starts.
- Theme colors follow Tokyo Night (`#7aa2f7`, `#3b4261`, `#15161e`, `#c0caf5`).

## License

Personal configuration. Use and adapt as you like.

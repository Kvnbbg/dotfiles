# My Dotfiles

A collection of my personalized configurations and scripts to set up a new development environment.

### ⚙️ What's Included
- **Bash & Zsh:** Custom aliases, functions, and a tailored shell experience.
- **Git:** My global `.gitconfig` settings for user info, aliases, and more.
- **Tmux:** My `tmux.conf` for a consistent, multi-pane terminal setup.
- **Custom Scripts:** Utility scripts to automate tasks and keep everything in sync.

---

### ⚠️ Note on VS Code

My VS Code settings are not included in this repository. Visual Studio Code has a built-in **Settings Sync** feature that is the recommended way to keep your editor preferences, extensions, and keybindings synchronized across multiple machines.

To use it, simply sign in with your GitHub or Microsoft account within VS Code.

---

### 🚀 Usage

My `dotctl` script is designed to make setup and maintenance easy.

**Install:** Quickly set up all dotfiles on a new machine. This command symlinks the files from this repository to your home directory.

```bash
dotctl install
```

Update: Pull the latest changes from the remote repository and apply them. This is useful for keeping your dev environment synchronized across multiple machines.

```bash
dotctl update
```
Update: Pull the latest changes from the remote repository and apply them. This is useful for keeping your dev environment synchronized across multiple machines.

```bash
dotctl diff
```

### dotctl Script Structure (Conceptual)

Your dotctl script will likely be a simple bash script that uses a case statement to handle different subcommands.

```bash
#!/usr/bin/env bash

# This script should be run from the dotfiles repository root.
DOTFILES_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" >/dev/null 2>&1 && pwd)"
cd "$DOTFILES_DIR"

# --- Main Script Logic ---
case "$1" in
    install)
        echo "Installing dotfiles..."
        # Symlink files to home directory
        ln -sf "$DOTFILES_DIR/.bashrc" "$HOME/.bashrc"
        ln -sf "$DOTFILES_DIR/.zshrc" "$HOME/.zshrc"
        ln -sf "$DOTFILES_DIR/.gitconfig" "$HOME/.gitconfig"
        # Add more ln commands for other dotfiles

        echo "Dotfiles installed successfully."
        ;;

    update)
        echo "Updating..."
        # Pull latest changes from Git
        git pull origin main
        # Add any other update commands here, e.g., for Zsh plugins
        # Example: if you use a Zsh plugin manager like zplug or zgen, you would add the update command here.
        # zgen update
        # zplug update

        echo "Update complete."
        ;;
        
    diff)
        echo "Showing diff..."
        git status
        git diff
        ;;

    *)
        echo "Usage: dotctl [install|update|diff]"
        exit 1
        ;;
esac
```

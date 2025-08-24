# My Dotfiles

A collection of my personalized configurations and an **omnibus** script to manage everything. This repository is my single source of truth for a consistent and efficient development environment.

-----

### ⚙️ What's Included

  - **Shells:** Custom `.bashrc` and `.zshrc` for Bash and Zsh.
  - **Git:** My global `.gitconfig` file with useful aliases and settings.
  - **Tmux:** My `.tmux.conf` for a consistent, multi-pane terminal setup.
  - **`dotctl`:** A custom script to manage the installation and updates of all dotfiles.

-----

### 🚀 Usage

My `dotctl` script is designed to make setup and maintenance easy.

**Install:** Quickly set up all dotfiles on a new machine. This command creates symbolic links from this repository to your home directory.

```bash
dotctl install
```

**Update:** Pull the latest changes from the remote repository and apply them. This is useful for keeping your dev environment synchronized across multiple machines.

```bash
dotctl update
```

**Diff:** View and manage differences between your local dotfiles and the repository.

```bash
dotctl diff
```

-----

### ⚠️ A Note on Sensitive Files

You should **never** store sensitive information like API keys, passwords, or machine-specific data in a public Git repository. To manage these files, we use a combination of a dedicated secrets file and `.gitignore`.

#### 1\. The Secrets File

Instead of putting secrets directly in your dotfiles, store them in a separate, unversioned file like `.secrets` or `.env`. Then, `source` that file from your main shell configuration.

**In your `~/.bashrc` file:**

```bash
# Load my private environment variables
source ~/.secrets
```

**In your `~/.secrets` file (which is NOT in Git):**

```
export API_KEY="your_private_api_key_here"
export DATABASE_PASSWORD="your_secret_password"
```

This way, your shell can access the secrets, but they are not exposed in your public repository.

#### 2\. The `.gitignore` File

The `.gitignore` file is essential for telling Git which files to ignore and not upload to your repository. This is how you keep your secrets safe.

**Add these lines to your `~/dotfiles/.gitignore` file:**

```
# Exclude sensitive files from the repository
.secrets
.env
.profile
```

This ensures that your private files remain on your local machine and are never accidentally committed.

-----

### How to Manage Your Dotfiles with Git

Managing dotfiles with Git requires using **symbolic links** (or symlinks), which act as pointers from the old file location to the new one inside your repository.

#### What is a Symbolic Link?

A symbolic link is a special file that points to another file or directory. When your system tries to access the symlink, it is automatically redirected to the target file. It's like a desktop shortcut.

#### How to Create a Symbolic Link

You use the `ln` command with the `-s` flag. The syntax is:

```bash
ln -s [target_file] [link_name]
```

**Example:**
To manage your `~/.bashrc` file, you would:

1.  **Move the original file:**
    ```bash
    mv ~/.bashrc ~/dotfiles/.bashrc # or && mv *.sh ~/dotfiles/ && mv *.env ~/dotfiles/
    ```
2.  **Create the symbolic link:**
    ```bash
    ln -s ~/dotfiles/.bashrc ~/.bashrc
    ```

This allows your system to find `.bashrc` where it expects to, while the actual file is safely managed inside your Git repository.

-----

### `dotctl` Script Structure (Conceptual)

Your `dotctl` script is a simple Bash script that uses a `case` statement to handle different subcommands.

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

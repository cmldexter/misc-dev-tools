# misc-dev-tools
Collection of mini dev tools and scripts to speed up a dev environment

## How to Install

To make these scriptlets globally available on your system, you can add the `bin` directory to your shell's `$PATH`. 

Add the following snippet to your shell profile (`~/.zshrc` or `~/.bashrc`):

```bash
# Add misc-dev-tools to PATH
export PATH="$HOME/Documents/GitHub/misc-dev-tools/bin:$PATH"
```

After modifying your profile, reload your shell (e.g. `source ~/.zshrc`) or restart your terminal. You can then run any script in the `bin/` directory (like `sync-gitlab-github`) directly from any folder.

## Scripts

### `sync-gitlab-github`
Fetches all remote branches from a specified `github` remote and pushes them directly to an `origin` (GitLab) remote to ensure both repositories are synchronized. 

**When to use:** Use this script when you maintain mirrors between GitHub and GitLab and need a quick, forceful synchronization to pull all branches from GitHub and push them identically to GitLab. It's particularly useful in cases where automatic CI/CD mirroring fails or trails behind.

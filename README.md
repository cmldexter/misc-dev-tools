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

#### Troubleshooting: GitHub `main` is ahead of GitLab

If the sync script fails because `origin/main` (GitLab) has diverged or is behind `github/main`, you can manually reconcile the branches with the following steps:

```bash
# 1. Prune stale remote-tracking refs and fetch latest state
git fetch --prune origin

# 2. Reset your local main to match GitHub's main exactly
git checkout main
git reset --hard github/main

# 3. Merge GitLab's main into your (now GitHub-aligned) local main
git merge --no-ff origin/main -m "Merge GitLab main into GitHub main"

# 4. Push the reconciled main back to GitHub
git push github main
```

> **Note:** After running these commands, re-run `sync-gitlab-github` to push the merged result back to GitLab and finish syncing all remaining branches.

### `ansible-ssh-update`
A Python utility that dynamically creates or updates a local SSH configuration file (`~/.ssh/config.d/ansible-ssh-config`) by parsing an Ansible `hosts.yml` inventory. It automatically extracts `ansible_host`, `ansible_user`, and `ansible_ssh_private_key_file` mappings so you can natively run `ssh <hostname>` directly from your terminal.

**When to use:** When you are managing infrastructure via Ansible and want to seamlessly SSH into those same nodes without looking up their IP addresses or remembering the private key paths. See `samples/hosts.sample.yml` for an example of a supported inventory format.

### `fetch-kubeconfig`
A simple bash wrapper that securely connects to your remote Kubernetes master node, extracts the administrative `rke2.yaml` kubeconfig file, safely replaces the `127.0.0.1` loopback address with your server's reachable hostname/IP, and places it in your local `~/.kube/` directory.

**When to use:** When provisioning a new Kubernetes cluster and needing a 1-click way to grab the administrative kubeconfig securely so you can run local `kubectl` commands.

### `hermes-remote`
A one-liner that chains SSH into the Kubernetes host and `kubectl exec` into the Hermes Agent pod, activating the Python venv automatically. Supports all `hermes` subcommands: interactive REPL, one-shot mode (`-z "prompt"`), TUI (`--tui`), session management, and more.

**When to use:** When you want instant Hermes Agent CLI access from any workstation without manually chaining `ssh` → `kubectl exec` → `. activate`.

**Setup:** Create `~/.config/hermes-remote.conf` with your K8s host and SSH user:
```
K8S_HOST=your-k8s-hostname
K8S_USER=your-ssh-user
```
Alternatively, export `HERMES_K8S_HOST` and `HERMES_K8S_USER` environment variables (these take precedence over the config file).
